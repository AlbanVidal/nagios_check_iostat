nagios check iostat
=========================

Check iostat of physical devices

# Dependances

`iostat`, `column`, `awk` and `sfdisk` binarys are required, to install on Debian/Ubuntu system:
``` shell
# Debian 9 / Ubuntu <= 17.04
apt install sysstat gawk bsdmainutils util-linux

# Debian 10 / Ubuntu 17.10
apt install sysstat gawk bsdmainutils fdisk
```

# Usage
``` shell
check_iostat [options]
Options:
    -d, --devices  <disks_expr>    Expression of disks search (defaut: sd|vd|mmc)
    -i, --interval <interval>      Specifies the amount of time in seconds between each report (between 1-10, default: 5)
    -w, --warning  <percent>       Define usage warning percent (between 0-100, default: 80)
    -c, --critical <percent>       Define usage critical percent (between 0-100, default: 100)
    -p, --perf-data                Enable performance data output
    -e, --examples                 Show usage examples
    -h, --help                     Print this help
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

# On Raspberry Pi (with Raspbian):
check_iostat
  OK
  State:  Device:  rrqm/s  wrqm/s  r/s   w/s   rMB/s  wMB/s  avgrq-sz  avgqu-sz  await  r_await  w_await  svctm  %util
  OK      mmcblk0  0.00    0.00    0.00  0.00  0.00   0.00   0.00      0.00      0.00   0.00     0.00     0.00   0.00

# Change interval, only in sda and sdb, warning 50:
check_iostat -i 1 -d 'sda|sdd' -w 50
  CRITICAL,  see_details_above:
  State:     Device:             rrqm/s  wrqm/s  r/s   w/s       rMB/s  wMB/s  avgrq-sz  avgqu-sz  await  r_await  w_await  svctm  %util
  CRIT       sda                 0,00    86,00   0,00  35784,00  0,00   10,16  0,58      8,91      0,24   0,00     0,24     0,03   94,80
  OK         sdd                 0,00    0,00    0,00  0,00      0,00   0,00   0,00      0,00      0,00   0,00     0,00     0,00   0,00
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
