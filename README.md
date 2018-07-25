
# people's open meshifier

WORK IN PROGRESS. 

This is a script to automatically get your system on the People's Open Network mesh. Not just as a client but as an actual mesh node.

It will work on any Debian-based GNU/Linux distro or OpenWRT/LEDE based distro.

Currently, it has been tested and works well with a TP-LINK TL-WN722Nv2 and Debian 9. It is NOT RECOMMENDED to try using this with your built-in wireless card, as the script makes changes to your Network Manager configuration that may be difficult to undo if you don't know where to look (hint: /etc/NetworkManager/NetworkManager.conf).   

First, install dependencies manually,
```
sudo apt install babeld resolvconf jq
sudo cp -r rtlwifi /lib/firmware/. # firmware for TL-WN722Nv2, if your's already works you may not need to do this
```
Next, meshify your interface and start babeld with,
```
sudo ./meshify <interface> <ip_address>
babeld <interface> 
```
For example, it may look something like,
```
sudo ./meshify wlx563eae5f6254 100.65.96.129
babeld wlx563eae5f6254
```
These TP-LINK USB WiFi cards typically have their MAC address in their interface name, which is cool.

If you do not specify an IP address, the script will retrieve one from a meshnode database for you (still experimental, leave `debug=true` in curl command, so you don't expend all mesh IP addresses). 

To test, find a place with a active People's Open Node (or a friend with another TL-WN722) and try pinging the other node's IP address, it's also good to specify the interface like so,
```
ping <neighbour_node_IP> -I <interface> 
```
If one of the nodes you are meshing with has an internet connection, you may even be able to get to the outside world, try stuff like,
```
ping 100.64.0.43 # sudomesh exit node
ping 8.8.8.8
ping archlinux.org
```
If dns was configured correctly, these should work over the meshed interface!

Eventually, this script will try to set up everything without affecting your existing network configuration, but sometimes it is not possible, e.g. if your wifi driver/adapter does not support multiple virtual interfaces on a single radio (multivif). If this is the case then the script will ask you before modifying your configuration.

# Future plans

* Make an option to also configure tunneldigger
* Make an option to also configure an open AP with DHCP server
