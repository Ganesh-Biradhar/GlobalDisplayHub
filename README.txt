Production Kiosk Setup for Raspberry Pi 3 (Debian 13 / Trixie)
Step 1: Update the system
sudo apt update
sudo apt full-upgrade -y
sudo reboot
Step 2: Install Chromium
sudo apt install chromium -y

Verify:

chromium --version
Step 3: Enable Auto Login
sudo raspi-config

Go to:

System Options
    ↓
Boot / Auto Login
    ↓
Desktop Autologin

Reboot.

Step 4: Disable Screen Blanking
sudo raspi-config

Go to:

Display Options
    ↓
Screen Blanking
    ↓
No

Reboot.

Step 5: Install Unclutter
sudo apt install unclutter -y
Step 6: Create Kiosk Folder
mkdir -p ~/kiosk
Step 7: Create Startup Script
nano ~/kiosk/start.sh

Paste:

#!/bin/bash

# Wait for desktop and network
sleep 10

# Hide mouse cursor
unclutter -idle 0.5 -root &

while true
do
    chromium \
        --kiosk \
        --incognito \
        --no-first-run \
        --disable-session-crashed-bubble \
        --disable-infobars \
        --disable-restore-session-state \
        --overscroll-history-navigation=0 \
        --disable-features=TranslateUI \
        --disable-sync \
        --no-default-browser-check \
        --disable-gpu \
        https://rfc-towards-sustainability.netlify.app/

    sleep 2
done

Save:

CTRL + O
ENTER
CTRL + X
Step 8: Make Executable
chmod +x ~/kiosk/start.sh
Step 9: Create Autostart Folder
mkdir -p ~/.config/autostart
Step 10: Create Autostart File
nano ~/.config/autostart/kiosk.desktop

Paste:

[Desktop Entry]
Type=Application
Name=Chromium Kiosk
Exec=/home/pi/kiosk/start.sh
Terminal=false
X-GNOME-Autostart-enabled=true

Save.

Step 11: Test Chromium

Before rebooting, make sure Chromium works:

chromium https://google.com

If Chromium opens normally, continue.

Step 12: Reboot
sudo reboot
Expected Boot Sequence
Power ON

↓

Wi-Fi Connects

↓

Desktop Auto Login

↓

Desktop Loads

↓

Mouse Hidden

↓

Chromium Starts

↓

Website Opens

↓

Fullscreen Kiosk

↓

If Chromium Closes

↓

Automatically Restarts
If the Browser Doesn't Start

Open a terminal and run:

/home/pi/kiosk/start.sh

If there is an error, it will be displayed in the terminal.

Verify Chromium Command

Check where Chromium is installed:

which chromium

Expected:

/usr/bin/chromium

If it returns:

/usr/bin/chromium-browser

edit ~/kiosk/start.sh and replace every occurrence of:

chromium

with:

chromium-browser
Check Autostart File
cat ~/.config/autostart/kiosk.desktop

It should display:

[Desktop Entry]
Type=Application
Name=Chromium Kiosk
Exec=/home/pi/kiosk/start.sh
Terminal=false
X-GNOME-Autostart-enabled=true
Production Checklist
✅ Raspberry Pi OS (Debian 13 / Trixie)
✅ Desktop Autologin Enabled
✅ Chromium Installed
✅ Screen Blanking Disabled
✅ Mouse Cursor Hidden
✅ Chromium Starts Automatically
✅ Full-Screen Kiosk Mode
✅ Automatic Browser Restart
✅ Boots Directly to Dashboard
One important question before you recreate the kiosk setup:

When you renamed ~/.config/autostart/kiosk.desktop to kiosk.desktop.bak and rebooted, did the Raspberry Pi desktop appear normally, or is it still showing a black screen?

The answer determines whether the issue is in the kiosk configuration or the desktop environment itself.
