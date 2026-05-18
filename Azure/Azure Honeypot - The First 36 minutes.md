# Azure Honeypot - The First 36 minutes.

## Stats at a glance

Total logs collected - 33,735

Total Failed Logins - 33,710

Timeframe - 22:54:40 – 23:30:39 UTC

MITRE ATT&CK ID: T1110.004 - Brute Force: Credential Stuffing

## Findings

At first, the number is alarming. Don’t get me wrong, more than 33k login attempts in in 30 minutes is nothing to walk away from. After further analysis it doesn’t seem quite at threatening. 

It turns out that out of the 33,710 failed logins (Windows EventID 4625), they all came from 2 sources over 4 IP addresses. 3 IPs are from the 185.156.73.0/24 range, and the other is from 92.63.197.69.

Digging into these IP addresses a little further, we find that the 3 IPs from the 185.156.73.0/24 range (185.156.73.24, 185.156.73.74, and 185.156.73.169) all belong to Khnykin Vitaliy Yakovlevich out of Ukraine. 

The 92.63.197.69 IP is based out of Russia, and my research links the IP to Petersburg Internet Network Ltd. It’s noted that this IP frequently abuses RDP attacks. 

The most frequent username attempted is administrator, with 1,358 attempts. A few attempted usernames were successful, guest and defaultuser. These are built in accounts that are disabled, so no access attempts were successful. 

## Steps to remediate

The best thing to remediate this attack would be to close port 3389. Assuming the port needs to remain open, the next best steps will be to block both IP ranges (185.156.73.0/24 and 92.63.197./24) at the firewall. The most important thing to remember here is to have this rule as a higher priority than the RDP allow rule. 

<img width="571" height="898" alt="Screenshot 2026-05-18 at 9 12 39 AM" src="https://github.com/user-attachments/assets/813599c6-534d-4003-a333-f052449fcbc4" />


Additional controls would be to ensure strong password policies, enforce account lockout policies, and enable MFA for all accounts.


## Log Stat Visualization 

Using an assist from AI, I was able to create a helpful visualization of the data. 

<img width="764" height="1268" alt="Screenshot 2026-05-18 at 9 24 37 AM" src="https://github.com/user-attachments/assets/2aacfafc-8e6d-4fea-be67-65085d7d11cd" />

## My Next Steps

Since this is a lab, and not a production environment, I want to see how far I can push this and how much more information I can gather. Next steps will include:

  Enabling Sysmon for more detailed logs
  
  Enabling credential logging through RyRDP for password analysis
  
  Running the test machine for 24 hours




