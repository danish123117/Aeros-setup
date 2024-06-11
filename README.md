# Rasp berry pi configuration

This guide explains how to change the SSH port from the default port 22 to port 8080. This can enhance security by reducing the risk of automated attacks on the default SSH port.
## 1. Setup 
### i) Download and install raspberry pi imager 
You can use following link 
```
https://www.raspberrypi.com/software/
```
### ii) Open it and flash the recomended raspberrypi OS desktop distro to a micro sd card 

### iii) Insert the sd card into the raspberrypi and boot it 


### iv) enable PCIe 
```
sudo nano /boot/firmware/config.txt
```

to enable PCIe add the following line to this file and save 

```
dtparam=pciex1
```

optionally we can also change the ssd speed from default Gen 2 speed to Gen 3
for this we add the following line 

```
dtparam=pciex1_gen=3
```

### v) Change boot order 

run following command 

```
sudo rpi-eeprom-config --edit
```
update the following line and save
```
BOOT_ORDER=0xf416
```
the last digit 6 stands for SSD it should be at the end for booting from ssd 
more details about the meaning can be found here 

```
https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#BOOT_ORDER
```
### vi) Install ssd and reboot
Flash the ssd with os of choice and then install it on your Rasp berry 
On the next reboot everything should work fine

## 2. Steps to Change SSH Port

### A Backup SSH Configuration
Before making any changes, back up the current SSH configuration file:

```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```
### B Change the port number:
Look for a line that says #Port 22. This line may be commented out with a # at the beginning
Uncomment the line by removing the # and change the port number from 22 to 8080. The line should now read:

```
Port 8080

```

### C Change port number in ssh.socket file

Open the ssh.socket file 

```
sudo nano /lib/systemd/system/ssh.socket
```
Change the port number from 22 to 8080 flowing line

```
ListenStream=8080
```

### D Reload daemon and restart ssh 

Run the following commands 

```
sudo systemctl daemon-reload 
sudo systemctl restart ssh  
```

### E Test if it works 

Run the following 

```
ssh -p 8080 user@ipaddress
```
## 3. Important links 
### i) How to sync date time
If datetime is out of sync, it is not posible to connect to Polimi networks
```
https://www.server-world.info/en/note?os=Ubuntu_24.04&p=ntp&f=3
```
Minor edit 

```
NTP=pool.ntp.org

p.s. the update does not work on Polimi networks you need to connect a mobile hotspot to run the update
````





