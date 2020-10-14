## Devices - Common (EC2 and Edge)

The following are the checks to identify the reasons for player restarts.

#### Has host machine or EC2 virtual machine restarted?

1. Check for “DEVICE BOOTED” log

In /mnt/ops/logs/darti_<date>.log:
  
```
  Sep 28 09:03:41 localhost bash[76]: com.amagi.additional_message: ***** DEVICE BOOTED *****
```

This indicates that a maintanance reboot of virtual machine in case of EC2 instance or due to power fluctuation in case of EDGE device.
