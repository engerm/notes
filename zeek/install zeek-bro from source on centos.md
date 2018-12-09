# Install zeek/bro from source on CentOS 7.5 (CentOS-7-x86_64-DVD-1804.iso)

The following are basic steps for a base setup of a CentOS VM on Virtual box, basic CentOS configuration, zeek compile, and a standalone zeek configuration. The below are __NOT__ meant for a production server without further review and configuration. 

CentOS version used at time drafting documation:

```
cat /etc/centos-release
CentOS Linux release 7.5.1804 (Core)
```

## OPTIONAL: configure network interface, yum, and SSHD


1. configure networking. Edit `/etc/sysconfig/network-scripts/ifcfg-enp0s3`. Change on boot to "yes". Use `vi` to change:

    ```ONBOOT=no```
    
    to 
    
    ```ONBOOT=yes```

1. restart networking. Execute:

    ```
    service network restart
    ```

1. get ip address. Execute:

    ```
    ip a
    ```

1. install sshd. Execute:

    ```
    yum install -y openssh-server
    ```

1. configure sshd. Edit `/etc/ssh/sshd_config`. Change `#PermitRootLogin yes` to `PermitRootLogin yes`. Use `vi` to change:

    ```#PermitRootLogin yes``` 
    
    to 
    
    ```PermitRootLogin yes```
    
    Additionally the Protocol should be defined as `Protocol 2`. Also the default certs should also be removed and new certs should be generated.
    

1. configure sshd on boot. Execute:

    ```
    systemctl enable sshd.service
    ```

1. start sshd. Execute:
    
    ```systemctl restart sshd.service```

1. update CentOS. Execute:

    ```yum update -y```

1. install additional applications. Execute:

    ```
    yum install -y mlocate vim wget git epel-release
    ```

1. install pip after installing epel-release. Execute:

    ```
    yum install python-pip
    pip install --upgrade pip
    ```



## Prepare installation

1. Install zeek development dependencies. Execute:

    ```
    yum install -y cmake make gcc gcc-c++ flex bison libpcap-devel openssl-devel python-devel swig zlib-devel
    ```

1. OPTIONAL: Install additional zeek development libraries. Execute:

    ```
    yum install -y GeoIP-devel krb5-devel
    ```
    
    a. For GeoLite, download GeoLite mmdb file. Execute:
    
    ```
    mkdir /usr/local/src/GeoIP/`date +%Y%m%d` -p
    wget http://geolite.maxmind.com/download/geoip/database/GeoLite2-City.tar.gz -P /usr/local/src/GeoIP/`date +%Y%m%d`
    tar -zxf /usr/local/src/GeoIP/`date +%Y%m%d`/GeoLite2-City.tar.gz --directory /usr/local/src/GeoIP/`date +%Y%m%d`/
    cp /usr/local/src/GeoIP/`date +%Y%m%d`/GeoLite2-City_*/GeoLite2-City.mmdb /usr/share/GeoIP/GeoLite2-City.mmdb
    ```
 
1. Prepare and download zeek. Execute the following: 

    ```
    mkdir /usr/local/src/zeek/`date +%Y%m%d` -p
    cd /usr/local/src/zeek/`date +%Y%m%d`
    git clone --recursive git://github.com/zeek/zeek.git   
    cd zeek
    ```

## install zeek

1. Configure zeek with mobile IPv6; if not just `./configure`. Execute:

    ```
    ./configure --enable-mobile-ipv6
    ```

1. make the install. Execute (the following takes about 31 minutes):

    ```
    make
    ```

1. install zeek. Execute:

    ```
    make install
    ```

1. check version of bro. Execute:

    ```
    /usr/local/bro/bin/bro -v
    ```
    
    * result:

    ```
    /usr/local/bro/bin/bro version 2.6-20
    ```
    

## Configure environment post-zeek install

1. Configure environment. Execute: 

    ```
    export PATH=/usr/local/bro/bin:$PATH
    ```

1. check version of bro. Execute:

    ```
    bro -v
    ```
    
    * result:

    ```
    bro version 2.6-20
    ```

## Configure auto restart

1. Configure zeek cron. Execute:

    ```
    broctl cron enable
    ```

1. add to cron and check every five (5) minutes. Execute:

    ```
    echo "*/5 * * * * root /usr/local/bro/bin/broctl cron" > /etc/cron.d/bro
    ```

1. get current device interface. Execute:

    ```
    ip link show
    ```

    * result:

    ```
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 21:12:27:78:21:12 brd ff:ff:ff:ff:ff:ff
    ```

1. check zeek interface for standalone. check node.cfg. Execute:

    ```
    cat /usr/local/bro/etc/node.cfg | grep ^interface
    ```

    * result:

    ```
    interface=eth0
    ```

1. OPTIONAL: if the interface from `ip link show` is different from the interface in node.cfg, then update node.cfg setting. Change the interface setting in node.cfg to the interface identified in `ip link show`. 

    a. make copy of node.cfg

    ```
    cp /usr/local/bro/etc/node.cfg /usr/local/bro/etc/node.cfg.`date +%Y%m%d`.orig
    ```

    b. Use `vi` to change:

    ```interface=eth0``` 
    
    to 
    
    ```interface=enp0s3```


1. deploy default bro. Execute:

    ```
    broctl deploy
    ```

1. check status of zeek. Execute:

    ```
    broctl status
    ```

    * result:

    ```
    Name         Type       Host          Status    Pid    Started
    bro          standalone localhost     running   21595  07 Dec 19:03:30
    ```


## Troublueshooting

If the following error occurs on `broctl deploy` then interface setting in /usr/local/bro/etc/node.cfg needs to be configured:
 
```
...
...
...
==== stderr.log
fatal error: problem with interface eth0 (pcap_error: SIOCGIFHWADDR: No such device (pcap_activate))
...
...
...
```


## references

* [Installing Bro](https://www.bro.org/sphinx/install/install.html)
* [INSTALL BRO ON CENTOS 7.X/6.X](https://thecomputersecurityblog.wordpress.com/2015/03/17/install-bro-on-centos-7-x6-x/)
* [Zeek GeoLocation](https://www.bro.org/sphinx/frameworks/geoip.html)