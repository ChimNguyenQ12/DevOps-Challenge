Provide your solution here:
free -h
ps aux --sort=-%mem | head -10
ps aux | grep nginx
netstat -ant | grep ESTABLISHED | wc -l
journalctl -u nginx --since "1 hour ago" | tail -20
dmesg | grep -i "out of memory"
grep 'worker_processes' /etc/nginx/nginx.conf
pmap -x $(pgrep nginx) | tail -10
tail -f /var/log/nginx/error.log


sudo systemctl restart nginx

