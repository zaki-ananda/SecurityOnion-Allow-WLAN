# Security Onion: How to Allow WLAN/Wireless Interface for Management

By default, Security Onion does not allow WLAN interface for management purpose. The installer screen would block the installation if the machine has less than the required NICs, and would only list wired interfaces without any option to select WLAN interface. This can be extremely limiting for standalone deployment on Laptop, Mini-PC, and other single NIC machines. 

Fortunately, there are various hacks to overcome this problen. One of them is through [dividing the wired interface into multiple VLAN subinterfaces](https://github.com/zaki-ananda/SecurityOnion-LibvirtSetup). The other, described below, involves editing the Security Onion installation script to allow selecting a WLAN interface. The difference between the two is that this one will allow you to have a baremetal installation of Security Onion, granting more resources for the instance (as opposed to having to split the resource between host machine and VM instance).

## Steps:
This guide assumes that you have finished the first installation of Security Onion (ie: configured the username & password) and currently on the blue installation screen (aka TUI or Terminal User Interface)

1. Hit 'Cancel' on the TUI
2. Run 'nmtui' and connect to your desired AP
3. Run 'ip addr' and write down or memorize the IPv4 address associated with the WLAN interface
4. Navigate to `SecurityOnion/setup` (assuming you're on the user home directory)
5. Here, we need to edit the `so-functions` file (which is a bash script). Keep in mind that the only available editor in this environment is `vim`.
6. Search for every instance of `|wl`. There should be two instances, both being part of `awk -F: '$0 !~ "lo|vir|veth|wl|br|docker|^[^0-9]"{print $2}'`. Delete the `|wl` filter in both of them.
8. Run `so-setup iso` and continue with the installation. By now it should not block your installation when using a single NIC machine, and you should be able to select a WLAN interface. When prompted with the management interface's IP address, enter the IPv4 address associated with the WLAN interface.
