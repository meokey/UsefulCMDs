## This note is for all Linux-like systems
##
Find Out Which Process Listening on a Particular Port<p>
`
$ sudo ss -tulpn | grep 80
$ sudo lsof -i :80
$ sudo fuser 80/tcp
$ sudo netstat -ltnp | grep -w ':80' 
`<p>
Find out ports that listens other than local connections<p>
`
$ sudo ss -tulpn | grep -v "127.0.0."
`<p>

## check remote port reachable
check single port<p>
`
$ nc -zv 192.168.1.15 22
`<p>
# port range<p>
<p>
$ nc -zv 192.168.1.15 20-80
`<p>
