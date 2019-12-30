# nagios

On server like mysql:--

ls /usr/lib64/nagios/plugins/  ==> plugin


vim /etc/nagios/nrpe.cfg  

---
allowed_hosts=127.0.0.1,IP of nagios

dont_blame_nrpe=1

command[check_disk_inode]=/usr/lib64/nagios/plugins/check_disk_inode  $ARG1$  $ARG2$  $ARG3$

----

On Nagios Sever:-


