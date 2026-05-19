# Setting up Sysmon on Windows

## The Why

As part of my Azure honeypot, I wanted to see if I could get more detailed logs. Remembering that Sysmon logs are more detailed, this seemed like exactly what I wanted to do. So let’s get to it! 

## The Setup

It looks like the setup is actually pretty easy (Note to self: Never say this). 

It’s a simple PowerShell script. (Note from future self, NO IT ISN'T!)

But first, I need to verify that Sysmon is not already running, type the following in PowerShell

```Get-Service sysmon*```

Assuming that it’s not already installed and running, which it will not be by default, install it with 

```Enable-WindowsOptionalFeature -Online -FeatureName Sysmon```

Haha, yeah. That didn't work. It’s never that easy!

I actually had to go to https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon and download Sysmon from there. 

---
<img width="800" height="453" alt="Screenshot 2026-05-18 at 1 18 03 PM" src="https://github.com/user-attachments/assets/6aeebfe7-a425-4170-b9e9-52139f219658" />

---

Open the Downloads folder, not the sysmon folder itself. Right-click and extract all. I put this in ```C:\Users\Sysmon``` for easy access

<img width="969" height="666" alt="Screenshot 2026-05-18 at 3 01 05 PM" src="https://github.com/user-attachments/assets/7e925631-5439-42e7-ab74-ff45425285f6" />

---
Great, now open PowerShell with admin privs and navigate to ```C:\Users\Sysmon```

Then ```ls``` to make sure all the files are there. 

<img width="935" height="870" alt="Screenshot 2026-05-18 at 3 01 22 PM" src="https://github.com/user-attachments/assets/92cf094d-98f1-4b31-8f62-7d801632a708" />
<img width="1114" height="646" alt="Screenshot 2026-05-18 at 3 02 41 PM" src="https://github.com/user-attachments/assets/e996ed19-6c4c-407e-8b6e-80305d87b0f2" />


## Sysmon Configuration

Now, it’s time to use a configuration for your sysmon logs. To use the default config, type

```sysmon -I```

However, I want to use a more robust config file, there are many online and I’m opting to use one recommended my Microsoft. 

Sysmon-modular – Modular Sysmon configuration with MITRE ATT&CK coverage maintained by Olaf Hartong
https://github.com/olafhartong/sysmon-modular

Once I found the script I wanted, ```sysmonconfig.xml``` in my case, I opened it and downloaded the raw file. 

<img width="1215" height="849" alt="Screenshot 2026-05-18 at 3 03 07 PM" src="https://github.com/user-attachments/assets/910c8df1-f9b3-439f-bb5e-1fe5dfb4401f" />
<img width="1570" height="735" alt="Screenshot 2026-05-18 at 3 03 46 PM" src="https://github.com/user-attachments/assets/fe09a733-0127-4bef-971f-9c0d52cfee2f" />

---
Move that file from the downloads into ```C:\Users\Sysmon```. Do this in powershell with 
``` mv C:\Users\USERNAME\Downloads\ sysmonconfig.xml .```

<img width="1100" height="646" alt="Screenshot 2026-05-18 at 3 04 20 PM" src="https://github.com/user-attachments/assets/141c9039-527f-4616-bd0b-608ecef47c35" />

Replace the last ```.``` With the file path you want to copy it to If you are not already in the folder you want to transfer it to.
Now, while in the ```C:\Users\Sysmon``` folder, run the command
```.\Sysmon64.exe -i .\ sysmonconfig.xml```
Click agree, wait a few seconds, and you should be all setup!

<img width="1104" height="649" alt="Screenshot 2026-05-18 at 3 04 34 PM" src="https://github.com/user-attachments/assets/d03fcd2e-fcff-400e-bbb0-a0a99eb84ab8" />


## Next Steps
Forward your logs to a SIEM, if you haven’t already.

Tune your sysmon configuration 
