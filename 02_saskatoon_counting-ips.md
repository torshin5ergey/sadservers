# ["Saskatoon": counting IPs](https://sadservers.com/scenario/saskatoon)

**Description:** There's a web server access log file at `/home/admin/access.log`. The file consists of one line per HTTP request, with the requester's IP address at the beginning of each line.
Find what's the IP address that has the most requests in this file (there's no tie; the IP is unique). Write the solution into a file `/home/admin/highestip.txt`. For example, if your solution is "1.2.3.4", you can do `echo "1.2.3.4" > /home/admin/highestip.txt`

**Test:** The SHA1 checksum of the IP address `sha1sum /home/admin/highestip.txt` is `6ef426c40652babc0d081d438b9f353709008e93` (just a way to verify the solution without giving it away.)

## References

- [man sort](https://www.man7.org/linux/man-pages/man1/sort.1.html)

## Solution

- Check log file
```bash
head -5 /home/admin/access.log
# output
83.149.9.216 - - [17/May/2015:10:05:03 +0000] "GET /presentations/logstash-monitorama-2013/images/kibana-search.png HTTP/1.1" 200 203023 "http://semicomplete.com/presentations/logstash-monitorama-2013/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/32.0.1700.77 Safari/537.36"
83.149.9.216 - - [17/May/2015:10:05:43 +0000] "GET /presentations/logstash-monitorama-2013/images/kibana-dashboard3.png HTTP/1.1" 200 171717 "http://semicomplete.com/presentations/logstash-monitorama-2013/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/32.0.1700.77 Safari/537.36"
83.149.9.216 - - [17/May/2015:10:05:47 +0000] "GET /presentations/logstash-monitorama-2013/plugin/highlight/highlight.js HTTP/1.1" 200 26185 "http://semicomplete.com/presentations/logstash-monitorama-2013/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/32.0.1700.77 Safari/537.36"
83.149.9.216 - - [17/May/2015:10:05:12 +0000] "GET /presentations/logstash-monitorama-2013/plugin/zoom-js/zoom.js HTTP/1.1" 200 7697 "http://semicomplete.com/presentations/logstash-monitorama-2013/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/32.0.1700.77 Safari/537.36"
83.149.9.216 - - [17/May/2015:10:05:07 +0000] "GET /presentations/logstash-monitorama-2013/plugin/notes/notes.js HTTP/1.1" 200 2892 "http://semicomplete.com/presentations/logstash-monitorama-2013/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/32.0.1700.77 Safari/537.36"
```

- Find the most frequent IP
```bash
# awk '{print $1}' /home/admin/access.log - print only first column for every row in the file
# sort - ascending sort ip. uniq works only with sequential identical lines, so sorting is necessary
# uniq -c - delete duplicates, -c count numbers of duplicates
# sort -nr - sort, -n strings as digits, -r reverse/descending
# head -1 - show only first row
awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -nr | head -1
```

- Write the result IP into `/home/admin/highestip.txt`

## Author

Sergey Torshin [@torshin5ergey](https://github.com/torshin5ergey)
