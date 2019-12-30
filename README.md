# nagios

On server like mysql:--

ls /usr/lib64/nagios/plugins/  ==> plugin or script
# copy your script or plugin 

vim /etc/nagios/nrpe.cfg  
```
allowed_hosts=127.0.0.1,IP of nagios

dont_blame_nrpe=1

command[script_name]=/usr/lib64/nagios/plugins/script_name  $ARG1$  $ARG2$  $ARG3$


```
service nrpe restart

On Nagios Sever:-
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

service nagios restart


