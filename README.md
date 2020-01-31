# Minecraft Server

This is a guide to install a Minecraft server that will start on boot.I have based this guide on Ubuntu 16.04 LTS

### Update & Upgrade

First we must update the system using these commands.
```
sudo apt-get update
sudo apt-get dist-upgrade
```
### Install Dependancies

Next we will install the dependencies required
```
sudo apt-get install openjdk-8-jre openjdk-8-jre-headless curl screen nano grep -y
```

### Setup User

Switch user to root:
```
su -
```
Add the user and group 'Minecraft':
```
useradd minecraft
groupadd minecraft
```

Add the Minecraft user to the related group:
```
usermod -a -G minecraft minecraft
```

Make home directory for Minecraft user
```
mkdir /home/minecraft
```

### Setup Mincraft Server

Open port on the firewall to allow the Minecraft server traffic:
```
sudo ufw allow 25565/tcp
```

Switch to the Minecraft user:
```
su minecraft
```

Change to the Minecraft home directory:
```
cd
```

Download the Minecraft server jar file:
Either download from or use the command: https://minecraft.net/en-us/download/server
```
wget https://launcher.mojang.com/mc/game/1.12.2server/886945bfb2b978778c3a0288fd7fab09d315b25f/server.jar 
```

Test that the server works with:
```
java -Xmx1024M -Xms1024M -jar minecraft_server.jar nogui
```
I'm using 1GB, you can use more if you want,

Accept the EULA in order to use the server software:
```
sed -i.orig 's/eula=false/eula=true/g' eula.txt
```
or manually
```
sudo nano eula.txt
```

Test the server again. It should run this time as we have accepted the EULA.
```
java -Xmx2048M -Xms2048M -jar minecraft_server.jar nogui
```

After this you can configure the settings in the relevant files stored within the server.

## Making the Systemmd Unit File

Switch to root user:
```
su
```

Create the systemd file to boot the server on launch:
```
nano /etc/systemd/system/minecraft-server.service
```

Copy and paste this text into the file, change your settings accordingly (if you have followed thisguide, leave as default.)

```
[Unit] Description=start and stop the minecraft-server
[Service] WorkingDirectory=/home/minecraft

User=minecraft
Group=minecraft
Restart=on-failure
RestartSec=20 5
ExecStart=/usr/bin/java -Xms2048M -Xmx2048M -jar server.jar nogui

[Install] WantedBy=multi-user.target
Alias=minecraft.service
```
Start the server with:
```
systemctl start minecraft-server.service
```
Stop the server with:
```
systemctl stop minecraft-server.service
```
To start the server on boot, run:
```
systemctl enable minecraft-server.service
```


