![Alt text](images/node.png?raw=true "Title")

[![YouTube Channel Views](https://img.shields.io/youtube/channel/views/UCz5BOU9J9pB_O0B8-rDjCWQ?label=YouTube&style=social)](https://www.youtube.com/channel/UCz5BOU9J9pB_O0B8-rDjCWQ)

# nodemode
Instructions to get a [Raspiblitz bitcoin node](https://github.com/rootzoll/raspiblitz) to run the tickerXL price ticker code ([here](https://github.com/llvllch/stonks))

# Prerequisites
A raspiblitz node, up and running. If you're building from scratch, here's the shopping list for the components we used: 

- Raspberry Pi 4 (4 or 8 Gb)
- Heatsink
- 1 TB SSD
 
# Instructions

## Login to your node 

Login over ssh.
`ssh admin@<YOURNODEIP>`
Using your node login password

Edit `/home/pi/.bashrc` and comment out the last line of it.

Exit, and ssh back in as the user pi. `ssh pi@<YOURNODEIP>`

## Install the ticker code 

Following the instructions at https://github.com/llvllch/stonks

## Add Autostart

```
cat <<EOF | sudo tee /etc/systemd/system/btcticker.service
[Unit]
Description=btcticker
After=network.target

[Service]
ExecStart=/usr/bin/python3 -u /home/pi/stonks/cryptotick.py
WorkingDirectory=/home/pi/btcticker/
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
EOF
```

## Done

Reboot, and the display will update. The ticker is now a node.
