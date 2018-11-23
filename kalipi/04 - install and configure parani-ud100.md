# Installing Parani-UD100 - 20181121

Installing and configuring the "Parani-UD100 Bluetooth 4.0 Class1 USB Adapter, 300m Working Distance, Exchangeable Antenna, BlueSoleil Driver" [Item Name/Code: UD100-G03].

Had some issues getting the KaliPi to recognize the Parani-UD100 Bluetooth 4.0 Class1 USB Adapter.

Use steps as needed and as appropriate; some steps below may *NOT* be needed.

Poor documentation below. Not sure what exactly got it working. Maybe something will be useful in future installs... _Steps 23-25 are optional_.

## hardware

* KaliPi: Raspberry Pi 3 Model B+ running Kali linux
* Parani-UD100 Bluetooth 4.0 Class1 USB Adapter [Item Name/Code: UD100-G03]

## Adding the Parani-UD100

1. Connect device into the Pi.
1. Locate connected USB device. Execute:

    ```
	lsusb
    ```

  * result should contain with DVB-T, meaning the OS has recognized the device and loaded the _correct_ drivers:

    ```
	Bus 001 Device 004: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
    ```

3. Check loaded DVB devices. Execute:

    ```
	lsmod  | grep dvb
    ```

 * result

    ```
	dvb_usb_rtl28xxu       28672  0
	dvb_usb_v2             24576  1 dvb_usb_rtl28xxu
	dvb_core              110592  2 dvb_usb_v2,rtl2832
    ```

_... where things get fuzzy..._ combination of troubleshooting, listing configs, installing, configuring, unconfiguring, reconfiguring, and then realizing something worked (or maybe it worked all along and your an idiot).
 
4. Install random stuff... Execute:

    ```
	apt-get install bluetooth bluez-cups bluez-obexd bluez-hcidump bluez-tools
    ```

1. Restart bluetooth. Execute:

    ```
	service bluetooth restart
    ```

1. Display local bluetooth devices. Execute:    

    ```
	hcitool dev
    ```

  * result:

    ```
	Devices:
		hci1	B8:27:EB:9B:9E:12
		hci0	00:01:95:48:90:91
    ```

7. List usb devices. Execute:

    ```
	lsusb -v | grep -A25 0001
    ```

  * result:
    
    ```
	Bus 001 Device 004: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
	Device Descriptor:
	  bLength                18
	  bDescriptorType         1
	  bcdUSB               2.00
	  bDeviceClass          224 Wireless
	  bDeviceSubClass         1 Radio Frequency
	  bDeviceProtocol         1 Bluetooth
	  bMaxPacketSize0        64
	  idVendor           0x0a12 Cambridge Silicon Radio, Ltd
	  idProduct          0x0001 Bluetooth Dongle (HCI mode)
	  bcdDevice           82.41
	  iManufacturer           0
	  iProduct                0
	  iSerial                 0
	  bNumConfigurations      1
	  Configuration Descriptor:
	    bLength                 9
	    bDescriptorType         2
	    wTotalLength          177
	    bNumInterfaces          2
	    bConfigurationValue     1
	    iConfiguration          0
	    bmAttributes         0xc0
	      Self Powered
	    MaxPower                0mA
	    Interface Descriptor:
	      bLength                 9
	      bDescriptorType         4
	      bInterfaceNumber        0
	      bAlternateSetting       0
	      bNumEndpoints           3
	      bInterfaceClass       224 Wireless
	      bInterfaceSubClass      1 Radio Frequency
	      bInterfaceProtocol      1 Bluetooth
	      iInterface              0
    ```

8. List installed packages. Execute:

    ```
	dpkg -l | egrep "bluez"
    ```

  * result:

    ```
	ii  bluez                                5.50-1                       armhf        Bluetooth tools and daemons
	ii  bluez-cups                           5.50-1                       armhf        Bluetooth printer driver for CUPS
	ii  bluez-firmware                       1.2-4                        all          Firmware for Bluetooth devices
	ii  bluez-hcidump                        5.50-1                       armhf        Analyses Bluetooth HCI packets
	ii  bluez-obexd                          5.50-1                       armhf        bluez obex daemon
	ii  bluez-test-scripts                   5.50-1                       all          test scripts of bluez
	ii  bluez-tools                          2.0~20170911.0.7cb788c-2     armhf        Set of tools to manage Bluetooth devices for linux
	ii  python-bluez                         0.22+really0.22-1            armhf        Python 2 wrappers around BlueZ for rapid bluetooth development
    ```

