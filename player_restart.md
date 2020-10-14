## Devices - Common (EC2 and Edge)

The following are the checks to identify the reasons for player restarts.

## Has host machine or EC2 virtual machine restarted?

Check for “DEVICE BOOTED” log in **/mnt/ops/logs/darti_\<date>.log**
  
```
  Sep 28 09:03:41 localhost bash[76]: com.amagi.additional_message: ***** DEVICE BOOTED *****
```

This indicates that a maintanance reboot of virtual machine in case of EC2 instance or due to power fluctuation in case of EDGE device.

**Recommended Actions**

1. Was there a planned reboot at data center or power fluctuation that resulted in **Edge device** reboot?
2. Was there an reboot of underlying hardware for an **EC2 instance**?

## Was there crash due to the log rotate bug? (A known issue)

If a player crash is observed around **00:00** or between **03:00 - 03:30**, then it could be because of _top/iotop/syslog_ compression. In this case the gzip process will be taking IO at the time of crash.

Check **/mnt/ops/logs/darti_iotop_\<date>.log**
  
```
          00:00:19 29362 be/7 root        0.00 B/s 2000.52 K/s  0.00 %  40.1 % gzip
```
  
**Recommended Actions**

There is no immediate recovery steps. The issue is resolved in the recent releases.

## Was there crash due to graphics load? (A known issue)


Check **/mnt/ops/logs/darti_iotop_\<date>.log** (_**top**_ log)

```
Tasks:  51 total,   3 running,  47 sleeping,   0 stopped,   1 zombie
%Cpu(s): 73.8 us, 11.6 sy,  0.0 ni, 14.5 id,  0.0 wa,  0.0 hi,  0.1 si,  0.0 st
```
CPU idle percentage < 20-25 is suspicious

Now check if high CPU load is because of a graphics issue.
Check for “Load too High” log before the player crashed. 

```
Sep 29 03:18:56 COLKA_001 player_app: ,WARN, com.amagi.amagi_classes.gfx_hndlr, Load too High ?? Last Average time taken to blend = 47.32. Warn count = 104. Logging interval = 5010 ms.
```

This indicates some issue with the graphics that are currently active.
Check for the below scenarios:

**Too many Graphics**

There are > 3-4 graphics playing which might cause high CPU load

```
Aug  7 05:56:07 CMHMM_001 player_app: ,INFO, com.amagi.amagi_classes.gfx_hndlr, Currently total queued graphics = 4. Currently active graphics = 4
```
**Recommended Action**
Check with L3 if these many graphics are supported in the release.

**Too big Graphics**

Also check if the currently scheduled graphics that are **big files**. Check for the graphics in **/mnt/ops/bugs/\<gfx>** directory. If the .tga files are around **1 MB or greater**, it might cause high CPU load issues. 
  
**Recommended Action**

Check if this graphics played fine earlier and if it did, check with L3.


