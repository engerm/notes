# Installing BlueHydra / blue_hydra / Blue Hydra - 2018110

Not sure what the proper name if for BlueHydra / blue_hydra / Blue Hydra, but here are the steps to install and configure.

There is an issue running in daemon mode. Note on the bottom reflect the troubleshooting.

Have not configured it with the Parani-UD100 Bluetooth 4.0 Class1 USB Adapter because it has not arrived yet.

All commands are executed as root; except for bundling and executing blue_hydra.


## Hardware.

Just using the Bluetooth within the Raspberry Pi works well in many scenerios. Tested Raspberry Pi in a basement, and it was able to pick up devices walking down the street, approximately 20 feet away and devices on the 2nd floor. 


## resolve dependencies

The following need to be installed:
```
bluez bluez-test-scripts python-bluez python-dbus libsqlite3-dev ubertooth
```

1. Optional: check dependancies. Execute:

    ```
dpkg -l | egrep "bluez|libsqlite3-dev|ubertooth|dbus|ruby-dev|bundler"
    ```

 * result for default Kali Pi/Arm install:

    ```
ii  at-spi2-core                         2.30.0-2                    armhf        Assistive Technology Service Provider Interface (dbus core)
ii  bluez                                5.50-1                      armhf        Bluetooth tools and daemons
ii  bluez-firmware                       1.2-4                       all          Firmware for Bluetooth devices
ii  bundler                              1.16.1-3                    all          Manage Ruby application dependencies
ii  dbus                                 1.12.10-1                   armhf        simple interprocess messaging system (daemon and utilities)
ii  dbus-user-session                    1.12.10-1                   armhf        simple interprocess messaging system (systemd --user integration)
ii  dbus-x11                             1.12.10-1                   armhf        simple interprocess messaging system (X11 deps)
ii  libdbus-1-3:armhf                    1.12.10-1                   armhf        simple interprocess messaging system (library)
ii  libdbus-glib-1-2:armhf               0.110-3                     armhf        deprecated library for D-Bus IPC
ii  libdbusmenu-glib4:armhf              16.04.1+18.04.20171206-1    armhf        library for passing menus over DBus
ii  libdbusmenu-gtk3-4:armhf             16.04.1+18.04.20171206-1    armhf        library for passing menus over DBus - GTK-3+ version
ii  libnet-dbus-perl                     1.1.0-4+b3                  armhf        Perl extension for the DBus bindings
ii  libqt5dbus5:armhf                    5.11.1+dfsg-9               armhf        Qt 5 D-Bus module
ii  python-dbus                          1.2.8-2+b1                  armhf        simple interprocess messaging system (Python interface)
ii  ruby-bundler                         1.16.1-3                    all          Manage Ruby application dependencies (runtime)
ii  ruby-dev:armhf                       1:2.5.1                     armhf        Header files for compiling extension modules for Ruby (default version)
    ```

1. Install the following dependancies as root:

    ```
apt-get -yf install bluez bluez-test-scripts python-bluez python-dbus libsqlite3-dev ubertooth
    ```

1. Optional: check dependancies. Execute:

    ```
dpkg -l | egrep "bluez|libsqlite3-dev|ubertooth|dbusruby-dev|bundler"
    ```

 * result for default Kali Pi/Arm install:

    ```
ii  at-spi2-core                         2.30.0-2                    armhf        Assistive Technology Service Provider Interface (dbus core)
ii  bluez                                5.50-1                      armhf        Bluetooth tools and daemons
ii  bluez-firmware                       1.2-4                       all          Firmware for Bluetooth devices
ii  bluez-test-scripts                   5.50-1                      all          test scripts of bluez
ii  bundler                              1.16.1-3                    all          Manage Ruby application dependencies
ii  dbus                                 1.12.10-1                   armhf        simple interprocess messaging system (daemon and utilities)
ii  dbus-user-session                    1.12.10-1                   armhf        simple interprocess messaging system (systemd --user integration)
ii  dbus-x11                             1.12.10-1                   armhf        simple interprocess messaging system (X11 deps)
ii  libdbus-1-3:armhf                    1.12.10-1                   armhf        simple interprocess messaging system (library)
ii  libdbus-glib-1-2:armhf               0.110-3                     armhf        deprecated library for D-Bus IPC
ii  libdbusmenu-glib4:armhf              16.04.1+18.04.20171206-1    armhf        library for passing menus over DBus
ii  libdbusmenu-gtk3-4:armhf             16.04.1+18.04.20171206-1    armhf        library for passing menus over DBus - GTK-3+ version
ii  libnet-dbus-perl                     1.1.0-4+b3                  armhf        Perl extension for the DBus bindings
ii  libqt4-dbus:armhf                    4:4.8.7+dfsg-17             armhf        Qt 4 D-Bus module
ii  libqt5dbus5:armhf                    5.11.1+dfsg-9               armhf        Qt 5 D-Bus module
ii  libqtdbus4:armhf                     4:4.8.7+dfsg-17             armhf        Qt 4 D-Bus module library
ii  libsqlite3-dev:armhf                 3.25.2-1                    armhf        SQLite 3 development files
ii  libubertooth1:armhf                  2018.08.R1-3                armhf        Shared library for Bluetooth experimentation
ii  python-bluez                         0.22+really0.22-1           armhf        Python 2 wrappers around BlueZ for rapid bluetooth development
ii  python-dbus                          1.2.8-2+b1                  armhf        simple interprocess messaging system (Python interface)
ii  qdbus                                4:4.8.7+dfsg-17             armhf        Qt 4 D-Bus tool
ii  ruby-bundler                         1.16.1-3                    all          Manage Ruby application dependencies (runtime)
ii  ruby-dev:armhf                       1:2.5.1                     armhf        Header files for compiling extension modules for Ruby (default version)
ii  ubertooth                            2018.08.R1-3                armhf        2.4 GHz wireless development platform for Bluetooth experimentation
ii  ubertooth-firmware                   2018.08.R1-3                all          Firmware for Ubertooth
    ```

