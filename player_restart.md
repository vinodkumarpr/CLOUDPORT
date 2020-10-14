## Devices - Common (EC2 and Edge)

The following are the checks to identify the reasons for player restarts.

#### Has host machine or EC2 virtual machine restarted?

Check for “DEVICE BOOTED” log in /mnt/ops/logs/darti_<date>.log:
  
```
  Sep 28 09:03:41 localhost bash[76]: com.amagi.additional_message: ***** DEVICE BOOTED *****
```

This indicates that a maintanance reboot of virtual machine in case of EC2 instance or due to power fluctuation in case of EDGE device.

Recommended Actions:

1. Was there a planned reboot at data center or power fluctuation that resulted in **Edge device** reboot?
2. Was there an reboot of underlying hardware for an **EC2 instance**?

### Was there crash during the log rotate? (A known issue)

If a player crash is observed around **00:00** or between **03:00 - 03:30**, then it could be because of _top/iotop/syslog_ compression. In this case the gzip process will be taking IO at the time of crash.

Check /mnt/ops/logs/darti_iotop_<date>.log
  
```
          00:00:19 29362 be/7 root        0.00 B/s 2000.52 K/s  0.00 %  40.1 % gzip
```
  
