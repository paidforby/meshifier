
WORK IN PROGRESS. DOES NOT YET DO ANYTHING USEFUL. THE FOLLOWING DOCUMENTATION IS ASPIRATIONAl.

This is a script to automatically get your system on the People's Open Network mesh. Not just as a client but as an actual mesh node.

It will work on any Debian-based GNU/Linux distro or OpenWRT/LEDE based distro.

It does the following:

* Configures your wireless adapter to work in adhoc/IBSS mode
* Requests a mesh-unique IPv4 subnet from an IP assignment server
* Installs and configures the Babel mesh routing protocol daemon `babeld`

It tries to do all of this without affecting your existing network configuration, but sometimes it is not possible, e.g. if your wifi driver/adapter does not support multiple virtual interfaces on a single radio (multivif). If this is the case then the script will ask you before modifying your configuration.

# Future plans

* Make an option to also configure tunneldigger
* Make an option to also configure an open AP with DHCP server
