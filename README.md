# SoftEther VPN

### Introduction
This article explains how to install and configure a SoftEther VPN.

### Installation


Using the command below, update and upgrade your server software packages to the latest version:
```
apt-get update
```

```
apt-get upgrade -y
```
Now using the command below to install SoftEther server requirements:
```
apt-get -y install build-essential wget make curl gcc  wget zlib1g-dev tzdata git libreadline-dev libncurses-dev libssl-dev
```

### Download SoftEther

You can download the latest SoftEther server package for Linux from their website:
[Download SoftEther](https://www.softether-download.com/en.aspx?product=softether)

```
Wget “download link”
```


```
Tar xzf “downloaded file name”
```

```
cd vpnserver
make
```


```
cd ..
mv vpnserver /usr/local
cd /usr/local/vpnserver/
chmod 600 *
chmod 700 vpnserver vpncmd
```


```
./vpnserver start
```

```
./vpncmd
```

Now your VPN server has been created and it’s ready. One last step is to change the Authentication mode to Password since that’s how we configured our user’s authentication mode in the server:
```
ServerPasswordSet
```


```
sudo cat >> /lib/systemd/system/vpnserver.service << EOF
[Unit]
Description=SoftEther VPN Server
After=network.target
[Service]
Type=forking
ExecStart=
ExecStart=/usr/local/vpnserver/vpnserver start
ExecStop=/usr/local/vpnserver/vpnserver stop
[Install]
WantedBy=multi-user.target
EOF
```


```
echo net.ipv4.ip_forward = 1 | ${SUDO} tee -a /etc/sysctl.conf
```

Then start the VPN client service using the following command:

```
systemctl enable vpnserver
systemctl start vpnserver
systemctl status vpnserver
```

```
ufw allow 443
ufw allow 500,4500/udp
ufw allow 1701
ufw allow 1194
ufw allow 5555
```
