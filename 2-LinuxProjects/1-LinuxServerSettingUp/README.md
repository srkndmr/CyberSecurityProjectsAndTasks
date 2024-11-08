
# Linux Server

## Table of Contents
- [Creates VM](#creates-vm)
  - [1) One VM for the client](#1-one-vm-for-the-client)
  - [2) One VM for the server](#2-one-vm-for-the-server)
  - [3) Virtualbox](#3-virtualbox)
- [Setup CLIENT device](#setup-client-device)
  - [1) Basic software installation](#1-basic-software-installation)
  - [2) Remote access](#2-remote-access)
- [Setup SERVER device](#setup-server-device)
  - [1) Create a new user for administration](#1-create-a-new-user-for-administration)
  - [2) Setup a static IP](#2-setup-a-static-ip)
  - [3) Firewall](#3-install-a-firewall)
  - [4) Check SSH](#4-check-ssh)
  - [5) Server Web Nginx](#5-server-web-nginx)
  - [6) DNS](#6-dns)
  - [7) DHCP](#7-dhcp)
  - [8) SAMBA](#8-samba)
  - [9) Security ](#9-security)



## Creates VM

### 1) One VM for the client

- Classic Ubuntu (dont forget on the first page of VirtualBox to mark the case "skip unattended installation" )
- Don't forget to select **separate the /home partition** at the disk selection step.
- I set the following credentials
	- Username : server
	- Password : root

> Hard password to find are the best

### 2) One VM for the server

- I set the following credentials
	- Username : server
	- Password : root

> Hard password to find are the best as I said

### 3) Virtualbox

- Create a NAT network
	- go to `Tools` -> `Network` -> `Nat networks` 
	- set a name for the network.
	- I did let oracle chose my network , here it was  **10.0.2.0/24**
	- Don't check the `Enable DHCP`, we'll do it by ourself

![NAT Network Configuration](./images/Networkprivate.PNG)

- **NAT Network**: Named "Project".
- **IPv4 Prefix**: `10.0.2.0/24`
- **IPv6 Prefix**: `fd17:625c:f037:2::/64`
- **DHCP Server**: Disabled.
- **Description**: This configuration shows a private NAT network setup for the project, with static addressing indicated by the disabled DHCP server.

- On both VM put 2 adaptater to begin with : NAT and the private NAT we created, once we finished every config, we'll remove the NAT and use only the private NAT


-Can also work with "private host network" like we did on another machine : 

![Debian Server Host-Only Network](./images/Private%20host.png)

## Setup CLIENT device

First of all update the device
- `sudo apt update`
- `sudo apt upgrade`
- `sudo reboot`

If you encounter any problem because sudo is not installed on your machine.
- ` sudo -i`
- ` apt install sudo `
- exit

Then you do the three lines above.

### 1) Basic software installation 

- **LibreOffice:**  `sudo apt install libreoffice`
- **Gimp:** `sudo apt install gimp`
- **Web-Browser (Mullvad) :** Go on their official website : 
https://mullvad.net/en/download/browser/linux  
`cd Downloads`  
`tar xf mullvad-browser-linux64-[YOUR_VERSION].tar.xf`  
`rm mullvad-browser-linux64-[YOUR_VERSION].tar.xf`  
`mv mullvad-browser ~/`  
`cd mullvad-browser`  
`./start-mullvad-browser-desktop --register-app`  

### 2) Remote access

2 options SSH and a remote desktop solution:

**2.1 SSH**
- install openssh `sudo apt install openssh-server openssh-client`
- enable ssh `sudo systemctl enable ssh`

**2.2 Remote desktop**
- Select and install a software here i use _remmina_ `sudo apt install remmina`
- open the software and create a new profile
- Install the software to an other machine to connect to the client.

To connect from windows :

### Step 1 :

`sudo apt install xrdp -y`  - Install the package  
`sudo systemctl start xrdp`  - Start the service  
`sudo systemctl enable xrdp`  - Make it boot when the server boot  
`ip addr show`  - To see our ip  
`sudo ufw allow 3389/tcp` - Allowing this service in our UFW  

### Step 2: Connect from Windows

Now that the Linux machine is set up, connect from Windows.

Open Remote Desktop Connection for RDP:

Press `Win + R`, type `mstsc`, and press Enter.
Enter the IP address of the Linux machine `10.0.2.10` in the Remote Desktop Connection dialog.

Connect and enter your Linux credentials.

## Setup SERVER device

First of all as root update the device and install sudo package
- `apt install sudo`
- `apt update`
- `apt upgrade`
- `sudo reboot`

Same as for the client, if you don't have sudo on it, follow the step that we did on the client.

### 1) Create a new user for administration 

-  `adduser userName` 

- `sudo usermod -aG sudo`

- An alternative is to type `visudo`

![Visudo Configuration](./images/visudo.PNG)


### 2) Setup a static IP

- First save the configuation to a file named incaseofemergencyconfig `sudo cp /etc/network/interfaces /etc/network/incaseofemergencyconfig`
- then modify the interfaces `sudo nano /etc/network/interfaces` as seen on this screenshot : 

![Static IP Configuration](./images/staticip.PNG)


- restart the network service `sudo systemctl restart networking.service`
- check if you have the good ip with a `ip a`
- Also, make sure that you have internet: `wget -q --spider https://facebook.com/ && echo 'Internet OK' || echo 'Internet KO'` ( you can replace facebook by any website you want)


### 3) Install a firewall

Using ufw

Using the `ufw` command:
- `sudo apt install ufw`

Make sure that those are the default parameter settings:
- `sudo ufw default deny incoming`
- `sudo ufw default allow outgoing`

Now, allow the SSH connection:
`sudo ufw allow 22` - for SSH
You will need to do the same with other ports:
- `sudo ufw allow 80` - for HTTP
- `sudo ufw allow 443` - for HTTPS
- `sudo ufw allow 54` - for DNS
(we'll add some other later in the project)

You can activate the firewall with `sudo ufw enable` and check its status with `sudo ufw status`

#SCREEN SHOT HERE

### 4) Check SSH 

- `sudo apt install openssh-server`
  `sudo systemctl start ssh`
  `sudo systemctl enable ssh` ( to make it starting when the server is booting)
- verify your server ip with   `ip a ` here :  `10.0.2.10 `
 
- Go on client device and try ssh connection with the user you have  `ssh server@10.0.2.10` should work.

- MAKE SURE THAT YOU DON'T HAVE ACCESS TO IT WHILE TRYING TO CONNECT AS ROOT.



### 5) SERVER WEB NGINX

`sudo apt install nginx -y` - Installation  
`sudo systemctl start nginx` - Starting the service   
`sudo systemctl enable nginx` - Making the service boot when the server boot  
`sudo systemctl status nginx` - Check the status of the service  
`sudo ufw allow 'Nginx Full'` - Enabling it on our firewall  

Then test it : 

Open your browser and go to your server’s IP address (here http://10.0.2.10).
You should see the NGINX default welcome page, confirming it's working.

To configure it :

Configuration files for NGINX are stored in `/etc/nginx/.`  
The main configuration file is `/etc/nginx/nginx.conf.`  
You can create custom configuration files in `/etc/nginx/sites-available/` and enable them by linking them to `/etc/nginx/sites-enabled/.`  

Whenever you do a configuration change restart the service :

`sudo systemctl restart nginx`  

### 6) DNS

The Domain Name System (DNS) acts like the internet’s address book. When you enter a website's name (such as www.example.com) in your browser, DNS steps in to convert that name into an IP address—a unique sequence of numbers that computers use to recognize each other across the network. This process allows you to visit websites using easy-to-remember names, rather than having to remember complex numerical addresses.

Documentation used on [adamtheautomator](https://adamtheautomator.com/bind-dns-server/).

### Installing BIND Packages

`sudo apt install bind9 bind9-utils bind9-dnsutils -y` - Download the packagge
`sudo systemctl is-enabled named` - Check if named service enabled
`sudo systemctl status named` - Check named service status
`sudo systemctl enable  named` - Make the service boot when the server boot

### Configuring BIND DSN Server

Navigate to the */etc/default/* directory and edit the */etc/default/named* file:
- `OPTIONS="-4 -u bind` - This tells BIND to use IPv4 only, instead of both IPv4 and IPv6. It can be useful if you're facing issues with IPv6 connectivity or if you only want to allow IPv4 traffic. 

Now navigate to the */etc/bind/* directory and edit the */etc/bind/named.conf.options)* file:
```
// listen port and address 
listen-on port 53 { localhost; 10.0.2.10; }; 

// for public DNS server - allow from any 
allow-query { any; }; 

// define the forwarder for DNS queries 
forwarders { 9.9.9.9; }; 

// enable recursion that provides recursive query 
recursion yes;
```

Do not forget to comment-out the `listen-on-v6` line.

Check the BIND configuration: `sudo named-checkconf`

###  Setting up DNS Zones

Navigate to the */etc/bind/* directory and edit the */etc/bind/named.conf.local* file: 
```
zone "bookstore." {
    type master;
    file "/etc/bind/zones/forward.bookstore.io";
};

zone "2.0.10.in-addr.arpa" {
    type master;
    file "/etc/bind/zones/reverse.bookstore.io";
};
```

Create a new directory */etc/bind/zones*: `sudo mkdir -p /etc/bind/zones/`

Copy the default **forward** and **reverse** zones:
- `sudo cp /etc/bind/db.local /etc/bind/zones/forward.kamkar3lib.io`
- `sudo cp /etc/bind/db.127 /etc/bind/zones/reverse.kamkar3lib.io`

Edit the **forward** zone file */etc/bind/zones/forward.bookstore.io*:
```
;
; BIND data file for the local loopback interface
;
$TTL    604800
@       IN      SOA     bookstore.io. root.bookstore.io. (
	                        2     ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL

; Define the default name server to ns1.kamkar3lib.io
@       IN      NS      ns1.bookstore.io.
ns1     IN      A       10.0.2.10

kamkar3lib.io. IN   MX   10   mail.bookstore.io.

www     IN      A       10.0.2.10
mail    IN      A       10.0.2.20
vault   IN      A       10.0.2.30
```

Edit the **reverse** file */etc/bind/zones/reverse.bookstore.io*:
```
;
; BIND reverse data file for the local loopback interface
;
$TTL    604800
@       IN      SOA     bookstore.io. root.bookstore.io. (
                            1         ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL

@       IN      NS      ns1.bookstore.io.
ns1     IN      A       10.0.2.10
10      IN      PTR     ns1.bookstore.io.
20      IN      PTR     mail.bookstore.io.
```

Check the BIND configurations by running the following commands:
- `sudo named-checkconf`  
- `sudo named-checkzone bookstore.io /etc/bind/zones/forward.bookstore.io`  
- `sudo named-checkzone bookstore.io /etc/bind/zones/reverse.bookstore.io`  

Restart and verify the *named* service with the `systemctl` command:
- `sudo systemctl restart named`  
- `sudo systemctl status named`  

### Opening DNS port with UFW Firewall
- `sudo ufw allow Bind9`  
- `sudo ufw status`  

### Verify BIND DNS Server Installation
- `dig @10.0.2.10 www.bookstore.io`  
- `dig @10.0.2.10 mail.bookstore.io`  
- `dig @10.0.2.10 vault.bookstore.io`  

- `dig @10.0.2.10 bookstore.io MX`  

- `dig @10.0.2.10 -x 10.0.2.10`  
- `dig @10.0.2.10 -x 10.0.2.20`  


### 7) DHCP

- Install ISC DHCP SERVER `sudo apt install isc-dhcp-server`
- Say wich interface gonna be used.
 Modify `sudo nano /etc/default/isc-dhcp-server`
	>`INTERFACESv4="enp0s3"`
leave INTERFACEv6 empty

- configure the DHCP by modify the file `sudo nano /etc/dhcp/dhcpd.conf`
	
```
subnet 10.0.2.0 netmask 255.255.255.0 {
	range 10.0.2.100 10.0.2.200;
	option domain-name-servers 10.0.2.10;
	option routers 10.0.2.1;
}
```
- Restart the service: `sudo service isc-dhcp-server restart`

### 8) SAMBA

`sudo apt install samba -y`

Backup the default Samba configuration file:

`sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup`

Edit the Samba configuration file:

`sudo nano /etc/samba/smb.conf`

Add a new shared folder configuration at the end of the file. For example, to share the folder /srv/samba/share:

[share]  

`path = /srv/samba/share  
browseable = yes  
writable = yes  
guest ok = no  
valid users = @sambashare`  

Create the Shared Directory

`sudo mkdir -p /srv/samba/share`  
`sudo chown :sambashare /srv/samba/share`  
`sudo chmod 2770 /srv/samba/share`  

Set Up Samba Users

`sudo usermod -aG sambashare username`  

Set a Samba password for this user:

`sudo smbpasswd -a username`  

Restart Samba Services

`sudo systemctl restart smbd`  
`sudo systemctl restart nmbd`  

Add Samba to the firewall

`sudo ufw allow 'Samba'`  
`sudo ufw reload`  

Run the following command to verify that the configuration is correct:

`testparm`


### Access the Samba Share

- **From Windows**:
  1. Open the Run dialog (`Win + R`).
  2. Type `\\[server IP]\share` and press Enter.

- **From Linux**:
  - Using `smbclient`:  
    ```bash  
    smbclient //server_IP/share  
    ```  
  - Or, mount the share using the `mount` command:  
    ```bash  
    sudo mount -t cifs //server_IP/share /path/to/mountpoint -o username=[your_username]  
    ```  

- **From macOS**:
  1. Open Finder.
  2. Go to **Go -> Connect to Server**.
  3. Enter `smb://[server IP]/share` and click **Connect**.


### 9) Security


### BACKUP

Enable protection for SSH and adjust ban times as needed.

- first we need to add a back up drive on the vm :
	- In VirtualBox, select the device and go to Settings.
	- Click on storage.
	- Click on the hard drive on "Controller:SATA".
	- Click Create
	- Leave VDI selected 
	- Allocate space
	- Select your new volume and click on "Choose".
	- turn on the VM and make a `lsblk` to verify if the newfrive is here

-  Now format the new drive `sudo mkfs.ext4 /dev/sdb`
-  Create a backup folder `sudo mkdir /mnt/conf_backups`
- Make a script to save files on the backup drive:
	- Perform this as ROOT `su`
	- create the folder and file for the script : `mkdir script` then `nano /root/scripts/backup-conf.sh`
	- put this script in it :
> `sudo mount /dev/sdb /mnt/backup-conf/` -> Mounts a disk device (`/dev/sdb`) to the `/mnt/backup-conf/` directory
> 
  > `sudo mkdir /tmp/$(date +%d-%b-%Y)` -> Creates a temporary directory with a name based on the current date:
   >
   >`sudo cp -r /etc/bind /etc/dhcp /etc/resolv.conf /tmp/$(date +%d-%b-%Y)/` -> Copies the following directories and files to the temporary directory
   >
   >`sudo tar -zcvf /mnt/conf_backups/$(date +%d-%b-%Y).tar.gz /tmp/$(date +%d-%b-%Y)/` -> Creates a compressed tarball archive of the temporary directory:
   >
   >`sudo rm -rf /tmp/$(date +%d-%b-%Y)` -> Removes the temporary directory and its contents:
   >
   >`sudo umount /dev/sdb` -> Unmounts the disk device (`/dev/sdb`)
   
 - and finaly make a crontab to execute it at a specific time `sudo crontab -u root -e`
 - to make the save every sunday at 2 AM write this `0 2 * * 0  /root/scripts/backup-conf.sh`
>- `0`: Specifies the minute when the task should run (in this case, 0 minutes).
>- `02`: Specifies the hour when the task should run (in this case, 2 AM in 24-hour format).
>- `*`: Specifies any value for the day of the month field, meaning the task can run on any day of the month.
>- `*`: Specifies any value for the month field, meaning the task can run in any month.
>- `0`: Specifies the day of the week when the task should run (in this case, Sunday, where Saturday is represented by 0).
>- `/root/scripts/backup-conf`: Specifies the command or script that should be executed at the specified time.
   
 ### FAIL2BAN

Fail2Ban monitors failed login attempts and blocks IPs with multiple failed attempts.

`sudo apt install fail2ban`  

To configure it:

`sudo cp /etc/fail2ban/jail.conf  /etc/fail2ban/jail.local`   

`sudo nano /etc/fail2ban/jail.local`   

Enable protection for SSH and adjust ban times as needed.

 ### AIDE

AIDE (Advanced Intrusion Detection Environment) scans your file system for changes.
`sudo apt install aide`  

`sudo aideinit`  

`sudo cp /var/lib/aide/aide.db.new /var/lib/aide/aide.db`  

To check for changes:
`sudo aide --check`    

 ### Monitor Logs

 Regularly monitor logs to check for suspicious activity. Use tools like logwatch for easier log summaries.

`sudo apt install logwatch    
sudo logwatch --detail High --mailto your-email@example.com --range today`   

### TCP Wrappers

tcp_wrappers allows you to control network access to specific services.
`sudo apt install tcpd`  

Configure access by editing the /etc/hosts.allow and /etc/hosts.deny files.
`sudo nano /etc/hosts.allow`  
> sshd: 10.0.2.X  

In /etc/hosts.deny, block other connections:
`sudo nano /etc/hosts.deny`  
> sshd: ALL  

### Web Application Firewall

Install ModSecurity for Apache:
`sudo apt install libapache2-mod-security2`  
`sudo a2enmod security2`  

Configure ModSecurity rules:
`sudo nano /etc/modsecurity/modsecurity.conf`  

Enable it and restart Apache:
`sudo systemctl restart apache2`  

### Encrypt Sensitive Directories

Encrypting directories containing sensitive data adds another layer of protection in case of unauthorized access.

Use ecryptfs-utils for this purpose:
`sudo apt install ecryptfs-utils`  
`sudo mount -t ecryptfs /path/to/sensitive-directory /path/to/mount-point`  

### Harden DNS with DNSSec

If your server provides DNS services, enable DNSSEC (DNS Security Extensions) to authenticate DNS queries and responses.
`sudo nano /etc/bind/named.conf.options`  
Add the following:
`dnssec-enable yes;`  
`dnssec-validation yes;`  

### Enable Logging and Monitoring for Sudo Commands

Track all sudo actions to have an audit trail of privileged commands.

Edit `/etc/sudoers` to enable logging:

`Defaults logfile="/var/log/sudo.log"`  


![My GIF](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExdWwxMHd0ODhwdzFsMDd3NXlxM3BkamIwNWZ4NmljZXhkbHJ3OTIzayZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/8UF0EXzsc0Ckg/giphy.gif)

