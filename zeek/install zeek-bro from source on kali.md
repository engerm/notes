# Install zeek/bro from source on Kali 2018.4 AMD64

The following are basic steps for a base setup of a Kali VM on Virtual box, basic Kali configuration, zeek compile, and a standalone zeek configuration. The below are __NOT__ meant for a production server without further review and configuration. 

Kali version:

```
uname -a
Linux kali 4.18.0-kali2-amd64 #1 SMP Debian 4.18.10-2kali1 (2018-10-09) x86_64 GNU/Linux
```

## OPTIONAL: configure SSHD and apt-get


1. configure sshd. Execute the following:

    ```
    update-rc.d -f ssh remove
    update-rc.d -f ssh defaults
    mkdir /etc/ssh/orig-install-keys/
    mv /etc/ssh/ssh_host_* /etc/ssh/orig-install-keys/
    dpkg-reconfigure openssh-server
    sed -i -e "s/PermitRootLogin without-password/PermitRootLogin yes/g" /etc/ssh/sshd_config
    update-rc.d -f ssh enable 2 3 4 5
    sed -i '1i-- my internal private server --" /etc/motd
    service ssh restart
    ```

1. Verify sources.list points to the correct repo. May have to execute:

    ```
    sed -i -e "s/#deb src/deb src/g" /etc/apt/sources.list
    ```

1. Update and update Kali

    ```
    apt-get update
    apt-get upgrade
    ```

## Prepare installation

1. Install zeek development dependencies. Execute:

    ```
    apt-get install -yf cmake make gcc g++ flex bison libpcap-dev libssl-dev python-dev swig zlib1g-dev
    ```

1. OPTIONAL: Install additional zeek development libraries. Execute:

    ```
    apt-get install -yf libgeoip-dev libkrb5-dev
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

1. make the install. Execute (the following takes about an hour and 15 minutes):

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
    bro          standalone localhost     running   31057  09 Dec 08:26:56
    ```


## references
* [Installing Bro](https://www.bro.org/sphinx/install/install.html)
* [How to Install Bro on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-bro-on-ubuntu-16-04)