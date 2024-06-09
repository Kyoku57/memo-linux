# Formations Linux

Courses reference : 
- https://www.udemy.com/course/linux-foundation-certified-systems-administrator-lfcs

## Essential commands

Documentation

```bash
sudo mandb
man man
man 1 prinf
apropos director
apropos -s 1,8 director # sections 1 8
```

Login 

```bash
ssh -V # version
ssh -v # verbose mode
hostnamectl
```

Variables

```bash
myVariable="www.point.lu/myfile.txt"
export myVariable
echo ${var:-prout}      # prout but var != prout
echo ${var:=prout}      # prout but var = prout
echo ${var:?sub}
echo ${var:+sub}
echo ${var:offset:length}
echo ${myVariable:5:2}  # oi
echo ${myVariable/w/z}  # ww.point.lu/myfile.txt
echo ${myVariable##*.}  # txt
echo ${myVariable%%.*}  # www
echo ${myVariable%.*}   # www.point.lu/myfile
```

Flux redirections
```bash
> 
>>
2> 
2>&1
'' => no interpretation
```

Parameters
```bash
$! # last result
$# # size
$0 # command
$1-9 # parameters
$@ # all parameters
```

- find
- cut
- cat | tac 
- sort
- ln (hard/soft)
- status
- sed 
- grep / egrep
- apropos / man
- if
- for 
- shell args

## Operations Deployment

- lsof
- ps
- lsusb
- lspci
- lscpu
- free
- fg 
- kill signals
- servicectl
- journalctl

Systemctl

Different kind of Units:
- service
- socket 
- device 
- timer

```bash
sudo systemctl stop|start|restart|reload|status sshd.service
sudo systemctl enable|disable --now sshd.service
sudo systemctl is-enabled sshd.service
sudo systemctl cat sshd.service
sudo systemctl mask|unmask atd.service
sudo systemctl list-units --type service --all
```

Create service

```bash
echo "My App Started" | system-cat -t MyApp -p info
sleep 5
echo "My App Crashed" | system-cat -t MyApp -p err
```

```bash
sudo cp /lib/systemd/system/ssh.service /etc/systemd/system/myapp.service
```

```properties
[Unit]
Description=MyApp
After=network.target auditd.service

[Service]
ExecStartPre=echo "Systemd is preparing to start MyApp"
ExecStart=/usr/local/bin/myapp.sh
KillMode=process
Restart=always
RestartSec=1
Type=simple

[Install]
WantedBy=multi-user.target
```
```bash 
sudo systemctl daemon-reload
sudo systemctl start myapp.service
```


## User and Group Management

- useradd -aG --home --shell
- usermod
- userdel
- gpasswd
- groups
- groupdel
- groupadd
- passwd
- sudoers
- PAM


## Networking

- ip -c link
- ip -a
- add ip delete address
- ss ltunp
- netstat
- cat /proc/net/bonding/bond0

Network configuration

```bash
sudo cp /usr/share/doc/examples/gateway.yaml /etc/netplan/99-gateway.yaml
sudo netplan try
sudo netplan apply
```

Firewall (UFW)

```bash
sudo ufw status
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status verbose

ss -tn

sudo ufw allow from 192.168.1.60 to any port 22
sudo ufw status numbered
sudo ufw delete 1
sudo ufw delete allow 22
sudo ufw allow from 10.11.12.0/24 to any port 22
sudo ufw deny from 10.11.12.100
sudo ufw status numbered
sudo ufw delete 4
sudo ufw insert 3 deny from 10.11.12.100

sudo ufw deny out on enp0s3 to 8.8.8.8
sudo ufw allow in on enp0s3 from 192.168.160 to 192.168.1.81 port 80 protocol tcp
sudo ufw allow out on enp0s3 from 192.168.1.81 to 192.168.1.60 port 80 protocol tcp
```

Packet filtering (firewallD)

```bash
sudo firewall-cmd --get-default-zone
sudo firewall-cmd --get-default-zone=public
sudo firewall-cmd --list-all
sudo firewall-cmd --info-service=cockpit
sudo firewall-cmd --add-service=http
sudo firewall-cmd --add-port=80/tcp
sudo firewall-cmd --remove-service=http
sudo firewall-cmd --remove-port=80/tcp
sudo firewall-cmd --add-source=10.11.12.0/24 --zone=trusted
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --remove-source=10.11.12.0/24 --zone=trusted
# permanent
sudo firewall-cmd --runtime-to-permanent
sudo firewall-cmd --add-port=80/tcp --permanent
```

Static routing IP traffic

```bash
sudo ip route add 192.168.0.0/24 via 10.0.0.100
sudo ip route add 192.168.0.0/24 via 10.11.12.100
sudo ip route add 192.168.0.0/24 via 10.11.12.100 dev enp0s3
sudo ip route del 192.168.0.0/24
sudo ip route add default via 10.0.0.100 #gateway
sudo ip route del default via 10.0.0.100
# permanent
nmcli connection show
sudo nmcli connection modify enp0s3 +ipv4.routes "192.168.0.0/24 10.0.0.100"
sudo nmcli device reapply enp0s3
ip route show
sudo nmcli connection modify enp0s3 -ipv4.routes "192.168.0.0/24 10.0.0.100"
sudo nmcli device reapply enp0s3
sudo nmtui
```

Timezone

```bash
timedatectl list-timezones | grep America > /home/bob/america
sudo timedatectl set-timezone Asia/Kolkata
timedatectl # see configuration

# systemd-timesyncd
sudo apt install systemd-timesyncd
sudo timedatectl set-ntp true
systemctl status systemd-timesync.service
cat /etc/systemd/timesyncd.conf # edit NTP server
timedatectl show-timesync
timedatectl timesync-status

# chrony
sudo yum install chrony -y
sudo systemctl start chronyd
sudo systemctl enable chronyd
sudo timedatectl show
sudo timedatectl set-ntp true
sudo timedatectl set-local-rtc 1
```

Redirection

```bash
/etc/sysctl.conf
/etc/sysctl.d/99-sysctl.conf
sudo sysctl --system
sudo sysctl -a | grep forward
nft / iptables --flush --table nat
Port redirection NAT
```


## Service Configuration

Not mandatory


## Storage Management



## Need to be seen

- AppArmor
- Starting/boot