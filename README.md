# Build a Minecraft Server

My son has asked me to help him build a Minecraft Server! I am totally making 
this a teaching project for him. I will use this README.md to walk him through
the process of building and later maintaining the server.

I will be giving him my old Dell XPS Mini Tower with the following specs:
- Intel Core i7 Processor
- 32GB of RAM
- 250GB SSD Drive

Regarding Operating Systems and Software, he will install the following:
- Ubuntu 18.04 Server (He already has experience building a Ubuntu Laptop)
- Paper Minecraft (Recommended for better performance)

## Install Ubuntu 18.04
### Create Boot USB
On a Linux system, insert a 4GB+ USB Drive in the systems USB slot, and then 
execute the following commands as root:
```bash
# Download latest Ubuntu 18.04 ISO image
curl -O https://releases.ubuntu.com/18.04/ubuntu-18.04.5-live-server-amd64.iso
# Find the device name of the USB Drive
sudo fdisk -l
# Once your find the device name (likely /dev/sdb), start the imaging of the USB drive with the ISO image
dd if=ubuntu-18.04.5-live-server-amd64.iso of=/dev/sdb bs=4M
```
Wait for the command to finish, and remove the USB Drive.

## Install Java

```bash
sudo apt update  # Update Apt Package Repository Metadata
sudo apt install openjdk-11-jre-headless  # Install JVM for Minecraft Server
# Function check for JRE/JVM
java -version
```

## Create Firewall Rule
```bash
sudo ufw status verbose  # check current status of OS firewall
sudo ufw allow ssh  # Add rule for SSH access
sudo ufw allow 25565  # Add rule for Minecraft Service
sudo ufw status verbose  # check post configuration status of OS firewall
```

## Install Minecraft Server
```bash
# Add service account for Minecraft service
useradd --system --shell /bin/bash --home /opt/minecraft -g minecraft minecraft
# Create and set ownership and permissions for working directory
sudo mkdir -p /opt/minecraft
sudo chown -R minecraft: /opt/minecraft
# Change to minecraft service account
sudo su - minecraft 
# Download latest Paper Minecraft Server JAR file
curl -O /opt/minecraft/ https://papermc.io/api/v2/projects/paper/versions/1.16.5/builds/468/downloads/paper-1.16.5-468.jar
# Accept the EULA
echo "eula=true" > /opt/minecraft/eula.txt
screen
java -Xms1024M -Xmx1024M -jar paper-1.16.5-468.jar nogui
```
Once you are finished, press `Ctrl + A + D` to detach from the screen session. You can log out of your terminal session now.
