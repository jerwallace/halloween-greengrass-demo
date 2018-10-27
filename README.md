# halloween-greengrass-demo

GreenGrass on RaspberryPi QuickStart

First, SSH into your raspberry pi.

```
sudo adduser --system ggc_user
sudo addgroup --system ggc_group
sudo apt-get install rpi-update
sudo rpi-update b81a11258fc911170b40a0b09bbd63c84bc5ad59
sudo reboot
uname -a
```

**Ensure that Raspberry Pi version is 4.9.30**
Edit 98-rpi.conf
```
cd /etc/sysctl.d
sudo nano 98-rpi.conf
```

Copy/paste this:
```
fs.protected_hardlinks = 1
fs.protected_symlinks = 1
```

```
sudo reboot
sudo sysctl -a 2> /dev/null | grep fs.protected
```

You should see **fs.protected_hardlinks = 1** and **fs.protected_symlinks = 1**.
Download and run greengrass

```
cd /home/pi/Downloads
git clone https://github.com/aws-samples/aws-greengrass-samples.git
cd aws-greengrass-samples
cd greengrass-dependency-checker-GGCv1.6.0
sudo modprobe configs
sudo ./check_ggc_dependencies | more
```

Look it over for problems. Install Node.JS
```
cd ~
wget https://nodejs.org/dist/v8.9.0/node-v8.9.0-linux-armv6l.tar.gz
tar -xzf node-v8.9.0-linux-armv6l.tar.gz
cd node-v8.9.0-linux-armv6l/
sudo cp -R * /usr/local/
node -v
```

It should say **v8.9.0**
```
sudo npm install -g n
```

Go into the IoT Core page and follow the wizard, download arm (raspbian Jesse) package and the certs. Copy them to your raspberry pi

```
scp <file> pi@<IP>:/home/pi/
sudo tar -xzvf greengrass-OS-architecture-1.6.0.tar.gz -C /
sudo tar -xzvf GUID-setup.tar.gz -C /greengrass
cd /greengrass/certs/
sudo wget -O root.ca.pem http://www.symantec.com/content/en/us/enterprise/verisign/roots/VeriSign-Class%203-Public-Primary-Certification-Authority-G5.pem
cat root.ca.pem
```

Cert should be there...
```
cd /greengrass/ggc/core/
```
