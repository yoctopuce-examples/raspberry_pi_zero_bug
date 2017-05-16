# Bug in the Raspberry Pi Zero stack
A simple program to exhibit a bug in the USB stack of the Raspberry Pi Zero.

### Sort descriptions of the bug

Randomly the Raspberry Pi Zero send corrupted CRC on ``IN`` request on interrupt endpoint.

![Screenshot of the USB trace](screen_capture.png)


### Context

We produce USB 2.0 device that work at Full Speed (not High Speed) and use two interrupt endpoints. See [http://www.yoctopuce.com]. Our devices are declared as HID device that use a Vendor specific protocol. Our open source library use the libUSB 1.0 to communicate with our devices.

We have tested ans used all Raspberry Pi device since the beginning. But, we discover that official Raspbian Image after march 2015 installed on a Rasbperry Pi Zero will no more work with our devices.

After some tests we found that USB packet sent by the Raspberry Pi Zero have sometime Invalid CRC. According to the USB specification (section: 8.7.1) when a device receive an invalid packet, it should ignore it and the USB host (the Raspberry Pi Zero) should resend it later.

But, on instead the libUSB return an error and is no more able to use the devices. Sometime it event completely free the Raspberry Pi Zero.

We have not been able to find if the issue is in the libUSB or the Linux kernel. But have done lots of regression testing and here is the results:

* Official Raspbian image until March 2015 does not have this issue.
* Only the Raspberry Pi Zero and Pi Zero W are affected.

We have


### Steps to compile this program

First clone this repository on your Raspberry PI Zero.
```
git clone https://github.com/yoctopuce-examples/raspberry_pi_zero_bug.git
```

Install libUSB 1.0 headers.

```
sudo apt-get install libusb-1.0-0-dev
```

Compile the file program with the following command

```
gcc bug_pizero.c -o bug_pizero -lusb-1.0
```


### Run the test



Run the example as root
```
sudo ./bug_pizero
```



pi@raspberrypi:~/raspberry_pi_zero_bug $ sudo ./bug_pizero
this is a simple program that exhibit a bug in the USB stack
of the Raspberry PI Zero and any Yoctopuce device
Use libUSB v1.0.19.10903
List all Yoctopuce devices present on this host:
 - USB dev: RELAYHI1-85D28 (24E0:D:0)
1 Yoctopuce devices are present
\ Test device RELAYHI1-85D28 (24E0:D)
+ Send USB pkt no 0
- rd_callback for RELAYHI1-85D28 : response 0 is valid (len=64)
+ Send USB pkt no 1
- rd_callback for RELAYHI1-85D28 : response 1 is valid (len=64)
+ Send USB pkt no 2
- rd_callback for RELAYHI1-85D28 : response 2 is valid (len=64)
+ Send USB pkt no 3
- rd_callback for RELAYHI1-85D28 : response 3 is valid (len=64)
+ Send USB pkt no 4
- rd_callback for RELAYHI1-85D28 : response 4 is valid (len=64)
+ Send USB pkt no 5
- rd_callback for RELAYHI1-85D28 : response 5 is valid (len=64)
+ Send USB pkt no 6
- rd_callback for RELAYHI1-85D28 : response 6 is valid (len=64)
+ Send USB pkt no 7
- rd_callback for RELAYHI1-85D28 : response 7 is valid (len=64)
+ Send USB pkt no 8
- rd_callback for RELAYHI1-85D28 : response 8 is valid (len=64)
+ Send USB pkt no 9
- rd_callback for RELAYHI1-85D28 : response 9 is valid (len=64)
+ Send USB pkt no 10
- rd_callback for RELAYHI1-85D28 : response 10 is valid (len=64)
+ Send USB pkt no 11
- rd_callback for RELAYHI1-85D28 : response 11 is valid (len=64)
+ Send USB pkt no 12
- rd_callback for RELAYHI1-85D28 : response 12 is valid (len=64)
+ Send USB pkt no 13
- rd_callback for RELAYHI1-85D28 : response 13 is valid (len=64)
+ Send USB pkt no 14
- rd_callback for RELAYHI1-85D28 : response 14 is valid (len=64)
= result: 15 pkt sent vs 15 pkt received
Cancel and free all pending transactions
pi@raspberrypi:~/raspberry_pi_zero_bug $




### Misc links:


USB specifications: http://www.usb.org/developers/docs/usb20_docs/
USB analyser used : http://www.ellisys.com/products/usbex200/index.php
Official Raspbian images: https://downloads.raspberrypi.org/raspbian/images/
Original project that was working with old Raspiban images:http://www.yoctopuce.com/EN/article/creating-an-multimeter-with-a-raspberry-pi-zero