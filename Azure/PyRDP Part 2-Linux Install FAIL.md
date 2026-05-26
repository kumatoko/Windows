PyRDP

# PyRDP (This does not work, this is not a tutorial. Just a documentation of my failures..)

This is a documentation of my PyRDP setup on an cloud based honeypot running in an Azure Instance. 

To start off, I found out about PyRDP while working with Claude to see what and how I can enumerate the attackers attempting to brute force my Azure honeypot. 
I’m curious to learn what the most common attack credentials are. I assume it will align well with the rockyou.txt wordlist.

According to PyRDP, you can capture plaintext creds or NTLM hashes. I think it would be a fun experiment to capture the NTLM creds and attempt to crack them. 
I have access to a purpose-built computer for password cracking and I’d like to give it a shot. 
Before any of that can take place, I have to get PyRDP installed on the honeypot. 




## THIS ACTUALLY WORKS (I hope…)

Alright, so attempting to install this on a proxy machine now. To start off, I’ll need a second machine. I’ll spin up a new VM in Azure. 
It’s mostly the same process as spinning  up the Azure Honeypot machine, except I’m using a Kali box for this. Why Kali? Because…why not. 

For settings, make sure it’s in the same resource group and region as the honeypot. 
Name the machine, select your SSH login option (I chose SSH Keys), and make sure port 22 is open.

---

<img width="780" height="1392" alt="Screenshot 2026-05-20 at 7 20 30 AM" src="https://github.com/user-attachments/assets/f7cb4acc-926a-4b23-af5c-dcc30c532f10" />
<img width="779" height="289" alt="Screenshot 2026-05-20 at 7 22 33 AM" src="https://github.com/user-attachments/assets/fd9a87ad-468c-45c0-aa76-a345ec65eaf7" />

---

On the networking tab, select the same Virtual Network as the honeypot and make sure it’s on the same subnet.

---
<img width="799" height="1133" alt="Screenshot 2026-05-20 at 7 24 39 AM" src="https://github.com/user-attachments/assets/9dbc6507-77e0-4f02-ba5f-a713113f0731" />


---

Disable Boot Diagnostics under monitoring. Then it’s ready to create. 

---
<img width="707" height="795" alt="Screenshot 2026-05-20 at 7 25 37 AM" src="https://github.com/user-attachments/assets/1903b6e0-5268-46b5-9f00-14556d62a08e" />


---

Double check the pricing and click create. There should be a popup to download the SSH keys here. Do that! 

---
<img width="863" height="798" alt="Screenshot 2026-05-20 at 7 28 21 AM" src="https://github.com/user-attachments/assets/2863f6fe-a3a8-4b17-9830-1f03d56d1058" />


---

I’ll go ahead and move the SSH keys out of my downloads folder. 

<img width="611" height="78" alt="Screenshot 2026-05-20 at 10 55 37 AM" src="https://github.com/user-attachments/assets/4d35d1cd-3cf1-447b-811a-9cc2887a46ad" />


Now, I need to SSH into the Kali box with the SSH keys. 
```
ssh -I KEYFILE.key soclabproxyuser@22.2.222.222
```

<img width="712" height="363" alt="Screenshot 2026-05-20 at 10 58 31 AM" src="https://github.com/user-attachments/assets/d19ee398-9af4-40b6-9e72-cce52285f5be" />


OOPS, looks like there’s an issue. Running an ```ls``` shows me the permissions. A quick google search tells me that I need to restrict permissions even more with a simple command. 
```
chmod 400 KEYFILE.key
```

Easy enough, now I’ll try to SSH again. Same command as above. 

And…SUCCESS!!

<img width="779" height="378" alt="Screenshot 2026-05-20 at 10 58 58 AM" src="https://github.com/user-attachments/assets/45cab40a-97f9-4c88-89c3-1fa3a40997e0" />


First thing to check is a connection to the honeypot through the local network. So I’ll ping the honeypot’s private IP address with ```ping 10.0.0.4```

<img width="513" height="271" alt="Screenshot 2026-05-20 at 11 01 06 AM" src="https://github.com/user-attachments/assets/690d4247-8ffb-42a7-9e5b-22de00338931" />


Whoa! Two things in a row worked!!! This is great.

This means that these two boxes are on the same private network, so I should be able to use the kali box as a proxy. 

But first, I want to go add a firewall rule to restrict traffic to the Kali box. I’ll check this at 
```
Resource groups > RG-SOC-Lab > COPR-NET-WEST-2-PROXY-nsg
```

I’m just now noticing that I mistyped CORP, it’s fine, this is fine.

---
<img width="1271" height="694" alt="Screenshot 2026-05-20 at 11 20 27 AM" src="https://github.com/user-attachments/assets/5651ab12-d579-46db-a828-4a89f30dd411" />

