autossh
=======

Some people are behind NAT, no port redirections are in place, so if you have access to shell account on some live host, you can get yourself a ssh tunel



Locate file in: /etc/init/autossh.conf
#####################################################################

- 9999 is your port on EXTERNAL host, so make sure there is no other service listening on that port and it's not blocked on firewall.

- 22 is your ssh port on your computer

- in case you have crazy admin up there running ssh on different port than 22, just change "-p 22" on to "-p <ssh_port>"


Add to your /etc/rc.local:
#################################
service autossh start
#################################


To start tunel, go to root (or use sudo) and: service autossh start
From now on (if everything went OK), you can log in to your external host and ssh to your machine behind the NAT using: ssh localhost -p 9999 -l <username_on_machine_behind_nat>
