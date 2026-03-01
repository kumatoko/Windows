## What to do if Member of Domain is greyed out.

I ran into this issue when setting up a home lab, trying to connect Windows 11 Enterprise to a Domain Controller. Luckly, it's a pretty simple fix. 

Start off by opening the Control Panel.

<img width="1264" height="791" alt="Screenshot 2026-03-01 at 9 55 27 AM" src="https://github.com/user-attachments/assets/fc8f7020-36a0-4a58-b6db-cd685d577f21" />

Go to Programs.

<img width="1271" height="793" alt="Screenshot 2026-03-01 at 9 58 24 AM" src="https://github.com/user-attachments/assets/f90d5d71-0604-4b8d-b60f-c2c27056e56c" />

Go to Programs and Features.

<img width="1268" height="792" alt="Screenshot 2026-03-01 at 10 00 19 AM" src="https://github.com/user-attachments/assets/6d63129d-5f07-4780-9dcf-fe47a217d0ec" />

Click on Turn Windows features on or off.

<img width="1275" height="795" alt="Screenshot 2026-03-01 at 10 02 42 AM" src="https://github.com/user-attachments/assets/3624022e-1f66-44a4-8a9a-411d1a3d087f" />

You may have to put in admin credentials here

Now scroll down to SMB and tick SMB 1.0/CIFS Client

<img width="1273" height="796" alt="Screenshot 2026-03-01 at 10 06 15 AM" src="https://github.com/user-attachments/assets/0020d90d-fc6a-421e-ad7e-ded53a83ed13" />

Click ok, then you'll be asked to restart your computer. Restart to enable the changes. 


After restart, open the advanced system settings by searching "system settings"

<img width="1272" height="793" alt="Screenshot 2026-03-01 at 10 14 24 AM" src="https://github.com/user-attachments/assets/2054bbc2-b7ea-43be-a59d-26188e706486" />

Navigate to the Computer Name tab and click Change

<img width="1272" height="795" alt="Screenshot 2026-03-01 at 10 15 48 AM" src="https://github.com/user-attachments/assets/28d8fb93-3be0-47b4-b12a-51310044640c" />

You should now be able to be a member of a Domain. 

<img width="1270" height="790" alt="Screenshot 2026-03-01 at 10 17 31 AM" src="https://github.com/user-attachments/assets/7b8b96ef-3ab2-47a5-bf29-c21e18c65083" />

If it is still greyed out, check to make sure you're not running a Home version of Windows. Windows Home will still let you run through these steps, but Domain will remain greyed out. 

Restart to make sure everything takes effect. You should now be able to login to the domain by selecting Other user and using your domain credentials.

<img width="1264" height="788" alt="Screenshot 2026-03-01 at 10 20 56 AM" src="https://github.com/user-attachments/assets/1485eef9-7c7d-43fb-b7db-95d91f3b1a05" />

