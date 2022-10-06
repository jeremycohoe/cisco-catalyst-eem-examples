# cisco-catalyst-eem-examples

# Set POE power off daily

```
!POE example power OFF
no event manager applet SelectivePowerOff
event manager applet SelectivePowerOff
! Turn *OFF* POE power to the ports daily at 5PM: 0 17 * * *
event timer cron name DST_Start cron-entry "0 17 * * *"
! or
! event none
!
 action 0.0 cli command "enable"
 action 0.1 cli command "show power inline"
 action 0.2 foreach line "$_cli_result" "\n"
 action 1.1  regexp "^([^[:space:]]*)[[:space:]]*[^[:space:]]*[[:space:]]*on.*$" "$line" temp interface
 action 1.2  if $_regexp_result eq 1
 action 1.3   cli command "conf t"
 action 1.4   cli command "interface $interface"
 action 1.5   cli command "power inline never"
 action 1.6   syslog msg "Turned off PoE on $interface"
 action 1.7  end
 action 2.1 end
```

# Set power on daily

```
!POE example power on
event manager applet SelectivePowerOn
! Turn **ON** POE power to the ports daily at 8AM: 0 8 * * *
event timer cron name DST_Start cron-entry "0 8 * * *"
!
! or
!event none
 action 0.0 cli command "enable"
 action 0.1 cli command "show power inline"
 action 0.2 foreach line "$_cli_result" "\n"
 action 1.1  regexp "^([^[:space:]]*)[[:space:]]*off.*$" "$line" temp interface
 action 1.2  if $_regexp_result eq 1
 action 1.3   cli command "conf t"
 action 1.4   cli command "interface $interface"
 action 1.5   cli command "power inline auto"
 action 1.6   syslog msg "Turned on PoE on $interface"
 action 1.7  end
 action 2.1 end
 exit
```
