## How to bypass network login on Windows 11 setup

I've run into this a few times now while setting up Windows machines for otehrs. 
Thankfully, the solution is simple.

To start off, make sure you don't have an ethernet cable connected.

Turn on the machine and go through the setup as usual.

<img width="691" height="495" alt="Screenshot 2026-05-16 at 8 25 44 AM" src="https://github.com/user-attachments/assets/56541b1e-fd65-45e3-bf56-3640a9af8be1" />

<img width="689" height="497" alt="Screenshot 2026-05-16 at 8 25 51 AM" src="https://github.com/user-attachments/assets/9e3b19a8-7d0e-416f-85c5-a778b167935b" />

Skip the second keyboard setup, if asked.

When you get to this screen...

<img width="685" height="496" alt="Screenshot 2026-05-16 at 8 26 02 AM" src="https://github.com/user-attachments/assets/6a6ae7d7-0f5b-48bc-9a2c-4ef157191ca9" />

... It looks like you can't do anything without connecting to the internet, but you can!
### Press ```Shift + F10```
this should pop open a command line window

From here, simply type
### ```OOBE/BYPASSNRO```
and hit ENTER

<img width="728" height="502" alt="Screenshot 2026-05-16 at 8 26 15 AM" src="https://github.com/user-attachments/assets/f72de075-35ab-40b1-8911-23e12d665285" />

From here, the machine will restart itself and take you back to the start of the setup, annoying, I know..

Now you can go through the full setup. When you get to the network page, you'll see an option to skip by clicking I don't have internet

<img width="691" height="498" alt="Screenshot 2026-05-16 at 8 26 28 AM" src="https://github.com/user-attachments/assets/4ecb4973-0c60-405d-99ce-34f5146bb22b" />

It'll then threaten you with a limited setup if you don't connect. It'll be fine, you can continue.

<img width="687" height="495" alt="Screenshot 2026-05-16 at 8 26 34 AM" src="https://github.com/user-attachments/assets/cef25ca7-b4ac-4a0f-be70-9ee5d25d576c" />

From here, finish your setup as usual and you should be good to go! 