9. List bluetooth. Execute:

    ```
	/usr/share/doc/bluez-test-scripts/examples/test-adapter list
    ```
    
  * result:

    ```
	[ /org/bluez/hci0 ]
	    AddressType = public
	    Name = kali #1
	    Powered = 1
	    Modalias = usb:v1D6Bp0246d0532
	    DiscoverableTimeout = 180
	    Alias = kali #1
	    PairableTimeout = 0
	    Discoverable = 0
	    Address = 00:01:95:48:90:91
	    Discovering = 0
	    Pairable = 1
	    Class = 0x000000
	    UUIDs = dbus.Array([dbus.String(u'00001801-0000-1000-8000-00805f9b34fb'), dbus.String(u'0000110e-0000-1000-8000-00805f9b34fb'), dbus.String(u'00001200-0000-1000-8000-00805f9b34fb'), dbus.String(u'0000110c-0000-1000-8000-00805f9b34fb'), dbus.String(u'00001800-0000-1000-8000-00805f9b34fb')], signature=dbus.Signature('s'), variant_level=1)

	 [ /org/bluez/hci1 ]
	    AddressType = public
	    Name = kali
	    Powered = 1
	    Modalias = usb:v1D6Bp0246d0532
	    DiscoverableTimeout = 180
	    Alias = kali
	    PairableTimeout = 0
	    Discoverable = 0
	    Address = B8:27:EB:9B:9E:12
	    Discovering = 0
	    Pairable = 1
	    Class = 0x000000
	    UUIDs = dbus.Array([dbus.String(u'00001801-0000-1000-8000-00805f9b34fb'), dbus.String(u'0000110e-0000-1000-8000-00805f9b34fb'), dbus.String(u'00001200-0000-1000-8000-00805f9b34fb'), dbus.String(u'0000110c-0000-1000-8000-00805f9b34fb'), dbus.String(u'00001800-0000-1000-8000-00805f9b34fb')], signature=dbus.Signature('s'), variant_level=1)
    ```    
_random actions..._ that probably were pointless

10. Execute:

    ```
	hciconfig -a
    ```

  * result:

    ```
	hci1:	Type: Primary  Bus: UART
		BD Address: B8:27:EB:9B:9E:12  ACL MTU: 1021:8  SCO MTU: 64:1
		DOWN
		RX bytes:668 acl:0 sco:0 events:34 errors:0
		TX bytes:423 acl:0 sco:0 commands:34 errors:0
		Features: 0xbf 0xfe 0xcf 0xfe 0xdb 0xff 0x7b 0x87
		Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
		Link policy: RSWITCH SNIFF
		Link mode: SLAVE ACCEPT

	hci0:	Type: Primary  Bus: USB
		BD Address: 00:01:95:48:90:91  ACL MTU: 310:10  SCO MTU: 64:8
		DOWN
		RX bytes:580 acl:0 sco:0 events:31 errors:0
		TX bytes:368 acl:0 sco:0 commands:30 errors:0
		Features: 0xff 0xff 0x8f 0xfe 0xdb 0xff 0x5b 0x87
		Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
		Link policy: RSWITCH HOLD SNIFF PARK
		Link mode: SLAVE ACCEPT
    ```
    
11. Execute:

    ```
	lsusb
    ```

  * result:

    ```
	Bus 001 Device 004: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
	Bus 001 Device 006: ID 0424:7800 Standard Microsystems Corp.
	Bus 001 Device 003: ID 0424:2514 Standard Microsystems Corp. USB 2.0 Hub
	Bus 001 Device 002: ID 0424:2514 Standard Microsystems Corp. USB 2.0 Hub
	Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    ```

