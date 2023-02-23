# cisco-catalyst-eem-examples

# Set POE power off daily

```
! EEM POE example SelectivePowerOff
no event manager applet SelectivePowerOff
event manager applet SelectivePowerOff
! Turn *OFF* POE power to the ports daily at 9PM: 0 21 * * *
event timer cron name SelectivePowerOff cron-entry "0 21 * * *"
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
![image](https://user-images.githubusercontent.com/29899961/221041632-5d75e387-0e50-4f61-9e64-1473e516b152.png)

```

# Set power on daily

```
! EEM POE example SelectivePowerOn
no event manager applet SelectivePowerOn
event manager applet SelectivePowerOn
! Turn **ON** POE power to the ports daily at 6AM: 0 6 * * *
event timer cron name SelectivePowerOn cron-entry "0 6 * * *"
!
! or
! event none
!
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

```
