# Install Kali on Raspberry Pi - 2018110

Here are steps to get a KaliPi going.

## equipment

* Raspberry Pi 3 Model B+
* Samsung EVO Plus 32gb SD card
* Parani-UD100 Bluetooth 4.0 Class1 USB Adapter [Item Name/Code
: UD100-G03]
 * 300m Working Distance, Exchangeable Antenna, BlueSoleil Driver" 

Install steps are using a MacBook Pro running macOS Majove (version 10.14).


## put image on SD card

Steps for the Raspberry Pi 3 Model B+:

1. Download arm image from [Kali](https://www.offensive-security.com/kali-linux-arm-images/): Kali Linux RaspberryPi 2 and 3
  * Downloaded _kali-linux-2018.4-rpi3-nexmon.img.xz_ 
  
1. Verify sha256. Execute the following command on the download file and ensure the sha256 from the downloaded file matches that sha256 on the Kali site. Execute:

    ```
shasum -a 256 *
    ```
 * Result:

    ```
2504c2607c46a73c9c49e138aec8ce7e98dc7a8effbaec75cf7e59c89eba309a  kali-linux-2018.4-rpi3-nexmon.img
ae953fbbe5d161baab321f5a9cd4e1899789b4e924aee015516db5787f3a16f5  kali-linux-2018.4-rpi3-nexmon.img.xz
    ```

2. Extract img:
    ```
unxz --keep ~/Desktop/raspberry-pi/rpi/kali-linux-2018.4-rpi.img.xz
    ``` 

2. Put SD card in mac 
3. Launch Disk Utility app, select SD card - e.g. 32gb. Click Erase. Set as follows:
 * Name: KALI-PI-ARM
 * Format: MS-DOS (FAT)
 * Scheme: Master Boot Record
 * Click Erase    
4. Locate SD card's path. Execute:

    ```
diskutil list
    ```
    
 * result:
 
    ```
/dev/disk2 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *32.0 GB    disk2
   1:                 DOS_FAT_32 KALI                    32.0 GB    disk2s1
    ```
    
5. Unmounting the drive.  Will use _disks2s1_; note _disks2s1_ is for `unmount` and changes to _rdisks2s1_ for `dd`

    ```
sudo diskutil unmount disk2s1
    ```

 * result:

    ```
Volume KALI-PI-ARM on disk2s1 unmounted
    ```
 
1. Write image to SD card:

    ```   
sudo dd if=kali-linux-2018.4-rpi3-nexmon.img of=/dev/rdisk2 bs=1m
    ```

 * Optional: as image is being written to disk, CONTROL + T will show the current status.
 * result:

    ```
4291+1 records in
4291+1 records out
4499999744 bytes transferred in 135.691864 secs (33163372 bytes/sec)
    ```

9. Check out disk util again

    ```
diskutil list
    ```
    
 * result:
 
    ```
/dev/disk2 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *32.0 GB    disk2
   1:             Windows_FAT_32 NO NAME                 64.0 MB    disk2s1
   2:                      Linux                         4.4 GB     disk2s2
    ```

4. Unmount SD card. Actually I didn't use the command I used the Disk Utility app and unmounted the entire disk.

    ```
sudo diskutil unmountDisk disk2
    ```
    
 * result:
 
    ```
Unmount of all volumes on disk2 was successful
    ```

4. Remove SD card from Mac laptop
5. Put recently formatted SD card in Raspberry Pi
6. Connect keyboard and mouse
7. Turn on power to RaspberryPi



## Resources:

* How to install Kali on Raspberry Pi: [Install Kali Linux on Raspberry Pi using a Mac 2017/18
](https://www.youtube.com/watch?v=um6vNLiqZhM)
* Additional information on moving image to SD card: [Writing .img file to SD Card from a Mac](https://raspberrypi.stackexchange.com/questions/4144/writing-img-file-to-sd-card-from-a-mac).
* Kali Documentation: [Kali Linux – Raspberry Pi](https://docs.kali.org/kali-on-arm/install-kali-linux-arm-raspberry-pi)
