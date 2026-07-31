If your goal is:

When Raspberry Pi 3 powers ON → Automatically boot → Connect to Wi-Fi → Open a web link in full-screen automatically (Kiosk Mode).

Follow these steps.

Step 1: Install Raspberry Pi OS

Install:

✅ Raspberry Pi OS (64-bit) Desktop

Step 2: First Boot

Connect

HDMI Display
Keyboard
Mouse
Wi-Fi

Complete

Language
Country
Timezone
Password
Wi-Fi
Step 3: Update Raspberry Pi

Open Terminal.

sudo apt update
sudo apt full-upgrade -y
sudo reboot
Step 4: Install Chromium

Normally already installed.

Check:

chromium-browser --version

or

chromium --version

If not installed:

sudo apt install chromium-browser -y
Step 5: Disable Screen Sleep

Open

Menu
→ Preferences
→ Raspberry Pi Configuration

Disable

Screen blanking

or

Terminal

sudo raspi-config

Navigate to:

Display Options

↓

Screen Blanking

↓

No
Step 6: Disable Power Saving

Edit

sudo nano /etc/xdg/lxsession/LXDE-pi/autostart

Add

@xset s off
@xset -dpms
@xset s noblank

Save

CTRL+O

ENTER

CTRL+X
Step 7: Create Autostart Directory
mkdir -p ~/.config/autostart
Step 8: Create Chromium Autostart
nano ~/.config/autostart/kiosk.desktop

Paste

[Desktop Entry]
Type=Application
Name=Chromium Kiosk
Exec=chromium-browser --kiosk --incognito --noerrdialogs --disable-infobars --disable-session-crashed-bubble --check-for-update-interval=31536000 https://YOUR_LINK_HERE

Example

Exec=chromium-browser --kiosk --incognito https://google.com

or

Exec=chromium-browser --kiosk http://192.168.1.10:8000

Save.

Step 9: Hide Mouse Cursor

Install

sudo apt install unclutter -y

Edit

nano ~/.config/autostart/kiosk.desktop

Example

[Desktop Entry]
Type=Application
Exec=sh -c "unclutter -idle 0.5 & chromium-browser --kiosk --incognito https://YOUR_LINK"
Step 10: Auto Login

Open

sudo raspi-config

Go to

System Options

↓

Boot / Auto Login

↓

Desktop Autologin
Step 11: Reboot
sudo reboot

After reboot

Power ON

↓

Desktop loads

↓

Chromium starts

↓

Website opens

↓

Fullscreen

↓

No mouse

↓

No keyboard required
Optional: Disable Chromium Error Messages

Use:

chromium-browser \
--kiosk \
--incognito \
--disable-infobars \
--disable-session-crashed-bubble \
--no-first-run \
--disable-restore-session-state \
--overscroll-history-navigation=0 \
https://YOUR_LINK
Optional: Automatically Restart Chromium if it Crashes

Create a launcher script:

mkdir -p ~/kiosk
nano ~/kiosk/start.sh

Paste:

#!/bin/bash

while true
do
    chromium-browser \
    --kiosk \
    --incognito \
    --disable-infobars \
    --no-first-run \
    https://YOUR_LINK

    sleep 2
done

Make it executable:

chmod +x ~/kiosk/start.sh

Then change the autostart entry:

Exec=/home/pi/kiosk/start.sh

This will automatically relaunch Chromium if it exits unexpectedly.

Production Setup Checklist
✅ Raspberry Pi OS (64-bit)
✅ Auto Login enabled
✅ Chromium installed
✅ Kiosk Mode
✅ Full-screen browser
✅ Screen blanking disabled
✅ Power saving disabled
✅ Mouse cursor hidden
✅ Browser restarts automatically if it crashes
✅ Starts automatically on every power-on

This setup is suitable for digital signage, dashboards, industrial HMIs, kiosk displays, and web-based control panels.