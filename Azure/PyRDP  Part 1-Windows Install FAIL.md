# PyRDP (This does not work, this is not a tutorial)

This is a documentation of my PyRDP setup on an cloud based honeypot running in an Azure Instance. 

To start off, I found out about PyRDP while working with Claude to see what and how I can enumerate the attackers attempting to brute force my Azure honeypot. 
I’m curious to learn what the most common attack credentials are. I assume it will align well with the rockyou.txt wordlist.

According to PyRDP, you can capture plaintext creds or NTLM hashes. I think it would be a fun experiment to capture the NTLM creds and attempt to crack them. 
I have access to a purpose-built computer for password cracking and I’d like to give it a shot. 
Before any of that can take place, I have to get PyRDP installed on the honeypot. 

## The Setup

Reading the README.md on GitHub has a great description on the install. This particular install will be on Windows, so we’ll start there. 

The readme recommends installing Scoop to make this install easy. 
It seems like at a minimum, have Python installed too. 

---
<img width="1198" height="444" alt="Screenshot 2026-05-19 095934" src="https://github.com/user-attachments/assets/c9c373cf-0696-4040-ba27-c974d1e75dda" />

---

I did have a little trouble getting Scoop installed, although it wasn’t too bad. Just follow the link and there’s a Quickstart guide. 

---
<img width="1698" height="326" alt="Screenshot 2026-05-19 100010" src="https://github.com/user-attachments/assets/b6f8ab33-22c5-42d1-ab55-dd9b807c5113" />

---

For me, the Powershell script didn’t work. 

I was getting an error that I couldn’t run this without admin, funny given that I am admin. Whatever, let’s see what else I can do. 

<img width="1158" height="312" alt="Screenshot 2026-05-19 095629" src="https://github.com/user-attachments/assets/aa41d00e-1dc8-406d-9e27-be7540ec45c4" />

There’s a link in the error with terrible color choices, that seems like a good place to start. 
This takes me to the Scoop (un)installer page. 

I went straight to the For Admin section and used the command ```irm get.scoop.sh -outfile 'install.ps1'
.linstall.ps1 -RunAsAdmin [-OtherParameters ...] ```

---
<img width="1198" height="533" alt="Screenshot 2026-05-19 095853" src="https://github.com/user-attachments/assets/561a95a4-8a03-4721-8604-0e3e8fb3c2de" />

---

I thought there might be something off with the ```[-OtherParameters …]``` but I tried it anyway. And surprise surprise, it threw back an error. 

<img width="1123" height="191" alt="Screenshot 2026-05-19 095725" src="https://github.com/user-attachments/assets/acd3a032-ed95-4fa7-8335-e22e9d3a66e9" />


Ok, so I’ll go one command at a time. ```irm get.scoop.sh -outfile 'install.ps1```

After an ```ls``` It looks like the install file has downloaded, so I’ll run the second line again without the ```[-OtherParameters …]```

<img width="438" height="175" alt="Screenshot 2026-05-19 095752" src="https://github.com/user-attachments/assets/ce3e1f58-e5ce-412b-bcc8-b79b52d738bd" />

This seems to have worked just fine. It looks like Scoop is now installed. 

At this point I’ll head back to the PyRDP Github to continue the install. 

So I’ll make sure Python is installed with 
``` scoop install python```

<img width="1109" height="525" alt="Screenshot 2026-05-19 100405" src="https://github.com/user-attachments/assets/61dca4d2-3673-4995-9454-10d136f88ec5" />

Then..
```scoop install pipx```

<img width="1110" height="167" alt="Screenshot 2026-05-19 100330" src="https://github.com/user-attachments/assets/1f4eadbc-3fbc-4101-b304-b4532d7ff365" />

Then..
```pipx ensurepath```

<img width="1110" height="193" alt="Screenshot 2026-05-19 100449" src="https://github.com/user-attachments/assets/9f334595-3afe-404c-8119-2f430bfc3b15" />


Then I’ll go ahead and logout, then in again.


From here, head back into PoerwShell. I’ll continue with the regular install with the command

```pipx install pyrdp-mtim[full]```

<img width="1141" height="364" alt="Screenshot 2026-05-19 100539" src="https://github.com/user-attachments/assets/78177ebf-c6cd-4806-acbb-8874f083e774" />

---

From here, you should be good to go! Theoretically, in my case, it didn’t work. I’m shocked..

<img width="1122" height="327" alt="Screenshot 2026-05-19 100758" src="https://github.com/user-attachments/assets/c31d00ea-efba-4a2b-b4d4-1eb84b8db5ad" />

Ok, so I need to figure this out now. Let’s GO!!!

I tried a few more things, but I could not get this to work. Including installing "Microsoft C++ Build Tools" and attempting to install Docker, and run this PyRDP through Docker. 
I was unable to get that to work either. 

Looking through the documentation, it looks like PyRDP needs to be installed on a second machine, basically a proxy that intercepts the RDP traffic. 
Perhaps I missed this, but I assumed it would be on the machine behind accessed. 
Thinking through how it works, I guess it makes sense not to have PyRDP on the same machine being hacked. 

---
More to come...
-

