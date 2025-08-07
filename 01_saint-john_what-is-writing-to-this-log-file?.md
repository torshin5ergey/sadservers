# ["Saint John": what is writing to this log file?](https://sadservers.com/scenario/saint-john)

**Description:** A developer created a testing program that is continuously writing to a log file `/var/log/bad.log` and filling up disk. You can check for example with `tail -f /var/log/bad.log`.
This program is no longer needed. Find it and terminate it. Do not delete the log file.

**Test:** The log file size doesn't change (within a time interval bigger than the rate of change of the log file).

## References

- [man lsof](https://linux.die.net/man/8/lsof)

## Solution

- Find the process that is using `/var/log/bad.log` file
```bash
lsof /var/log/bad.log
# output
COMMAND   PID  USER   FD   TYPE DEVICE SIZE/OFF  NODE NAME
badlog.py   7 admin    3w   REG  0,706   205472 20099 /var/log/bad.log
```

- Kill the process
```bash
kill 7
```

## Author

Sergey Torshin [@torshin5ergey](https://github.com/torshin5ergey)
