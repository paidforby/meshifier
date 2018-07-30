
# people's open meshifier

WORK IN PROGRESS. 

This is a script to automatically get your system on the People's Open Network mesh. Not just as a client but as an actual mesh node.

It will work on any Debian-based GNU/Linux distro or OpenWRT/LEDE based distro.

Currently, it has been tested and works well with a TP-LINK TL-WN722Nv2 and Debian 9. It is NOT RECOMMENDED to try using this with your built-in wireless card, as the script makes changes to your Network Manager configuration that may be difficult to undo if you don't know where to look (hint: /etc/NetworkManager/NetworkManager.conf).   

It now has the ability to create virtual interfaces on a physical adpater of your choosing (WARNING: still very untested). And lots of options,
```
./meshify
-a (automatically retrieve an IP address from meshnode database, recommended for production usage)
-m <mesh_ip_address> (set your ip address manually, recommended for testing purposes)
-b (start babel after meshifying an interface)
-f (force the usage of a non-virtual interface, i.e. probably the one you use for the internets)
-p <physical_device> (on which you'd like to create a virtual interface)
-i <interface_name> (interface you'd like to meshify)
-u <interface_name> (unmeshify a previously meshified interface)
-x <interface_name> (delete virtual interface, use with care!)
```

To use, install dependencies manually,
```
sudo apt install babeld resolvconf jq
sudo cp -r rtlwifi /lib/firmware/. # firmware for TL-WN722Nv2, if your's already works you may not need to do this
```
Next, meshify your interface and start babeld with,
```
sudo ./meshify -m <ip_address> -p [physical_apadter] -i <virtual_interface> 
babeld <virtual_interface> 
```
For example, it may look something like,
```
sudo ./meshify -m 100.65.96.129 -p phy0 -i mesh0 
babeld mesh0
```
To test, find a place with a active People's Open Node (or a friend with another meshified interface) and try pinging the other node's IP address, it's also good to specify the interface like so,
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

Some wireless drivers/adapters do not support mixed modes of operation, to see what modes your adapter supports check,
```
iw list
```
You should see an output like this,
```
[...]
Supported interface modes:
     * IBSS
     * managed
     * AP
     * AP/VLAN
     * WDS
     * monitor
     * mesh point
[...]
software interface modes (can always be added):
     * AP/VLAN
     * monitor
valid interface combinations:
     * #{ managed } <= 1, #{ AP, P2P-client, P2P-GO } <= 1, #{ P2P-device } <= 1,
       total <= 3, #channels <= 2
```

# Future plans

* Make an option to also configure tunneldigger
* Make an option to also configure an open AP with DHCP server