12. Execute:

    ```
	lsusb -v
    ```

  * result:

    ```
	Bus 001 Device 004: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
	Device Descriptor:
	  bLength                18
	  bDescriptorType         1
	  bcdUSB               2.00
	  bDeviceClass          224 Wireless
	  bDeviceSubClass         1 Radio Frequency
	  bDeviceProtocol         1 Bluetooth
	  bMaxPacketSize0        64
	  idVendor           0x0a12 Cambridge Silicon Radio, Ltd
	  idProduct          0x0001 Bluetooth Dongle (HCI mode)
	  bcdDevice           82.41
	  iManufacturer           0
	  iProduct                0
	  iSerial                 0
	  bNumConfigurations      1
	  Configuration Descriptor:
	    bLength                 9
	    bDescriptorType         2
	    wTotalLength          177
	    bNumInterfaces          2
	    bConfigurationValue     1
	    iConfiguration          0
	    bmAttributes         0xc0
	      Self Powered
	    MaxPower                0mA
	    Interface Descriptor:
	      bLength                 9
	      bDescriptorType         4
	      bInterfaceNumber        0
	      bAlternateSetting       0
	      bNumEndpoints           3
	      bInterfaceClass       224 Wireless
	      bInterfaceSubClass      1 Radio Frequency
	      bInterfaceProtocol      1 Bluetooth
	      iInterface              0
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x81  EP 1 IN
		bmAttributes            3
		  Transfer Type            Interrupt
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0010  1x 16 bytes
		bInterval               1
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x02  EP 2 OUT
		bmAttributes            2
		  Transfer Type            Bulk
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0040  1x 64 bytes
		bInterval               1
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x82  EP 2 IN
		bmAttributes            2
		  Transfer Type            Bulk
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0040  1x 64 bytes
		bInterval               1
	    Interface Descriptor:
	      bLength                 9
	      bDescriptorType         4
	      bInterfaceNumber        1
	      bAlternateSetting       0
	      bNumEndpoints           2
	      bInterfaceClass       224 Wireless
	      bInterfaceSubClass      1 Radio Frequency
	      bInterfaceProtocol      1 Bluetooth
	      iInterface              0
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x03  EP 3 OUT
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0000  1x 0 bytes
		bInterval               1
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x83  EP 3 IN
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0000  1x 0 bytes
		bInterval               1
	    Interface Descriptor:
	      bLength                 9
	      bDescriptorType         4
	      bInterfaceNumber        1
	      bAlternateSetting       1
	      bNumEndpoints           2
	      bInterfaceClass       224 Wireless
	      bInterfaceSubClass      1 Radio Frequency
	      bInterfaceProtocol      1 Bluetooth
	      iInterface              0
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x03  EP 3 OUT
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0009  1x 9 bytes
		bInterval               1
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x83  EP 3 IN
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0009  1x 9 bytes
		bInterval               1
	    Interface Descriptor:
	      bLength                 9
	      bDescriptorType         4
	      bInterfaceNumber        1
	      bAlternateSetting       2
	      bNumEndpoints           2
	      bInterfaceClass       224 Wireless
	      bInterfaceSubClass      1 Radio Frequency
	      bInterfaceProtocol      1 Bluetooth
	      iInterface              0
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x03  EP 3 OUT
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0011  1x 17 bytes
		bInterval               1
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x83  EP 3 IN
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0011  1x 17 bytes
		bInterval               1
	    Interface Descriptor:
	      bLength                 9
	      bDescriptorType         4
	      bInterfaceNumber        1
	      bAlternateSetting       3
	      bNumEndpoints           2
	      bInterfaceClass       224 Wireless
	      bInterfaceSubClass      1 Radio Frequency
	      bInterfaceProtocol      1 Bluetooth
	      iInterface              0
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x03  EP 3 OUT
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0019  1x 25 bytes
		bInterval               1
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x83  EP 3 IN
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0019  1x 25 bytes
		bInterval               1
	    Interface Descriptor:
	      bLength                 9
	      bDescriptorType         4
	      bInterfaceNumber        1
	      bAlternateSetting       4
	      bNumEndpoints           2
	      bInterfaceClass       224 Wireless
	      bInterfaceSubClass      1 Radio Frequency
	      bInterfaceProtocol      1 Bluetooth
	      iInterface              0
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x03  EP 3 OUT
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0021  1x 33 bytes
		bInterval               1
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x83  EP 3 IN
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0021  1x 33 bytes
		bInterval               1
	    Interface Descriptor:
	      bLength                 9
	      bDescriptorType         4
	      bInterfaceNumber        1
	      bAlternateSetting       5
	      bNumEndpoints           2
	      bInterfaceClass       224 Wireless
	      bInterfaceSubClass      1 Radio Frequency
	      bInterfaceProtocol      1 Bluetooth
	      iInterface              0
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x03  EP 3 OUT
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0031  1x 49 bytes
		bInterval               1
	      Endpoint Descriptor:
		bLength                 7
		bDescriptorType         5
		bEndpointAddress     0x83  EP 3 IN
		bmAttributes            1
		  Transfer Type            Isochronous
		  Synch Type               None
		  Usage Type               Data
		wMaxPacketSize     0x0031  1x 49 bytes
		bInterval               1
	Device Status:     0x0001
	  Self Powered
    ```

