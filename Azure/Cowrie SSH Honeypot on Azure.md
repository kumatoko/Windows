# Cowrie SSH Honeypot on Azure

After gathering over 120,000 login attempts through RDP, I wanted to see if I could gather more info. Try as I might, I was unable to get PyRDP installed and running to gather more info on the RDP attempts. Through a little more research, I came across Cowrie.

Cowrie fit the bill for what I was looking for (Linux instead of Windows based), the ability to gather login creds (usernames and passwords!). Also, you can login with any username:password combo, which is great. 
<img width="910" height="716" alt="Screenshot 2026-06-08 at 11 08 20 AM" src="https://github.com/user-attachments/assets/88477641-7645-4ca9-9751-664e24963d20" />


Since the threat actorc can login, it also gives me the ability to see what happens next. It also has the added ability to accept uploads, then add a hash value for the uploaded file to the log. This adds depth to the amount of research I can do for login attempts. 

I found the setup to be somewhat straightforward. The README seems to be missing a few steps, or maybe I'm special and I needed to add a few extra steps. Anyway, here is how I was able to get it up and running. 


---

## The Setup

I used the git method for my instance.
First things first, create a folder to download Cowrie within my home directory.
```
mkdir cowrie && cd cowrie
```

Next, git it!
```
git clone https://github.com/cowrie/cowrie .
```
Go ahead and spin up a virtual environment
```
python3 -m venv cowrie-env
```
Then activate the virtual environment
```
source cowrie-env/bin/activate
```

You should see a change in the terminal showing the virtual environment

<img width="409" height="123" alt="Screenshot 2026-06-08 at 9 25 51 AM" src="https://github.com/user-attachments/assets/0476fa98-8506-420d-a323-fa5e9e521649" />


Next, make sure pip is installed and upgraded
```
pip install --upgrade pip
```
Then install the requirements with 
```
pip install -r requirements.txt
```
Here is where it started straying from the instructions I found. The initial instructions say to run this command
```
cp /etc/cowrie.cfg.dist /etc/cowrie.cfg     #This is not where I found it
```
However, ```/etc/cowrie.cfg.dist``` does not exist. Go ahead and search for it with 
```
find ~ -name cowrie.cfg.dist
```
It should be in something like ```/home/user/cowrie/src/cowrie/data/etc/cowrie.cfg.dist```
So I updated the copy command
```
cp /home/user/cowrie/src/cowrie/data/etc/cowrie.cfg.dist /home/user/cowrie/etc/cowrie.cfg
```
I'm actually still not sure if it's reading the file from here or /etc, so I copied it there too.
```
cp /home/user/cowrie/src/cowrie/data/etc/cowrie.cfg.dist /etc/cowrie.cfg
```
The next step is to redirect port 22 to 2222 in iptables. But first, make sure you don't lock yourself out of SSH. Do this by adding a non-standard port to your SSH config File.

## Change your SSH port
Go ahead and change your SSH port so you can still login to the actual machine. I wasn't sure if the iptables redirect was for the real SSH or the cowrie SSH. To be on the safe side, I just changed the port manually. 

This is a pretty simple process. 
Edit the SSH config file with 
```
sudo nano /etc/ssh/sshd_config
```
Change the port you use to login to something non-standard. Be sure to make a note of it.

<img width="612" height="397" alt="Screenshot 2026-06-08 at 9 57 53 AM" src="https://github.com/user-attachments/assets/ac2ceed5-83d0-4800-84bb-a42bdca56b4c" />


CTRL+X, save and close. 

Now, restart SSH with 
```
systemctl restart ssh
```

If you're running on Azure or other cloud service (or behind a firewall for that matter), make sure you allow connections through the new port. For me, I added a new firewall rule through Recourse Group -> RG-SOC-lab -> CORP-NET-WEST-2-PROXY-nsg -> Settings -> Inbound Security Rules -> Add.

---
<img width="575" height="835" alt="Screenshot 2026-06-08 at 10 06 41 AM" src="https://github.com/user-attachments/assets/82be290e-4e98-4cbb-a8a1-bcbee241b268" />


---
DO NOT LOG OUT OR CLOSE THE TERMINAL WINDOW!

Open a new terminal window and make sure you can login through the new port. 
Be sure to add your new port to the SSH command ```-p 2200```

If you can login, you're golden. If not, you missed something along the way. Usually for me, it's restarting the SSH service or forgetting the add the port to the SSH login command.

## Back to the Setup
The next step is to redirect port 22 to 2222 in iptables. 

### (This might be different for you. I had to use nft instead of iptables on a Raspberry Pi.)

Start off by running 
```
sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
```
Then, make it persistent with 
```
sudo apt install -y iptables-persistent
```
Then finalize it with 
```
sudo netfilter-persistent save
```

We're getting there, I promise...

Make sure you're still in the virtual environment and in your ```~/cowrie``` folder. Theoretically you should be able to fire up Cowrie from here with 
```
/bin/cowrie start
```

This did not work for me. A quick look in the ```cowrie/bin``` showed that the cowrie was not in there. A little digging, and some help from Claude, I realized that things were not quite right. Some of the required packages did not install.

I had to run 
```
pip list | grep -i twisted
```
This showed no results, telling me that twisted was not installed. I was able to install it with 
```
pip install twisted -force-reinstall
```
I then tried to start Cowrie again with no luck. More troubleshooting led me to the following. I looked to see if cowrie installed. 
```
pip list | grep -i cowrie
```
No results...
It did not. I feel like this was a big failure on the part of the README from Cowrie. Or maybe this is second nature for most people and I'm not familiar enough to know it. 

I ran the following , from the ```~/cowrie``` folder to install Cowrie
```
pip install .
```
Now, Cowrie showed when I ran ```pip list```
However, ```~/cowrie/bin/cowrie``` still did not exist. I was able to add it manually by creating the file
```
nano bin/cowrie
```
Then I added the following
```
#!/bin/sh
COWRIE_HOME=$(dirname $(dirname $(readlink -f "$0")))
COWRIE_PYTHON=${COWRIE_HOME}/cowrie-env/bin/python
cd ${COWRIE_HOME}
exec ${COWRIE_PYTHON} -m cowrie.scripts.cowrie "$@"
```
Save and close that, then change your file permissions with 
```
chmod +x bin/cowrie
```
Fingers crossed, I ran the command to start cowrie again.
```
bin/cowrie start
```

And it worked!!! 

Double check it with 
```
bin/cowrie status
```

If you're still not getting it to work, make sure you're running ```bin/cowrie``` and not ```/bin/cowrie```. Its a subtle difference, but it makes a difference. Ask me how I know. 

---

## Test the Logs
I wanted to make sure it was functional from a logging perspective, so within the virtual environment, open the logs with
```
tail -f ~/cowrie/var/log/cowrie/cowrie.log
```
This will show live updates to the log file. With that open, open a new terminal window. then SSH into your machine again. You can use any username and password.
```
ssh donut@123.32.132.123
```
No need to specify a port. You should see the login attempt live (with creds) on your log file. 

 <img width="1459" height="454" alt="Screenshot 2026-06-08 at 11 03 40 AM" src="https://github.com/user-attachments/assets/78b405af-5701-422e-9c34-a1e236226e2d" />


Woohoo!!! It works! 
