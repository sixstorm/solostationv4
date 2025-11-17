# Solostation V4

Solostation Version 4 is a simple, Python and MPV Player driven media player, focusing on old school cable TV.  A random amount of commercials are chosen to play, followed by a randomly selected TV episode or movie (with this pattern repeating indefinitely).  This is not intended on perfeclty mimicking a cable TV channel (see my other Solostation repos) but rather a quick, plug and play approach.

## Requirements

Here is my setup.  If you have other hardware or a different OS, you may have different results.

- Raspberry Pi 4B
- External hard drive (mounted at /media/usb)

## Setup

### Install DietPi and Update Everything

- No Desktop Environment!
- Make sure that user 'dietpi' is set to autologin
- Make sure SSH is installed for remote management

### Create ".env" file and populate

```bash
cp env_example.txt .env

# Populate all keys needed
# Only "USB_ROOT" is needed for 'simple_schedule.py'
```

### Create mount folder for USB drive

```bash
sudo mkdir /media/usb

# Make sure to use this path as "USB_ROOT" in your ENV file
```

### Edit FSTAB

```bash
sudo blkid. # Get UUID of drive
sudo nano /etc/fstab

# Add at bottom
UUID=<UUID_GOES_HERE> /media/usb ext4 noatime,lazytime,rw 0 0
```

### Install Argon

If you have an Argon v2 case like I do, download and install the script so that the power button has "powers"
```bash
curl https://download.argon40.com/argon1.sh | bash
```

### Install Tailscale (Optional)

```bash
# Install Tailscale
sudo apt install tailscale

# Log into Tailscale - Web Authentication
sudo tailscale up
```

### Autostart Python Script

```bash
nano /home/dietpi/.bashrc

# Add at bottom
[[ -z $DISPLAY && $(tty) == /dev/tty1 ]] && exec /home/dietpi/solostationv4/venv/bin/python3 /home/dietpi/solostationv4/simple_schedule.py
```

### Reboot!

---

If you followed the steps above correctly, you should have media playing within 30 seconds.  You can use Apple Shortcuts to POST to the Pi IP address over HTTP; see Python script for details.  

### Example:

```
# Skip to next item in playlist
http://192.168.1.10:8080/cmd | POST { "command": "playlist-next"}
```