# icinga
Icinga checks for FreeBSD

$USER5$ is the location of the script at target system, /home/icinga/ or /usr/local/scripts/ ...

```
define command{
        command_name    ssh-check-smart
        command_line    $USER1$/check_by_ssh -p$_HOST_SSH_PORT$ -t 20 -H $HOSTADDRESS$ -C "$USER5$/check_smart -d $ARG1$ -i $ARG2$ -m $ARG3$"
}

define command{
        command_name    ssh-check-raid-camcontrol
        command_line    $USER1$/check_by_ssh -p$_HOST_SSH_PORT$ -t 20 -H $HOSTADDRESS$ -C "$USER5$/check_camcontrol"
}

define command{
        command_name    ssh-check-raid-mfi
        command_line    $USER1$/check_by_ssh -p$_HOST_SSH_PORT$ -t 20 -H $HOSTADDRESS$ -C "$USER5$/check_mfi"
}

define command{
        command_name    ssh-check-raid-mpt
        command_line    $USER1$/check_by_ssh -p$_HOST_SSH_PORT$ -t 20 -H $HOSTADDRESS$ -C "$USER5$/check_mpt -u $ARG1$"
}

define command{
        command_name    ssh-check-raid-geom
        command_line    $USER1$/check_by_ssh -p$_HOST_SSH_PORT$ -t 20 -H $HOSTADDRESS$ -C "$USER5$/check_geom -t $ARG1$ -n $ARG2"
}
```

```

define service {
        use                     generic-12hours
        host_name               host.domain
        service_description     Host HDD0 S.M.A.R.T
        check_command           ssh-check-smart!/dev/ciss0!cciss,0!scsi
        notification_interval   720
}

define service {
        use                     generic-1hour
        host_name               host.domain
        service_description     Host RAID
        check_command           ssh-check-raid-camcontrol
        notification_interval   720
}

define service {
        use                     generic-1hour
        host_name               host.domain
        service_description     Host RAID
        check_command           ssh-check-raid-mpt!0
        notification_interval   720
}

define service {
        use                     generic-1hour
        host_name               host.domain
        service_description     Host RAID
        check_command           ssh-check-raid-mfi
        notification_interval   720
}

define service {
        use                     generic-12hours
        host_name               host.domain
        service_description     host RAID
        check_command           ssh-check-raid-geom!graid!Intel-123456789
        notification_interval   720
}


```
