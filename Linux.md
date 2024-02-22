## This note is for all Linux-like systems
##
## Find Out Which Process Listening on a Particular Port
'''
$ sudo ss -tulpn | grep 80
$ sudo lsof -i :80
$ sudo fuser 80/tcp
$ sudo netstat -ltnp | grep -w ':80' 
'''
## check remote port reachable
'''
# check single port
$ nc -zv 192.168.1.15 22
# port range
$ nc -zv 192.168.1.15 20-80
'''
