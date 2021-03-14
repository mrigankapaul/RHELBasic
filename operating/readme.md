```
shutdown --help
shutdown -r // reboot
shutdown -c // cancel
shutdown -h // or -P powering off the system.
shutdown +20 "bye bye folks" // in 20 min
shutdown 17:00 // shutdown at 5pm 
sutdown now // now
creating the file /etc/nologin standard users are restricted from logging into the system
using the shutdown command, standard users are restricted from login when less than 5 min remain before the event. /run/login
sudo !!
ls -l $(which reboot)
ls -l $(which poweroff)
sudo systemctl poweroff   // dont use this in enterprse system
sudo systemctl reboot or sudo reboot// dont use this enterprise system
which poweroff // path of the executable
sudo systenctl restart crond.service chronyd.service
sudo systenctl restart cron*
sudo yum install -y at
sudo systemctl enable atd && sudo systemctl start atd
sudo systemctl enable --now atd // start the service
systemctl status atd or ^enable^status // checking the status
ps -fp 1 // to verify the systemd is being used as a process manager
sudo systemctl stop chronyd.service
sudo systemctl disable chronyd.service
sudo yum install cockpit
sudo systemctl enable --now cockpit.socket
ss --ntl
sudo ss -ntlp
curl http://localhost:9090 // cockpit-tls will be enables
systemctl list-units [--type socket]// loaded systemd units
systemctl list-unit-files [--type socket] // all unit files no matter if they have been loaded
systemctl cat add
sudo systemctl edit --full atd
systemctl cat atd | head -n 3
```

### Boot Process 
- BIOS - The sytem bios is used to start the system and locate the boot partition.
- GRUB - The boot loader in RHEL is GRUB and allows you to select the linux kernel to load.
- Kernel - The linux kernel is loaded but prior to this the initialization ram disk is loaded to customize the boot process to your hardware.

### Systemd and Systemctl
- systemd as the system and service manager

### Unit file location
- default location /usr/lib/systemd/system
- /etc/systemd/system - administrator may do customization

### TODO
- Managing System Services
- Managing System Performance
- Managing Logging and log files

