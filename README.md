# raspberry_pi_zero_bug
A simple program to exhibit a bug in the USB stack of the Raspberry Pi Zero


### Steps to reproduce the issue


git clone this repository on your Raspberry PI Zero
```
git clone https://github.com/yoctopuce-examples/raspberry_pi_zero_bug.git
```

Install libUSB 1.0 headers

```
sudo apt-get install libusb-1.0-0-dev
```

Compile the file bug_pizero.c:

```
gcc bug_pizero.c -o bug_pizero -lusb-1.0
```

Run the example as root 
```
sudo ./bug_pizero
```
