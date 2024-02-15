## This document applies to Linux distros based on Debian, such as Ubuntu, Kali
## [Debian-based distributions](https://en.wikipedia.org/wiki/Category:Debian-based_distributions)
## check ssh status
$ systemctl status ssh

## check sudo status
$ sudo -l
$ sudo vi /etc/sudoers
$ sudo usermod -G <sudo group> <username>

## enable BBR if needed
## Check if BBR is Supported on your Debian System
$ sudo modprobe tcp_bbr
## enable the BBR parameter
$ sudo sh -c 'echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf'
$ sudo sh -c 'echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf'
## apply the changes, run the following command:
$ sudo sysctl -p
## Verify that BBR is Now Enabled on Debian 12, 11, or 10
$ sudo sysctl net.ipv4.tcp_congestion_control
## If BBR is enabled, you’ll see the following output:
## net.ipv4.tcp_congestion_control = bbr

## update the system
$ sudo apt update && sudo apt-get -y upgrade && sudo apt-get -y dist-upgrade && sudo shutdown -r now
$ sudo apt-get -y autoremove

## check system info
$ cat /etc/os-release
$ uname -a

## install often used tools
$ sudo apt-get install atop htop iotop speedtest-cli git

## check python3 version
$ python3 --version
$ sudo apt-get install python3-pip
$ pip --version

