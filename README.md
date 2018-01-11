nagios check iostat
=========================

Check iostat of physical devices

# Usage
``` shell
check_iostat [options]
Options:
    -w,--warning <percent>     define usage warning percent (between 0-100, default: 80)
    -c,--critical <percent>    define usage critical percent (between 0-100, default: 100)
    -h,--help                print this help
```

# Example to add this in nrpe
 + Copy this script in `/usr/lib/nagios/plugins` directory

 + Create file `/etc/nagios/nrpe.d/check_iostat.cfg` with this content:
``` shell
command[check_iostat]=/usr/lib/nagios/plugins/check_iostat
```
 + Reload NRPE service
``` shell
systemctl reload nagios-nrpe-server.service
```
