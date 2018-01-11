nagios check iostat
=========================

Check iostat of physical devices

# Usage
``` shell
check_iostat [options]
Options:
    -d, --devices  <disks_expr>    expression of disks search (defaut: sd|mmc)
    -w, --warning  <percent>       define usage warning percent (between 0-100, default: 80)
    -c, --critical <percent>       define usage critical percent (between 0-100, default: 100)
    -e, --examples                 show usage examples
    -h, --help                     print this help
```

# Usage examples
``` shell
# Check only sda and sdb devices:
check_iostat --devices 'sda|sdb'
    OK
    State:  Device:  rrqm/s  wrqm/s  r/s   w/s     rMB/s  wMB/s  avgrq-sz  avgqu-sz  await  r_await  w_await  svctm  %util
    OK      sdb      0,00    0,00    0,00  0,00    0,00   0,00   0,00      0,00      0,00   0,00     0,00     0,00   0,00
    OK      sda      0,00    163,40  0,00  592,80  0,00   3,16   10,90     1,44      2,44   0,00     2,44     0,03   1,92

# Check only sdd and md* devices:
check_iostat --devices 'sdd|md'
    OK
    State:  Device:  rrqm/s  wrqm/s  r/s   w/s   rMB/s  wMB/s  avgrq-sz  avgqu-sz  await  r_await  w_await  svctm  %util
    OK      sdd      0,00    0,00    0,00  1,60  0,00   0,00   3,00      0,01      4,00   0,00     4,00     4,00   0,64
    OK      md0      0,00    0,00    0,00  0,00  0,00   0,00   0,00      0,00      0,00   0,00     0,00     0,00   0,00

# Change range Waning and Critical:
check_iostat -w 90 -c 95
    OK
    State:  Device:  rrqm/s  wrqm/s  r/s   w/s      rMB/s  wMB/s  avgrq-sz  avgqu-sz  await  r_await  w_await  svctm  %util
    OK      sdb      0,00    0,00    0,00  0,00     0,00   0,00   0,00      0,00      0,00   0,00     0,00     0,00   0,00
    OK      sda      0,00    283,80  0,00  1399,80  0,00   7,62   11,15     5,01      3,58   0,00     3,58     0,04   5,04

# Change range Waning and Critical:
check_iostat -w 5 -c 10
    WARN,   see_details_above:
    State:  Device:             rrqm/s  wrqm/s  r/s   w/s      rMB/s  wMB/s  avgrq-sz  avgqu-sz  await  r_await  w_await  svctm  %util
    OK      sdb                 0,00    0,00    0,00  0,00     0,00   0,00   0,00      0,00      0,00   0,00     0,00     0,00   0,00
    WARN    sda                 0,00    253,80  0,40  1264,60  0,00   6,88   11,14     5,08      4,01   0,00     4,01     0,04   5,12
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
