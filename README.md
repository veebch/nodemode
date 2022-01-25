![Alt text](images/node.png?raw=true "Title")

[![YouTube Channel Views](https://img.shields.io/youtube/channel/views/UCz5BOU9J9pB_O0B8-rDjCWQ?label=YouTube&style=social)](https://www.youtube.com/channel/UCz5BOU9J9pB_O0B8-rDjCWQ)

# nodemode
Instructions to connect the [Veeb tickerXL](https://www.veeb.ch/store/p/tickerxl), running the crypto price ticker code ([here](https://github.com/llvllch/stonks)) to a [Raspiblitz bitcoin node](https://github.com/rootzoll/raspiblitz).

# Prerequisites
A [Raspiblitz](https://github.com/rootzoll/raspiblitz) node, up and running. If you're building from scratch, here's the shopping list for the components we used: 

- Raspberry Pi 4 (4 or 8 Gb)
- [Heatsink](https://www.amazon.de/gp/product/B082Y21GX5/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1) 
- [1 TB SSD](https://www.amazon.de/gp/product/B087S2J4VB/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1)
- [TickerXL](https://www.veeb.ch/store/p/tickerxl)
 
# Instructions

## Login to your node 

Login over ssh.
`ssh admin@<YOURNODEIP>`
Using your node login password

Edit `/home/pi/.bashrc` using the command `nano /home/pi/.bashrc` and comment out (add a # to the beginning of the line) the line that reads `SCRIPT=/home/admin/00infoLCD.sh`(the last line in mine), changing it to
`#SCRIPT=/home/admin/00infoLCD.sh` 

## Fix the spi setup 

Raspiblitz needs a couple of extra tools to run the epd display over spi
```
 sudo apt install python-spidev python3-spidev
```
followed by adding admin and pi to the spi group.
```
 sudo usermod -a -G spi admin
 sudo usermod -a -G spi pi
```
There is also a line in `/boot/config.txt` that assigns the display to a waveshare device. Opend this file for editing with 

    sudo nano /boot/config.txt
   
and delete or comment out the line.

Reboot, and ssh back in as the user pi. `ssh pi@<YOURNODEIP>`

## Install the ticker code 

Once you're logged into the node as user `pi`, following the installation instructions at https://github.com/llvllch/stonks

## Add Autostart

```
cat <<EOF | sudo tee /etc/systemd/system/btcticker.service
[Unit]
Description=btcticker
After=network.target

[Service]
ExecStart=/usr/bin/python3 -u /home/pi/stonks/cryptotick.py
WorkingDirectory=/home/pi/stonks/
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
EOF
```

## Done

Reboot, and the display will update. The ticker is now a full bitcoin (lightning enabled) node.
