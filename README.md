# nagios

On server like mysql or client server:--

yum install epel-release

yum install nrpe nagios-plugins-all openssl

 vi /etc/nagios/nrpe.cfg
```
allowed_hosts=127.0.0.1,192.168.72.28 #nagios server IP

dont_blame_nrpe=1

command[check_users]=/usr/local/nagios/libexec/check_users $ARG1$
command[check_load]=/usr/local/nagios/libexec/check_load $ARG1$
command[check_disk]=/usr/local/nagios/libexec/check_disk $ARG1$
command[check_swap]=/usr/local/nagios/libexec/check_swap $ARG1$
command[check_cpu_stats]=/usr/lib64/nagios/plugins/check_cpu_stats.sh $ARG1$
command[check_memory]=/usr/local/nagios/libexec/check_memory  $ARG1$
command[check_service]=/usr/local/nagios/libexec/check_service  $ARG1$
```

ls /usr/lib64/nagios/plugins/  ==> copy your script or plugin 

vim /etc/nagios/nrpe.cfg  
```
allowed_hosts=127.0.0.1,IP of nagios

dont_blame_nrpe=1

command[script_name]=/usr/lib64/nagios/plugins/script_name  $ARG1$  $ARG2$  $ARG3$


```
service nrpe restart

On Nagios Sever:-

yum install nagios-plugins-all nagios-plugins-nrpe nrpe

/usr/local/nagios/etc/objects

vim command.cfg
```
define command{
        command_name    check_disk_inode
        command_line    /usr/local/nagios/libexec/check_nrpe -H $HOSTADDRESS$ -c script_name -a '-w $ARG1$ -c $ARG2$ -p $ARG3$'
        }

```

/usr/local/nagios/etc/servers

vim sql.cfg ## if you want to set script for SQL server
```
define host{
use                             linux-server
host_name                       sql
alias                           sql
address                         172.31.36.19 ##IP 
max_check_attempts              5
check_period                    24x7
notification_interval           30
notification_period             24x7
}


define service {
        use                             generic-service
        host_name                       sql
        service_description             check disk inode of root
        check_command                   check_disk_inode!2!4!/
        notifications_enabled           0
        }

```

there should be a file with name check_nrpe

cat /usr/local/nagios/libexec/check_nrpe  ## this is default script when you install nrpe.

/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg   #check the nagios conf gile

service nagios restart
==============================================================================================
for windows server

vim /usr/local/nagios/etc/objects/commands.cfg
```
# 'check_nt' command definition
define command{
        command_name    check_nt
        command_line    $USER1$/check_nt -H $HOSTADDRESS$ -p 12489 -s password -v $ARG1$ $ARG2$
        }

# -s password   ##password is of windows server , when you install NSClient++ it will ask for password or rendem generate password

```

vim /usr/local/nagios/etc/nagios.cfg  ==> edit uncomment line 38 and copy to server path and again comment it or you can use same windows file also

/usr/local/nagios/etc/objects/windows.cfg   ==> copy this windows file /usr/local/nagios/etc/servers/windows.cfg 

```
define host{
        use             windows-server  ; Inherit default values from a template
        host_name       nagios-windowserver     ; The name we're giving to this host
        alias           nagios-windowserver     ; A longer name associated with the host
        address         172.31.43.63    ; IP address of the host
        }

----

define service{
        use                     generic-service
        host_name               nagios-windowserver
        service_description     C:\ Drive Space
        check_command           check_nt!USEDDISKSPACE!-l c -w 80 -c 90
        }

define service{
        use                     generic-service
        host_name               nagios-windowserver
        service_description     D:\ Drive Space
        check_command           check_nt!USEDDISKSPACE!-l d -w 80 -c 90
        }

```