---

This is on a different Network Security Group than the honeypot. Great, nothing else to do here.

NOW I should be able to get PyRDP up and running.

I’ll start off with the good ol’ ```sudo apt-get update```

And….. dang it…

<img width="1068" height="532" alt="Screenshot 2026-05-20 at 11 42 03 AM" src="https://github.com/user-attachments/assets/8fab7024-abc0-4a99-bb13-95d61ee0b534" />


Ok, a quick ```man apt-secure``` should reveal the answer.

Since that isn’t there either, I’ll resort to Claude to diagnose this issue. The results seem simple enough.

<img width="725" height="567" alt="Screenshot 2026-05-20 at 11 39 06 AM" src="https://github.com/user-attachments/assets/2340aeb4-75cf-4788-9378-3843a3127b3d" />


Running the first command and trying the update again gave me the same results. 

<img width="1072" height="397" alt="Screenshot 2026-05-20 at 11 43 33 AM" src="https://github.com/user-attachments/assets/9b0778a4-f5c4-4eb3-ac5c-89960c45fad8" />


Ok, perhaps I can spin up a local copy of Kali and see if the ```/usr/bin/sqv``` file is something I can copy and paste over. 
But first, I’ll make a backup of the sqv file, just incase this goes south and I need to restore. (Spoiler ALLERT! It goes south and I needed to restore)

<img width="567" height="222" alt="Screenshot 2026-05-20 at 11 51 21 AM" src="https://github.com/user-attachments/assets/bcc43822-4292-44c5-8028-0439ac52e188" />

<img width="652" height="658" alt="Screenshot 2026-05-20 at 11 58 57 AM" src="https://github.com/user-attachments/assets/fc8eb84e-2600-4014-9fbc-d968162d2232" />


No luck with the copy and paste. I’m not sure why I’m unable to copy from the nano doc, but that’s a problem for another day. 
 
I’ll try option 2 from Claude. 
```
sudo wget https://archive.kali.org/archive-key.asc -O /etc/apt/trusted.gpg.d/kali-archive-keyring.asc
```

This seems to have worked! I can now upgrade. 

<img width="1049" height="481" alt="Screenshot 2026-05-20 at 11 59 32 AM" src="https://github.com/user-attachments/assets/8a475a57-e235-436c-ac1c-fd4cadcfebef" />


Ok, back to trying to install pip

I’ll start with 
```
sudo apt install python3 python3-pip python3-venv \
        build-essential python3-dev openssl \
        libegl1 libxcb-cursor0 libxkbcommon-x11-0 libxcb-icccm4 libxcb-keysyms1 \
        libnotify-bin \
```

Then I’ll add 
```
Sudo apt-get install libavcodec62
```

Next, 
```
Sudo apt install python3-pip
```

Then 
```
python3 -m pip install --user pipx
```

<img width="1065" height="485" alt="Screenshot 2026-05-20 at 12 14 47 PM" src="https://github.com/user-attachments/assets/d22efc22-3859-44ed-a94d-7e10ece9e199" />

Here, I ran into a little trouble. Given the option to ```—break```, yes, let’s break things!

```
python3 -m pip install --user pipx —break-system-packages
```

<img width="825" height="403" alt="Screenshot 2026-05-20 at 12 15 15 PM" src="https://github.com/user-attachments/assets/250873a9-8326-4658-b413-b2497bb5fcf6" />

Great, that worked! Now I’ll check with ```python3 -m pipx ensurepath```

<img width="1020" height="196" alt="Screenshot 2026-05-20 at 12 16 28 PM" src="https://github.com/user-attachments/assets/09093a50-c99b-49f3-adbb-6413f2bc986d" />


I can only assume this means everything is working. After a day and a half of figuring this out, I’m nearly ready to install PyRDP. And they said cybersecurity is fun. :P

Alright, time to install PyRDP. Fingers crossed.

<img width="789" height="421" alt="Screenshot 2026-05-20 at 12 24 07 PM" src="https://github.com/user-attachments/assets/6b711b7e-15b6-48c7-aa5e-d56eb39c1d62" />


The first attempt came back with an error. So I ran the command it described and attempted to install PyRDP again. 

<img width="1001" height="294" alt="Screenshot 2026-05-20 at 12 25 08 PM" src="https://github.com/user-attachments/assets/92cfa461-c427-420a-bc34-5415287a33cf" />


It looks like I’m still getting some of the same errors I was getting during my Windows install attempt. At this point, I’m feeling defeated. I need to take a break and come back another day. Argh..



## Links

Great video form the creator of PyRDP. - youtube.com/watch?v=e-Q4pYf9-oE

GitHub Repo - https://github.com/GoSecure/pyrdp

