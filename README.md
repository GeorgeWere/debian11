# Stuff about Debian 11

- Debian 11 got released and if you are debian fan like me am sure you cant wait to test it out. I did :)
- One thing you will notice is your user cannot use **sudo** and if you are from ubuntu or linux mint, am sure you will miss it, so here is a small guide to enable it.

# Why is it disabled by default

- sudo is a light program that allows users to run execute commands with another users security previlleges. This "user" is usually root.
- **sudo** can pose great security challenges, in fact if you are running a server environment you are safe to not enable it but if you are running desktop env then you can enable it. Reason being sudo allows you to execute commands that your user can not. In addition, the commands that are applied with sudo are not registered in the system log.

# Enable sudo on Debian 11

- first you have to install it and for that, you need to have access to the root user of the system. This is vital.

- open a terminal or connect to your server using SSH.

```lua
:~$ su
```

- Then, you will have to enter the root user key. If you did the installation, there should be no problem.
- After that, you can install sudo from the Debian repositories.

```lua
:~# apt install sudo
```

- Sudo is quite light so the installation is quite fast.

- Now you have to modify the file */etc/sudoers* which is where all the sudo configuration is located. Use below command for this:

```lua
visudo
```

- The file does not have too many lines. In the user privilege specification section, you will find a line like this.

```lua
root ALL=(ALL:ALL) ALL
```

- Under it, add your user and leave the rest the same. Something like that.

```lua
your-user ALL=(ALL:ALL) ALL
```

- Next, press **CTRL + O** to save the changes and **CTRL + X** to close it.

- After that, you can use sudo. 

# Now How about we Change our DHCP to Static

# Set a Static IP Address on Debian Server

- With Debian, setting a static IP address is a bit more old-school, so I’m going to show you how it’s done.

# How to give a standard user sudo privileges in a Debian server

- You can follow above tutorial or simply see below :)

- Let’s create a new user first. I’ll demonstrate by creating the user olivia (you can name the user whatever you like). To do that, log into Debian server as the root user and issue the command:

```bash
adduser george
```

- Once you’ve added the new user, add that user to the sudo group with:

```bash
sudo usermod -aG sudo george
```

- Now you can Exit from the root user and log in with the new user account.

# Now the Fun Part
## How to set a static IP address in a Debian server

- The first thing you must do is locate the name of your network device. For that, issue the command:

```lua
ip -c link show
```
- You should at least see two devices, lo (for loopback) and another named device **(such as enp0s3)**.

- Next, let’s back up the current network configuration file with the command:

```lua
sudo cp /etc/network/interfaces ~/
```

- Open the configuration file for editing with the command:

```lua
sudo nano /etc/network/interfaces
```
- If you find nano isn’t installed, add it with the command:

```lua
sudo apt-get install nano -y
```
- With the interfaces file open for editing, you should see a DHCP configuration that looks like this:

```lua
# The primary network interface

allow-hotplug enp0s3

iface enp0s3 inet dhcp
```
- Comment that block out so it looks like this:

```lua
# The primary network interface

# allow-hotplug enp0s3

# iface enp0s3 inet dhcp
```
- Now, we can add the necessary configuration for a static IP address. Let’s configure **enp0s3** to use the address **192.168.50.50**, with a *gateway* of **192.168.50.1**, and a *DNS nameserver* of **8.8.8.8** That configuration will look like this:

```lua
# The primary network interface

auto enp0s3

iface enp0s3  inet static

address 192.168.50.50

netmask 255.255.255.0

gateway 192.168.50.1

dns-domain example.com

dns-nameservers 8.8.8.8
```

- Make sure to edit the above configuration to match your network scheme. Save and close the file.

- Finally, restart the networking service with the command:

```lua
sudo systemctl restart networking
```

- Make sure the networking configuration is correct, by issuing the command:

```lua
ip a
```
- You should see the static IP address you configured. You’re good to go.

# Side Note

- This is for those who prefer to use the terminal. Of cos if you installed your environment to use GUI then you can use the gui tools to do so.


