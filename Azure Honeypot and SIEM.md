# Setting up an Azure honeypot and SIEM

This is something I've wanted to do for a while now and I'm finally getting around to doing it. 
The reason for this lab is to detect real world threats, gather intel on the detected threats, and document my findings. 
At this point, I have no idea what I'm going to find. So let's get to it and see what happens!
The setup for this lab is based on a Josh Madakor video I found when researching various home labs.

## Account Creation (This shouldn't be this hard.)
To start off, I needed to setup an account with Azure. Right off the bat, I was running into trouble. Microsoft kept rejecting my attempts to make an account. 
I have no idea why, but I tried multiple emails over multiple networks and they were all blocked. 

---
<img width="1015" height="578" alt="Screenshot 2026-05-17 at 10 47 07 AM" src="https://github.com/user-attachments/assets/a641f68e-6380-450e-a3e6-8af5dd65db19" />

---

I also tried to create an account through a VPN. Surprisingly, I wasn't blocked. It asked me to click and hold the a button to
prove that I was human, but the only button I could click was the back button. I was off to a flying start here...

---
<img width="962" height="567" alt="Screenshot 2026-05-17 at 10 49 36 AM" src="https://github.com/user-attachments/assets/70353da1-2057-4882-921f-9578ee1967cc" />

---
At some point, I remembered I had an account with Skype. I was then able to use that login to gain access to Azure. 

## Azure Honeypot Setup
## Create a Resource Group
I'll start off by going to Resource Groups and clicking Create

---
<img width="1561" height="775" alt="Screenshot 2026-05-17 at 11 11 20 AM" src="https://github.com/user-attachments/assets/419c77fd-2272-4aef-bc89-6c7df95634e6" />

---
<img width="1569" height="759" alt="Screenshot 2026-05-17 at 11 15 22 AM" src="https://github.com/user-attachments/assets/53b6828e-df41-4e10-936c-fa07ca180142" />

---
Select your Azure subscription. If you just created the account, it should default to your only subscription.
Next, create a name for the Resource Group. Something like RG-SOC-Lab works here.
Then select what region you want your server to be located, ideally close to your location.

---
<img width="745" height="322" alt="Screenshot 2026-05-17 at 11 18 46 AM" src="https://github.com/user-attachments/assets/4bfefc9b-9a0a-490e-9c2e-e3c4133d2e86" />

---
Then click Review + create, then Create on the next page. 

From here, I had to refresh the Resource Group page for the new RG to show up. 

## Create a Virtual Network
Now that I've confirmed the RG, it't time to create a virtual network. It's similar to the RG. Go to Virtual Network and click Create

---
 <img width="1570" height="786" alt="Screenshot 2026-05-17 at 11 25 34 AM" src="https://github.com/user-attachments/assets/96c4caf5-c266-4689-adf3-daee6e4452a5" />

---
From here, confirm the subscription, recourse group, give the virtual network a name, and select the same region as the RG. 

---
<img width="779" height="762" alt="Screenshot 2026-05-17 at 11 30 42 AM" src="https://github.com/user-attachments/assets/a034c1f7-e85a-4d04-81be-ba5e36e081bd" />

---
Click on the Security tab and make sure nothing is checked.

---
<img width="889" height="769" alt="Screenshot 2026-05-17 at 11 37 15 AM" src="https://github.com/user-attachments/assets/4e7cead6-df09-47f9-8181-9c3bbcc01d87" />

---
Then click Review + create. It should look like this.

---
<img width="567" height="771" alt="Screenshot 2026-05-17 at 11 38 27 AM" src="https://github.com/user-attachments/assets/49016ae4-fb38-4bce-a50b-42c08de9e66d" />

---
Now I'll head back to Resource Groups

---
<img width="1563" height="783" alt="Screenshot 2026-05-17 at 11 46 31 AM" src="https://github.com/user-attachments/assets/daa90761-dce1-4f17-85d1-e6d9fb7f9ac9" />

---
Click on the RG that was created earlier, that other little guy, don't worry about that little guy.

---
<img width="1572" height="788" alt="Screenshot 2026-05-17 at 11 48 21 AM" src="https://github.com/user-attachments/assets/eaa04f27-6bc6-4a98-aa5e-d97b5a0f30f1" />

---
All of this to check and make sure the Virtual Network is showing. All good here!

---
<img width="1574" height="767" alt="Screenshot 2026-05-17 at 11 50 35 AM" src="https://github.com/user-attachments/assets/1eb8836a-352b-4dda-aa15-9e49c5e5147a" />

## Create a Virtual Machine (That's waht we're here for!)
I can only assume you know the first step here. Head to Virtual Machine and click create.

