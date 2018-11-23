# Configure Kali on Raspberry Pi - 2018110

Configure the KaliPi - _i.e. basic linux configuration_.

All commands are executed as root.


## boot

* plug in keyboard and mouse
* insert SD card
* turn on
* log in as root / toor


## change root password

1. Execute the following to change the password:

    ```
    passwd
    ```

2. Optional: create another user and disable SSH for root.

    
## connect to network

1. Either connect to local Wi-Fi or plug in local cable


## configure ssh

1. Execute the following. _openssh-server_ is already installed.

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

2. Get IP address
3. ssh into Kali Pi


## update pi

1. Verify sources.list points to the correct repo. May have to execute:

    ```
    sed -i -e "s/#deb src/deb src/g" /etc/apt/sources.list
    ```

1. get current version. execute:

    ```
    uname -a
    ```

 * result:

    ```
    Linux kali 4.14.71-kali-v7+ #1 SMP Sun Oct 14 00:17:58 UTC 2018 armv7l GNU/Linux
    ```

2. Update and update Kali

    ```
    apt-get update
    apt-get upgrade
    ```

## OPTIONAL: create user and group

This will create a user called 'blueh' that will be used to execute blue hydra; username irrelevant and can be changed.

1. Create user and allow for SSH. Execute:

    ```
    useradd -m -s $(which bash) -G sudo -d /home/ blueh
    ```

2. Set password. Execute:

    ```
    passwd blueh
    ```
    
## OPTIONAL: install other useful apps

1. Execute:

    ```
    apt-get -yf install sqlite3
    ```
    