## get blue_hydra


1. create blue_hydra source directory. Execute:

    ```
mkdir /usr/local/src/blue_hydra_`date +%Y%m%d`
    ```

1. get blue_hydra source. Execute:

    ```
git clone https://github.com/pwnieexpress/blue_hydra.git /usr/local/src/blue_hydra_`date +%Y%m%d`
    ```

## '_bundle_' blue_hydra

1. Change into the blue_hydra source directory and ensure contents were downloaded corrected. Execute:

    ```
cd /usr/local/src/blue_hydra_`date +%Y%m%d`; ls
    ```

1. Set permissions for the non-root user. Execute:

    ```
chown blueh:blueh -R /usr/local/src/blue_hydra_`date +%Y%m%d`
    ```

1. Change to another user; recommend not bundling as root:

    ```
su blueh
    ```
        
1. run bundler (as non-root user). Execute:

    ```
bundle install
    ```
    
 * Results:

    ```
Don't run Bundler as root. Bundler can ask for sudo if it is needed, and installing your bundle as root will break this application for all non-root users on this
machine.
Fetching gem metadata from http://rubygems.org/........
Resolving dependencies...
Using rake 12.3.1
Fetching public_suffix 3.0.3
Installing public_suffix 3.0.3
Using addressable 2.5.2
Using bundler 1.16.1
Fetching coderay 1.1.2
Installing coderay 1.1.2
Fetching data_objects 0.10.17
Installing data_objects 0.10.17
Fetching diff-lcs 1.3
Installing diff-lcs 1.3
Fetching dm-core 1.2.1
Installing dm-core 1.2.1
Fetching dm-do-adapter 1.2.0
Installing dm-do-adapter 1.2.0
Fetching dm-migrations 1.2.0
Installing dm-migrations 1.2.0
Fetching do_sqlite3 0.10.17
Installing do_sqlite3 0.10.17 with native extensions
Fetching dm-sqlite-adapter 1.2.0
Installing dm-sqlite-adapter 1.2.0
Fetching dm-timestamps 1.2.0
Installing dm-timestamps 1.2.0
Fetching dm-validations 1.2.0
Installing dm-validations 1.2.0
Fetching louis 2.0.7
Installing louis 2.0.7
Fetching method_source 0.9.1
Installing method_source 0.9.1
Fetching pry 0.12.0
Installing pry 0.12.0
Fetching rspec-support 3.8.0
Installing rspec-support 3.8.0
Fetching rspec-core 3.8.0
Installing rspec-core 3.8.0
Fetching rspec-expectations 3.8.2
Installing rspec-expectations 3.8.2
Fetching rspec-mocks 3.8.0
Installing rspec-mocks 3.8.0
Fetching rspec 3.8.0
Installing rspec 3.8.0
Bundle complete! 8 Gemfile dependencies, 22 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
    ```

## run application

Run as root.
    
1. start application

    ```
bundle exec ./bin/blue_hydra
    ```    

2. Press enter.


## additional configurations

From within the /usr/local/src/blue_hydra_[CCYYMMDD] directory, the following configurations were made.

Current config:
```
cat blue_hydra.yml
log_level: info
bt_device: hci0
info_scan_rate: 240
btmon_log: false
btmon_rawlog: false
file: false
rssi_log: false
aggressive_rssi: false
ui_filter_mode: :disabled
ui_inc_filter_mac: []
ui_inc_filter_prox: []
signal_spitter: false
chunker_debug: false
```

1. Copy existing yml config. Execute:

    ```
cp blue_hydra.yml blue_hydra.yml.`date +%Y%m%d`.orig
    ```