---
<img width="1567" height="747" alt="Screenshot 2026-05-17 at 11 55 39 AM" src="https://github.com/user-attachments/assets/edf451ff-ec2a-46b0-b892-624fab6f5599" />

---
From here, just select a general Virtual Machine. 

---
<img width="342" height="476" alt="Screenshot 2026-05-17 at 11 56 46 AM" src="https://github.com/user-attachments/assets/ebb3d256-06b9-4bf2-a6eb-63d5393ddac5" />

---
The setup for this VM took a bit. Mostly, I had to find the right combination of OS image and machine size.
I settled on Windows 10 Enterprise, version 22H2 - x64 Gen2 (free services eligible)
and 
Standard_D2ads_v7 - 2 vopus, 8 GiB memory

Don't forget to pick the RG that was setup earlier, set a machine name, and create a username and password. 
Also, make sure the Licensing box is ticked. Click Next

---
<img width="1337" height="717" alt="Screenshot 2026-05-17 at 12 11 45 PM" src="https://github.com/user-attachments/assets/49c0350a-b1f3-4037-a4ef-8a989a7b75a4" />
<img width="1337" height="910" alt="Screenshot 2026-05-17 at 12 16 42 PM" src="https://github.com/user-attachments/assets/78b066dd-3186-4a3a-9988-e2f62d049dd9" />

---
I kept the defaults for the Disks. Click Next.

---
<img width="829" height="879" alt="Screenshot 2026-05-17 at 12 29 12 PM" src="https://github.com/user-attachments/assets/a89e3cd7-0fc0-496d-9f08-254faca72933" />

---
Check that the Virtual network is the one we setup earlier. 
The default subnet should be just fine.
Make sure the Public IP looks good.
Go ahead and check the Delete public IP and NIC when VM is deleted box.
Click Next.

---
<img width="1006" height="877" alt="Screenshot 2026-05-17 at 12 32 10 PM" src="https://github.com/user-attachments/assets/53aae1a6-cb88-485f-81a5-5d45d75c145d" />

---
 We can skip the Management tab and click Next again.

 Go ahead and disable Boot diagnostics.
 Now we can click Review + create

 ---

 <img width="811" height="874" alt="Screenshot 2026-05-17 at 12 36 53 PM" src="https://github.com/user-attachments/assets/74260ddd-48fc-4c99-999c-fa90e1f292c8" />

This will take a few minutes, so take a break and go touch grass!

Ok, welcome back. 

The validation should be complete. Double check that you didn't select a crazy expensive machine and click Create.

---
<img width="970" height="882" alt="Screenshot 2026-05-17 at 12 41 26 PM" src="https://github.com/user-attachments/assets/1243f039-4bd4-4e36-b616-945a9b9679ae" />

---
 This takes a few more minutes. 

 and...

 Success!!!

 ---
<img width="1338" height="659" alt="Screenshot 2026-05-17 at 12 47 08 PM" src="https://github.com/user-attachments/assets/84a0e1aa-313d-4aee-a72d-fb1c56f1804e" />

---
Alright, the bulk of the work is now done, but I still have a few things to setup. 

## The Rest of the Setup (Log forwarding and Sentinel)

Back in the Azure workspace, search for "Log Analytics Workspace"

---
<img width="544" height="427" alt="Screenshot 2026-05-17 at 2 40 44 PM" src="https://github.com/user-attachments/assets/2a0ed7bd-0a94-4d03-b1e4-3f572ef9f83f" />

---
Then click create

---
<img width="1338" height="789" alt="Screenshot 2026-05-17 at 2 45 59 PM" src="https://github.com/user-attachments/assets/239018d0-5787-48b5-9b8e-dab3c9cf4b17" />

---

On this page, select the resource group that was setup earlier, give the workspace a name, and select your region. 

---
<img width="792" height="778" alt="Screenshot 2026-05-17 at 3 01 37 PM" src="https://github.com/user-attachments/assets/8d1d3863-956c-4067-acf1-27fbc3b82956" />

---

Wait for this run run its checks, then click create. 

---
<img width="737" height="780" alt="Screenshot 2026-05-17 at 3 05 18 PM" src="https://github.com/user-attachments/assets/ad7c6226-13f9-4629-910b-6e2bbfdc1258" />

---

Now wait for that to deploy. Once deployed, go back to the search bar and search "Sentinel". 

Create a Sentinel instance, this will be the SIEM.

---
<img width="551" height="335" alt="Screenshot 2026-05-17 at 3 07 58 PM" src="https://github.com/user-attachments/assets/27aa692a-238c-4767-bb23-606cf317804e" />

---

