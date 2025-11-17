# Solostation V3

Solostation with a different twist.

Make sure that you have all of the requirements (requirements.txt) installed via Pip.  Simply run the command below for Solostation to build a schedule and start playback.

```bash
venv/bin/python3 main2.py
```

## UPDATE: 11/17/25

Although that Solostation v3 "works", I have created a more simple approach just to have a single channel working with no time tables or rules.  In "simple_schedule.py", you will see that a random amount of commercials are played between each randomly selected movie or episode.  

Below are my notes on how to prepare a Raspberry Pi 4b, making this script plug and play.

## Install DietPi and Update Everything

- No Desktop Environment!
- Make sure that user 'dietpi' is set to autologin

## Create ".env" file and populate

```bash
cp env_example.txt .env

# Populate all keys needed
# Only "USB_ROOT" is needed for 'simple_schedule.py'
```

## Create mount folder for USB drive

```bash
sudo mkdir /media/usb

# Make sure to use this path as "USB_ROOT" in your ENV file
```

## Edit FSTAB

```bash
sudo blkid. # Get UUID of drive
sudo nano /etc/fstab

# Add at bottom
UUID=<UUID_GOES_HERE> /media/usb ext4 noatime,lazytime,rw 0 0
```

## Install Argon

If you have an Argon v2 case like I do, download and install the script so that the power button has "powers"
```bash
curl https://download.argon40.com/argon1.sh | bash
```

## Install Tailscale

```bash
# Install Tailscale
sudo apt install tailscale

# Log into Tailscale - Web Authentication
sudo tailscale up
```

## Autostart Python Script

```bash
nano /home/dietpi/.bashrc

# Add at bottom
[[ -z $DISPLAY && $(tty) == /dev/tty1 ]] && exec /home/dietpi/solostationv3/venv/bin/python3 /home/dietpi/solostationv3/simple_schedule.py
```

## Reboot!

---

If you followed the steps above correctly, then on reboot, you should have media playing within 30 seconds.  You can use Apple Shortcuts to POST to the Pi IP address over HTTP; see Python script for details.  

### Example:

```
# Skip to next item in playlist
http://192.168.1.10:8080/cmd | POST { "command": "playlist-next"}
```