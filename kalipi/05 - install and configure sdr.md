# KaliPi and R820T SDR - 20181121

Basic install steps. Have not used the SDR much so the below steps may be incomplete or inaccurate.

## Hardare

* KaliPi: Raspberry Pi 3 Model B+ running Kali linux
* Parani-UD100 Bluetooth 4.0 Class1 USB Adapter [Item Name/Code: UD100-G03]
* NooElec R820T SDR & DVB-T NESDR Mini


## Install steps

1. Install packages. Execute:

    ```
    apt-get -fy install rtl-sdr
    ```
    
2. Connect antenna and plug device into the Pi.
3. Locate connected USB device. Execute

    ```
    lsusb
    ```
    
  * result should contain with DVB-T, meaning the OS has recognized the device and loaded the _correct_ drivers:

    ```
Bus 001 Device 009: ID 0bda:2832 Realtek Semiconductor Corp. RTL2832U DVB-T
    ```
 
1. Check loaded DVB devices. Execute:

    ```
    lsmod  | grep dvb
    ```
    
 * result

    ```
    dvb_usb_rtl28xxu       28672  0
    dvb_usb_v2             24576  1 dvb_usb_rtl28xxu
    dvb_core              110592  2 dvb_usb_v2,rtl2832
    ```
 
 1. Black default drivers. Execute:

    ```
    echo "blacklist dvb_usb_rtl28xxu" >> /etc/modprobe.d/blacklist-dvb.conf
    ```
 
 1. Unplug the usb device.
 2. Plug back in
 3. Run a test. Execute:


    ```
    rtl_test
    ```
    
 * result

    ```
    Found 1 device(s):
      0:  Generic, RTL2832U, SN: 77771111153705700
    Using device 0: Generic RTL2832U
    Detached kernel driver
    Found Rafael Micro R820T tuner
    Supported gain values (29): 0.0 0.9 1.4 2.7 3.7 7.7 8.7 12.5 14.4 15.7 16.6 19.7 20.7 22.9 25.4 28.0 29.7 32.8 33.8 36.4 37.2 38.6 40.2 42.1 43.4 43.9 44.5 48.0 49.6
    [R82XX] PLL not locked!
    Sampling at 2048000 S/s.
    Info: This tool will continuously read from the device, and report if
    samples get lost. If you observe no further output, everything is fine.
    Reading samples in async mode...
    Allocating 15 zero-copy buffers
    lost at least 144 bytes
    ```

## scan range

* Use ```rtl_power``` to scan	

 
## resources

* Manual: [NESDR Installation Manual for Ubuntu - Nooelec](https://www.nooelec.com/store/downloads/dl/file/id/72/product/0/nesdr_installation_manual_for_ubuntu.pdf)
* Useful information: [RX_TOOLS: RTL-SDR COMMAND LINE TOOLS (RTL_POWER, RTL_FM, RTL_SDR) NOW COMPATIBLE WITH ALMOST ANY SDR](https://www.rtl-sdr.com/rx_tools-rtl-sdr-command-line-tools-rtl_power-rtl_fm-rtl_sdr-now-compatible-with-almost-any-sdr/)