13. Execute:

    ```
	rfkill list
    ```

  * result:

    ```
	0: hci0: Bluetooth
		Soft blocked: no
		Hard blocked: no
	1: phy0: Wireless LAN
		Soft blocked: no
		Hard blocked: no
	2: hci1: Bluetooth
		Soft blocked: no
		Hard blocked: no
    ```

14. Execute:

    ```
	service bluetooth restart
    ```

  * result:

    ```
	[nothing]
    ```

15. Execute:

    ```
	service bluetooth status
    ```

  * result:

    ```
	● bluetooth.service - Bluetooth service
	   Loaded: loaded (/lib/systemd/system/bluetooth.service; disabled; vendor preset: disabled)
	   Active: active (running) since Tue 2018-11-13 23:40:42 UTC; 1min 38s ago
	     Docs: man:bluetoothd(8)
	 Main PID: 1285 (bluetoothd)
	   Status: "Running"
	    Tasks: 1 (limit: 2203)
	   CGroup: /system.slice/bluetooth.service
		   └─1285 /usr/lib/bluetooth/bluetoothd
	Nov 13 23:40:42 kali systemd[1]: Starting Bluetooth service...
	Nov 13 23:40:42 kali bluetoothd[1285]: Bluetooth daemon 5.50
	Nov 13 23:40:42 kali systemd[1]: Started Bluetooth service.
	Nov 13 23:40:42 kali bluetoothd[1285]: Starting SDP server
	Nov 13 23:40:42 kali bluetoothd[1285]: Bluetooth management interface 1.14 initialized
	Nov 13 23:40:42 kali bluetoothd[1285]: Sap driver initialization failed.
	Nov 13 23:40:42 kali bluetoothd[1285]: sap-server: Operation not permitted (1)
	Nov 13 23:40:42 kali bluetoothd[1285]: Sap driver initialization failed.
	Nov 13 23:40:42 kali bluetoothd[1285]: sap-server: Operation not permitted (1)
    ```

16. Execute:

    ```
	echo "blacklist hci_usb" >> /etc/modprobe.d/blacklist
	echo "hci_usb reset=1" >> /etc/modules
    ```

17. Execute:

    ```
	service bluetooth status
    ```

  * result:

    ```
	[nothing...]	
    ```

18. Execute:

    ```
	service bluetooth status
    ```

  * result:

    ```
	[same as last time]
    ```

19. Undo the following:

    ```
	echo "blacklist hci_usb" >> /etc/modprobe.d/blacklist
	echo "hci_usb reset=1" >> /etc/modules
    ```

20. Execute:

    ```
	service bluetooth status
    ```

  * result:

    ```
	[nothing...]	
    ```

21. Execute:

    ```
	hciconfig dev
    ```

  * result:

    ```
	hci0:	Type: Primary  Bus: USB
		BD Address: 00:01:95:48:90:91  ACL MTU: 310:10  SCO MTU: 64:8
		UP RUNNING
		RX bytes:6147 acl:0 sco:0 events:355 errors:0
		TX bytes:10108 acl:0 sco:0 commands:314 errors:0
    ```

22. Execute:

    ```
	hcitool scan
    ```

  * result:

    ```
	Scanning ...

    ```

23. Execute:

    ```
	hciconfig hci0 piscan
    ```

  * result:

    ```
	[nothing...]	
    ```

24. Execute:

    ```
	hciconfig -a
    ```

  * result:

    ```
	hci1:	Type: Primary  Bus: UART
		BD Address: B8:27:EB:9B:9E:12  ACL MTU: 1021:8  SCO MTU: 64:1
		UP RUNNING
		RX bytes:3236 acl:0 sco:0 events:180 errors:0
		TX bytes:7438 acl:0 sco:0 commands:179 errors:0
		Features: 0xbf 0xfe 0xcf 0xfe 0xdb 0xff 0x7b 0x87
		Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
		Link policy: RSWITCH SNIFF
		Link mode: SLAVE ACCEPT
		Name: 'kali'
		Class: 0x000000
		Service Classes: Unspecified
		Device Class: Miscellaneous,
		HCI Version: 4.1 (0x7)  Revision: 0x0
		LMP Version: 4.1 (0x7)  Subversion: 0x6119
		Manufacturer: Broadcom Corporation (15)
	hci0:	Type: Primary  Bus: USB
		BD Address: 00:01:95:48:90:91  ACL MTU: 310:10  SCO MTU: 64:8
		UP RUNNING PSCAN ISCAN
		RX bytes:6159 acl:0 sco:0 events:357 errors:0
		TX bytes:10147 acl:0 sco:0 commands:316 errors:0
		Features: 0xff 0xff 0x8f 0xfe 0xdb 0xff 0x5b 0x87
		Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
		Link policy: RSWITCH HOLD SNIFF PARK
		Link mode: SLAVE ACCEPT
		Name: 'kali #1'
		Class: 0x000000
		Service Classes: Unspecified
		Device Class: Miscellaneous,
		HCI Version: 4.0 (0x6)  Revision: 0x2031
		LMP Version: 4.0 (0x6)  Subversion: 0x2031
		Manufacturer: Cambridge Silicon Radio (10)	
    ```