2. Make some changes. Execute:

    ```
cat blue_hydra.yml
log_level: info
bt_device: hci0
info_scan_rate: 60
btmon_log: true
btmon_rawlog: true
file: false
rssi_log: true
aggressive_rssi: false
ui_filter_mode: :disabled
ui_inc_filter_mac: []
ui_inc_filter_prox: []
signal_spitter: false
chunker_debug: false  
    ```

3. Run in deamon mode. 

    ```
bundle exec ./bin/blue_hydra -d &
    ```     

## warning about the Fixnum issue

When running in daemon mode, there was the following warning; runs blue_hydra runs fine so this can be ignored:

```
bundle exec ./bin/blue_hydra -d
/var/lib/gems/2.5.0/gems/data_objects-0.10.17/lib/data_objects/pooling.rb:149: warning: constant ::Fixnum is deprecated
```

Current setup is:

```
ruby -v
ruby 2.5.1p57 (2018-03-29 revision 63029) [arm-linux-gnueabihf]
```

and

```
gem list

*** LOCAL GEMS ***

activesupport (4.2.10)
addressable (2.5.2)
atomic (1.1.16)
bigdecimal (default: 1.3.4)
bundler (1.16.1)
cmath (default: 1.0.0)
cms_scanner (0.0.40.1)
coderay (1.1.2)
csv (default: 1.0.0)
data_objects (0.10.17)
date (default: 1.0.0)
dbm (default: 1.0.0)
did_you_mean (1.2.1)
diff-lcs (1.3)
dm-core (1.2.1)
dm-do-adapter (1.2.0)
dm-migrations (1.2.0)
dm-sqlite-adapter (1.2.0)
dm-timestamps (1.2.0)
dm-validations (1.2.0)
do_sqlite3 (0.10.17)
etc (default: 1.0.0)
ethon (0.9.0)
fcntl (default: 1.0.0)
ffi (1.9.10)
fiddle (default: 1.0.0)
fileutils (default: 1.0.2)
gdbm (default: 2.0.0)
i18n (0.7.0)
io-console (default: 0.4.6)
ipaddr (default: 1.2.0)
json (default: 2.1.0)
louis (2.0.7)
method_source (0.9.1)
mime (0.4.4)
mime-types (3.2.2)
mime-types-data (3.2015.1120)
mini_exiftool (2.9.0)
minitest (5.11.3)
molinillo (0.6.4)
net-http-digest_auth (1.4)
net-http-persistent (2.9.4)
net-telnet (0.1.1)
nokogiri (1.8.4)
openssl (default: 2.1.0)
opt_parse_validator (0.0.16.2)
pkg-config (1.3.1)
power_assert (1.1.1)
pry (0.12.0)
psych (default: 3.0.2)
public_suffix (3.0.3, 2.0.5)
rake (12.3.1)
rdoc (default: 6.0.1)
rspec (3.8.0)
rspec-core (3.8.0)
rspec-expectations (3.8.2)
rspec-mocks (3.8.0)
rspec-support (3.8.0)
ruby-progressbar (1.9.0)
rubyzip (1.2.1)
scanf (default: 1.0.0)
sdbm (default: 1.0.0)
spider (0.32)
stringio (default: 0.0.1)
strscan (default: 1.0.0)
test-unit (3.2.8)
thor (0.19.4)
thread_safe (0.3.6)
typhoeus (1.3.0)
tzinfo (1.2.5)
webrick (default: 1.4.2)
wpscan (3.3.1)
xmlrpc (0.3.0)
yajl-ruby (1.3.1)
zlib (default: 1.0.0)
```

Fixnum is referenced in two (2) files.
 
```
grep -rnw -e "Fixnum"
spec/runner_spec.rb:15:    expect(created_device.last_seen.class).to eq(Fixnum)
lib/blue_hydra/cli_user_interface.rb:275:            if scanner_status[:ubertooth].class == Fixnum
```

***Solution*** - do nothing. It's just a warning and things are still working. Tried updating Rails and Ruby and things broke.


## resources

* BlueHydra's README.md: [pwnieexpress/blue_hydra/README.md](https://github.com/pwnieexpress/blue_hydra)
* BlueHydra install instructions: [Blue Hydra Installation Instructions](https://www.pwnieexpress.com/blog/blue-hydra-installation-instructions)
* Install instructions: [Detect Bluetooth Low Energy Devices in Realtime with Blue Hydra](https://null-byte.wonderhowto.com/how-to/detect-bluetooth-low-energy-devices-realtime-with-blue-hydra-0179744/)
* Setup and install instructions: ["Finding and Tracking Bluetooth Devices - Tradecraft" by Hacker Warehouse](https://www.youtube.com/watch?v=UvfNjQFtp_A)