Then click create, I'm skipping the screenshot for this one, you can figure it out by now.

On the next page, select the Log Analytics Workspace that was just created. Click Add.

---
<img width="1233" height="772" alt="Screenshot 2026-05-17 at 3 09 52 PM" src="https://github.com/user-attachments/assets/dece15fa-185b-452a-8be8-23a0a0f7816f" />

---

Sentinel will take a few minutes to spin up. 

From here, I need to create a connection from Sentinel to the VM. 

Under Sentinel, go to the Content Hub

---
<img width="1330" height="817" alt="Screenshot 2026-05-17 at 3 22 46 PM" src="https://github.com/user-attachments/assets/04438533-e308-42cf-bbc6-d45cd6281ec8" />

---

Content hub is under content management. Search for “Security Event” and  Windows Security Events should come up. Select it, then click Install.

---

<img width="1343" height="812" alt="Screenshot 2026-05-17 at 3 25 30 PM" src="https://github.com/user-attachments/assets/5f4a0b44-9e3c-4bba-95dd-17cbacdc15c8" />

---

Once installed, click Manage

---
<img width="1338" height="785" alt="Screenshot 2026-05-17 at 3 28 56 PM" src="https://github.com/user-attachments/assets/518f24f9-693d-4718-96ba-8b38ea326ed9" />

---
From here, tick the box for Windows Security Events at AMA.

---
<img width="1115" height="755" alt="Screenshot 2026-05-17 at 3 41 19 PM" src="https://github.com/user-attachments/assets/d36e7724-11d2-4441-8a7c-995207f799a6" />

---
Then scroll down and click on Open Connector Page

---

<img width="1166" height="786" alt="Screenshot 2026-05-17 at 3 44 50 PM" src="https://github.com/user-attachments/assets/89542394-4edc-423b-94e9-9f51bcb12e30" />

---

Find +Create Data Collection Rule and click it.

---

<img width="1149" height="769" alt="Screenshot 2026-05-17 at 3 49 10 PM" src="https://github.com/user-attachments/assets/8c9d0e52-4638-4b51-850b-2b5f9e1956c4" />


---

Give it a name, check the subscription and resource group, then click Next.

---
<img width="626" height="766" alt="Screenshot 2026-05-17 at 3 50 23 PM" src="https://github.com/user-attachments/assets/ef9a46c1-2708-4ec4-b813-aa78e53fde31" />

---
Select your VM and click next.

---
<img width="622" height="548" alt="Screenshot 2026-05-17 at 3 52 07 PM" src="https://github.com/user-attachments/assets/fb23d9a3-0632-4f00-ad0d-e2f8d9bb5690" />

---
Next again and then Create. 

---
<img width="620" height="771" alt="Screenshot 2026-05-17 at 3 53 39 PM" src="https://github.com/user-attachments/assets/3652a747-7be1-4b90-a031-0c5637e5dd0e" />

---

Now, go to the VM, under Settings > Extensions + applications. The Monitoring Agent should show up now. 

---
<img width="1509" height="802" alt="Screenshot 2026-05-17 at 3 56 53 PM" src="https://github.com/user-attachments/assets/b3c1f2dc-fdca-4df1-a695-94a2e9657e03" />

---

## And then...

I have to admit, I wasn't expecting what happened next since I have not opened up the firewall. The only open port is 3389 for RDP. I know there is a screenshot showing ports 22, 80, and 443 are also open, but I decided to only have 3389 open to start. 

Head back to the Log Analytics Workspace > LOG-Analytics-SOC-LAB > Logs > KQL mode, then search SecurityEvent.

---

<img width="1333" height="805" alt="Screenshot 2026-05-17 at 4 04 47 PM" src="https://github.com/user-attachments/assets/0fd64694-b0af-4b0f-8627-6e5fc2d634f9" />

---

I already have 1000 logs maxed out on my search. I changed it to MAX logs and it returned 14,446.

By the time I was able to type out a search filter for EventID 4625 (Windows event id for failed login attempts) it was up to 16,627!

---
<img width="694" height="669" alt="Screenshot 2026-05-17 at 4 13 15 PM" src="https://github.com/user-attachments/assets/9cfdae95-9157-471b-b20c-d09a228df5dd" />

---
For a frame of reference, I changed the search to filter out the EventID 4625. It came back with 23 logs. Wow...

---
<img width="699" height="673" alt="Screenshot 2026-05-17 at 4 18 08 PM" src="https://github.com/user-attachments/assets/09f1eb90-df27-4732-9802-f31a028fc91c" />

---

I think this will do it for now. I'm going to play around with this. Open up different ports, disable firewalls, and whatever else I can think of. This should make for some interesting research. 

