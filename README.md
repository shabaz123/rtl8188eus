# rtl8188eus
This repo was forked from Munazirul/rtl8188eus and modified so it would build on Rockchip RK3588 running Ubuntu, specifically the **Axelera Metis M.2 Eval System**. 

This repo contains a driver for RTL8188EUS which is used with the 2.4 GHz TP-Link TL-WN725N USB Wireless Adapter. This fork has not been tested on any other hardware other than that mentioned.

# Steps to install this driver on your linux machine

Plug in the adapter first, and ensure you have an Ethernet cable connected between compute board and your home switch/router, because steps 1-3 require Internet access.

First off, type **ping www.bbc.co.uk** and see that you get successful ping responses. If you don't get that, then there's an issue with the wired Ethernet connection.

If you're successful, then press **Ctrl-C** to stop the pinging, and then continue with these steps:

1. `sudo apt update`
2. `sudo apt install dkms bc`
3. `git clone https://github.com/shabaz123/rtl8188eus`
4. `cd rtl8188eus`
5. `sudo sh -c 'echo "blacklist r8188eu.ko" >> /etc/modprobe.d/realtek.conf'`
6. `sudo sh -c 'echo "blacklist 8188eu.ko" >> /etc/modprobe.d/realtek.conf'`
7. `sudo make`
8. `sudo make install`
9. `sudo reboot`
10. `sudo modprobe 8188eu` (Only if the wifi networks are not shown on your machine)

# Hardware Check

With the TL-WN725N USB adapter inserted, power up your machine if you have not already done so. Type **lsusb** and you should see a line indicating that a USB device containing RTL8188EUS is inserted. If you don't see that, then either there is an issue with the USB adapter, or you've got a different USB adapter than expected.

# Driver Check

Type **ifconfig -a** and you should see the interfaces listed, and one will be called something like wlxxxxxxxxxxxxx. If you don't see that, then the driver installation was not successful for some reason.

# Using the Driver

Type **nmcli radio wifi** and the output should indicate 'enabled'. Next, type **nmcli radio wifi on** and then after a few seconds, type **nmcli dev wifi list** to see a list of SSIDs. 

To connect to an SSID, type the following:

`sudo nmcli dev wifi connect MY_SSID password "MY_PASSWORD"`

where MY_SSID is the SSID name, and MY_PASSWORD is the password (enclose it in speech-marks) as shown above.

# Check you are connected to a Wireless Network

Type **ifconfig -a** and at the wlxxxxxxxxxxxxx entry, in the second line, you should see an IP address, such as:

`inet 192.168.1.85 netmask 255.255.255.0 broadcast 192.168.1.255`

Alternatively, you may see a line such as:

`inet6 fe80::xxxx:xxxx:xxxx:xxxx prefixlen 65 scopeid 0x20<link>`

If you don't see either of those lines, then there's either an issue with the wireless connection, or the router has not assigned an IP address to your network interface for some reason.

If the above was successful, then you can disconnect your Ethernet connection, and then try **ping www.bbc.co.uk** and see if you get ping successes!







