autossh
=======

Due to network restrictions, you may be unable to connect via SSH to your computer.  However, you can create a permanent, auto-reconnecting tunnel.

Autossh is available on Ubuntu with the Aptitude tool:

```sh
apt-get install autossh
```

Once this is installed, you can use the autossh.conf file from this repo in combination with an always-on Linux box you have access to (your home machine, or a VPS you own), to create a permanently available tunnel.


Create or edit the file: `/etc/init/autossh.conf`


- 9999 Is the EXTERNAL port on your remote host (the always-on machine, e.g. VPS), so make sure there is no other service listening on that port and it's not blocked by a firewall. You may need to edit your Security Group if you're doing this on an Amazon EC2 instance, for example.

- 22 is your SSH port on your computer

- The last section of the autossh.conf file (just before -p) defines your SSH connection from your computer to the remote host. You should be able to run this independently, as a test, e.g. `sudo ssh user@remote.com -p22`.  You can, if you need to, append `-i /home/user/.ssh/id_rsa` or equivilant to this line to use key-based authentication with a different key. N.B. You should **run the ssh connection test as sudo**, as this is how it is run by the `sudo start autossh` command.  It's also important that you **do** have key-based authentication setup, as you autossh is unable to prompt you for a password.

- `eth0` and `eth1` are your possible ethernet network interfaces, confirm these by reviewing the output of `ifconfig` (equally `wlan0` and `iwconfig` for WiFi connections - for these you may wish to add  `and net-device-up IFACE=wlan0` or `and net-device-up IFACE=wlan1`).


Now you've got the configuration setup, you can test it with:

```sh
sudo start autossh
```

and terminate it with:


```sh
sudo stop autossh
```

Run This Service Automatically
------------------------------

Add the following line to your `/etc/rc.local`:

```sh
sudo start autossh
```

To start tunel, go to root (or use sudo) and: `start autossh`.

From now on (if everything went ok), you can log-in to your external host and ssh to your machine behind the NAT using: ssh localhost -p 9999 -l username_on_your_computer


Troubleshooting
---------------

If you have problems, check out the logfiles under `/var/log/upstart`. There should be one logfile for each script/instance, so expect to find `autossh.log` as well as many `autossh_host-N.log` files.