25. Tried running blue_hydra again because it wasn't working with the adapter in it. Execute:

    ```
	bundle exec ./bin/blue_hydra -d
    ```

  * result:

    ```
	worked as expeced
    ```

_hmmmm...._

26. Execute:

    ```
	hciconfig hci1 down
    ```
    
27. Execute:

    ```
	hciconfig -a
    ```

  * result:

    ```
	hci1:	Type: Primary  Bus: UART
		BD Address: B8:27:EB:9B:9E:12  ACL MTU: 1021:8  SCO MTU: 64:1
		DOWN
		RX bytes:3523 acl:0 sco:0 events:184 errors:0
		TX bytes:7454 acl:0 sco:0 commands:183 errors:0
		Features: 0xbf 0xfe 0xcf 0xfe 0xdb 0xff 0x7b 0x87
		Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
		Link policy: RSWITCH SNIFF
		Link mode: SLAVE ACCEPT
	hci0:	Type: Primary  Bus: USB
		BD Address: 00:01:95:48:90:91  ACL MTU: 310:10  SCO MTU: 64:8
		UP RUNNING PSCAN ISCAN
		RX bytes:6436 acl:0 sco:0 events:360 errors:0
		TX bytes:10156 acl:0 sco:0 commands:319 errors:0
		Features: 0xff 0xff 0x8f 0xfe 0xdb 0xff 0x5b 0x87
		Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
		Link policy: RSWITCH HOLD SNIFF PARK
		Link mode: SLAVE ACCEPT
		Name: 'kali #1'
		Class: 0x000000
		Service Classes: Unspecified
		Device Class: Miscellaneous,
		HCI Version: 4.0 (0x6)  Revision: 0x2031
		LMP Version: 4.0 (0x6)  Subversion: 0x2031
		Manufacturer: Cambridge Silicon Radio (10)
    ```

28. Unplug the adapter:
29. Execute:

    ```
	hciconfig -a
    ```

  * result:

    ```
	hci1:	Type: Primary  Bus: UART
		BD Address: B8:27:EB:9B:9E:12  ACL MTU: 1021:8  SCO MTU: 64:1
		DOWN
		RX bytes:3523 acl:0 sco:0 events:184 errors:0
		TX bytes:7454 acl:0 sco:0 commands:183 errors:0
		Features: 0xbf 0xfe 0xcf 0xfe 0xdb 0xff 0x7b 0x87
		Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
		Link policy: RSWITCH SNIFF
		Link mode: SLAVE ACCEPT
    ```

30. Execute:

    ```
	lsusb
    ```

  * result:

    ```
	Bus 001 Device 006: ID 0424:7800 Standard Microsystems Corp.
	Bus 001 Device 003: ID 0424:2514 Standard Microsystems Corp. USB 2.0 Hub
	Bus 001 Device 002: ID 0424:2514 Standard Microsystems Corp. USB 2.0 Hub
	Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub		
    ```

31. Realize the Parani-UD100 was working.
32. Look at palm of hand
33. Close eyes.
34. Gently lift arm and slap open palm to forehead.
 * result:
 
    ```
	[soft snapping sound]
    ```
    
35. Run blue_hydra as before...

## resources

* Troubleshooting information: [[bluetooth] hcitool Can't init device hci0: Device or resource busy](https://bbs.archlinux.org/viewtopic.php?id=126603)
* Additional troubleshooting information: [HardwareSupportComponentsBluetoothUsbAdapters](https://wiki.ubuntu.com/HardwareSupportComponentsBluetoothUsbAdapters)
