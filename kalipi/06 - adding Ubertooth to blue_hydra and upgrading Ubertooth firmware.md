# Adding Ubertooth to blue_hydra and upgrading Ubertooth firmware

Note: there was an issue upgrading the Ubertooth when the Ubertooth was plugged into a USB hub that was plugged into the KaliPi; some of these steps may not be needed if the Ubertooth is _directly_ plugged into the KaliPi or device.

## hardware

* KaliPi: Raspberry Pi 3 Model B+ running Kali linux
* Parani-UD100 Bluetooth 4.0 Class1 USB Adapter [Item Name/Code
: UD100-G03]
* NooElec R820T SDR & DVB-T NESDR Mini
* Ubertooth

## steps

Full and complete steps for upgrading Ubertooth firmware are on [Great Scott Gadgets](https://github.com/greatscottgadgets/ubertooth) github. 

1. Plug Ubertooth into running KaliPi

2. From blue_hydra working directory, execute:

    ```
    bundle exec ./bin/blue_hydra
    ```

 * result:

    ```
    Blue Hydra : Devices Seen in last 300s, processing_speed: 1/s, DB Stunned: false
    Queue status: result_queue: 0, info_scan_queue: 2, l2ping_queue: 1
    Discovery status timer: 18, Ubertooth status: Disabled, firmware upgrade required, Filter mode: disabled
    UUID     |  SEEN ^ | VERS | ADDRESS           | RSSI | MANUF
    20942112 |     +2s | BTLE | 21:12:BA:XX:XX:XX | -37  | Something...
    47ea2112 |     +7s | BTLE | 21:12:CD:XX:XX:XX | -45  | Something...
    ```

1. Create directory for Ubertooth firmware. Execute:

    ```
    mkdir /opt/ubertooth-upgrade
    ```

1. Change directories. Execute: 

    ```
    cd /opt/ubertooth-upgrade
    ```

1. Download the latest firmware. Execute:

    ```
    wget https://github.com/greatscottgadgets/ubertooth/releases/download/2018-08-R1/ubertooth-2018-08-R1.tar.xz
    ```

1. Extract downloaded files. Execute:

    ```
    tar -xf ubertooth-2018-08-R1.tar.xz
    ```
    
1. Change directory. Execute:

    ```
    cd ubertooth-2018-08-R1/ubertooth-one-firmware-bin
    ```
    
1. Upgrade the firmware. Execute:

    ```
    ubertooth-dfu -d bluetooth_rxtx.dfu -r
    ```
    
 * result:

    ```
    libUSB Error: Pipe error:  (-9)
        Switching to DFU mode...
        Checking firmware signature
        ........................................
        ........................................
        ........................................
        .
        control message unsupported
    ```
    
1. Reset the Ubertooth. Execute:

    ```
    ubertooth-util -r
    ```
    
 * result:
    
    ```
    Resetting ubertooth device number 0
    ```
    
1. Upgrade the firmware. Execute:

    ```
    ubertooth-dfu -d bluetooth_rxtx.dfu -r
    ```
    
 * result

    ```
    libUSB Error: Pipe error:  (-9)
        Switching to DFU mode...
        Checking firmware signature
        ........................................
        ........................................
        ........................................
        .
        control message unsupported
    ```

1. Unplug ubertooth
2. Plug ubertooth back in
1. Upgrade the firmware. Execute:

    ```
    ubertooth-dfu -d bluetooth_rxtx.dfu -r
    ```

 * result

    ```
    libUSB Error: Pipe error:  (-9)
        Switching to DFU mode...
        Checking firmware signature
        ........................................
        ........................................
        ........................................
        .
        control message unsupported
    ```

1. Unplug ubertooth (from the usb hub)
1. Plug ubertooth ***directly*** into the KaliPi/device - i.e. do no not plug into a USB hub but plug directly into
1. Upgrade the firmware. Execute:

    ```
    ubertooth-dfu -d bluetooth_rxtx.dfu -r
    ```
    
 * result

    ```
    Switching to DFU mode...
    Checking firmware signature
    ........................................
    ........................................
    ........................................
    .
    Detached
    ```
    
1. Change back to working blue_hydra directory
1. From blue_hydra working directory, execute:

    ```
    bundle exec ./bin/blue_hydra
    ```

 * result:

    ```
    Blue Hydra : Devices Seen in last 300s, processing_speed: 1/s, DB Stunned: false
    Queue status: result_queue: 0, info_scan_queue: 2, l2ping_queue: 0
    Discovery status timer: 18, Ubertooth status: 12, Filter mode: disabled
    ```


## resources:

* [Firmware](https://github.com/greatscottgadgets/ubertooth/wiki/Firmware)
* [Ubertooth 2018-08-R1 - the DEFCON release
](https://github.com/greatscottgadgets/ubertooth/releases/tag/2018-08-R1)

