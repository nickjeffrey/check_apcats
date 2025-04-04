# check_apcats
nagios check for SNMP-enabled APC Automatic Transfer Switches

# Requirements
perl, snmpget on nagios server

# Device Setup
On the Device web management interface, ensure SNMP is enabled.
<img src=images/snmp.png>

# Configuration
You will need a section in the services.cfg file on the nagios server that looks similar to the following.
```
      # Define a service to check the APC ATS
      # Parameters are SNMP community name
      define service {
              use                             generic-service
              hostgroup_name                  all_apcats
              service_description             APC ATS
              check_command                   check_apcats!public
              }
```

You will also need a command definition similar to the following in commands.cfg on the nagios server

```
      # 'check_apcats' command definition
      # parameters are -H hostname -C snmp_community
      define command{
              command_name    check_apcats
              command_line    $USER1$/check_apcats -H $HOSTADDRESS$ -C $ARG1$
              }
```

# Sample Output
```
APC ATS OK - Dual power sources are fully redundant. Active power source is 2.  model:AP7721  serial:5A1316T0XXXX 

APC ATS WARN - Redundant power lost, cannot failover to alternate source!. Active power source is 2.  model:AP7721  serial:5A1316T0XXXX 
```

When both of the power sources are up, the web management interface will look similar to:
<imgr src=images/redundant_power.png>


If one of the redundant power sources is down, the web management interface will look similar to:
<imgr src=images/non_redundant_power.png>
