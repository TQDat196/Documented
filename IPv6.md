# IPv6 for CE-Router
## Setup Enviroment for CE-8300 | DALLAS
### Setup test tool
* Install Virtual Machine (VMWare, Virtual Box, ...). #Run as Administrator#
* On Virtual Machine:
    * Follow test tool setup inside [Test Installation Guide](http://interop.ipv6.org.tw/CERouter/pdf/CE_CFT_install_guide.pdf)
  * Most of thing have been setting up already. Therefore, you've just need to config :
    * "CE-Router MAC Address" 
    * "Name of Tester Interface".
* CE-Router_Self_Test_1_0_X's directory : `/usr/local/www/apache22/data/CE-Router_Self_Test_1_0_4.tar.gz`
* Use `#dhclient em2` to setup DHCP
* reset wifi `$ifconfig em2 down + $ ifconfig em2 up`  if DHCP setup do smthing wrong.
* Run Test Case : <br>
Ex: Run Test case 10 of wan_rfc1981: <br>
  * Move into directory wan_rfc1981.
  * Run test tool with case 10 : <br>
`# make AROPT="-s 10 -e 10" ipv6ready_p2_ce` <br>
  Checkout last page of [Test Installation Guide](http://interop.ipv6.org.tw/CERouter/pdf/CE_CFT_install_guide.pdf) for more information.

## Access to CE-Router
- Install [minicom](https://www.poftut.com/install-use-linux-minicom-command-tutorial-examples/) to virtual access CE Router with (usr/pass) = (root/admin).#Run as Administrator#
- Run `# sudo killall -9 minicom` to close all process're  [minicom](https://www.poftut.com/install-use-linux-minicom-command-tutorial-examples/).

## Build image and flash to CE-Router:
* Build image
   * Clone link:http://pm.veriksystems.com:8181/belkin/nodes_dev_tb_ipv6[Ipv6 Project] to your building directory.
   * Move to project build (Ex: rogue).
   * Use *# make menuconfig* to access configuration.
   * Link or Download all lib needed for your project (dir: output/dl)
   * Build it.

* Flash on CE-Router:
  * Copy `.img` file into your flash dir. (Ex: [Build with docker](http://pm.veriksystems.com:8181/vu.truong/verik_build))
  * In minicom:
    * `# reboot -f`
    * Press "**enter key**" to enter setting.
    * `# set serverip <ip_server>`
    * `# set img <flash_dir>`
    * `run flashimg` if` boot_part=1` | `run flashimg2` if `boot_part=2`.
    * Plug ethernet cable's server to WAN's CE-Router.
    * `# reset`

## Notice:
* Reset DHCP on CE-Router: *# sysevent set dhcpv6_client-restart* before any testing process.

## Documents & files:
* [Test Installation Guide](http://interop.ipv6.org.tw/CERouter/pdf/CE_CFT_install_guide.pdf)
* Check 'report.html' file inside each Rfc test case for more info.
* Sample testing report for all rfc can find in  [CE-Router Self-Test Report](http://interop.ipv6.org.tw/CERouter/CE-Router_Self_Test_1_0_1/) (This test report is out of date).

# IPv6        
## Terminology
* TCP : Transmission Control Protocol.
* UDP : Universal Diagram Protocol.
* Telnet : Remote login service.
* ARP : Address Resolution Protocol (Mapping Internet Address to Physical Address).
* Reverse ARP : Mapping hardware address to Internet Address.
* IPv6 : Internet Protocol version 6.
* ICMPv6 : Internet Control Message Protocol for IPv6.
* DHCPv6 : Dynamic Host Control Protocol for IPv6.
* DNSv6 : Domain Name System for IPv6.



## ICMPv6
* ICMPv6 is used to make:
   * echo request, echo reply (ping) 
   * report errors encountered in processing packets like:
      * Destination Unreachable
      * Package Too Big
      * Time Exceeded
      * ...

## Path MTU Discovery for IPv6
[Rfc1981 Doc](https://tools.ietf.org/html/rfc1981)

* Define:
  * When one IPv6 node has a large amount of data to send to another node, the data is transmitted in series of IPv6 packets.  It is usually preferable that these packets be of the largest size that can successfully traverse the path from the source node to the destination node.  This packet size is referred to as the Path MTU (PMTU), and it is equal to the minimum link MTU of all the links in a path.

* Flow:
  * A node received a Packet Too Big message (ICMPv6) reporting a next-hop MTU.
  * Package tranfer have to have fragment header included. 
```C
IPv6_min_MTU = 1280;
 if(next-hop MTU < IPv6_min_MTU)
   PMTU = IPv6_min_MTU;
 else  
   PMTU = next-hope MTU; 
```
*   * PMTU or MTU must not increase after 5 mins (*10 mins* is recommend)

## Neighbor Discovery for IPv6
[Rfc4861 Doc](https://tools.ietf.org/html/rfc4861)

* Router Solicitation
* Router Advertisement
* Neighbor Solicitation
* Neighbor Advertisemnt
* Redirect

## DHCPv6
* Basic operation: Provide configuration, address, automated delegation for clients.

## IPv6 Prefix Options for DHCPv6
[Rfc 3633 Doc](https://tools.ietf.org/html/rfc3633)

**IPv6 Prefix**

IPv4 have a subnet mask but instead of that IPv6 use prefix length. <br>
Ex: `2001:1111:2222:3333::/64` just like `192.168.1.1/24` <br>
`/64` : Number of bits that we use for the prefix.


**Identity association (IA)**
Is a construct through which a server and a client can identify, group, and manage a set of related IPv6 addresses.  Each IA consists of an IAID and associated configuration information. 
* **IA_ID**: The unique identifier for this IA_NA; the IAID must be unique among the identifiers for all of this client's IA_NAs. 
* **IA_TA**: IA temporary address (option). 
* **IA_PD** : IA_PD option is used to carry a prefix delegation identity association


**Prefix Delegation**
* A delegating router can delegate prefixes to requesting routers.
* The Prefix Delegation options provide a mechanism for automated
   delegation of IPv6 prefixes using the Dynamic Host Configuration
   Protocol (DHCP).  This mechanism is intended for delegating a long-
   lived prefix from a delegating router to a requesting router, across
   an administrative boundary, where the delegating router does not
   require knowledge about the topology of the links in the network to
   which the prefixes will be assigned.

**T1 (seconds)**
The time at which the client contacts the server from which the addresses in the IA_NA were obtained to extend the lifetimes of the addresses assined to the IA_NA.


**T2 (seconds)**
The time at which the client contacts any available server to extend the lifetimes of the addresses assigned to the IA_NA.

**Renew**
A client sends a Renew message to the server that originally provided the client's addresses and configuration parameters to extend the lifetimes on the addresses assigned to the client and to update other configuration parameters.

**Reply**
A server sends a Reply message containing assigned addresses and configuration parameters in response to a Solicit, Request, Renew, Rebind message received from a client.  A server sends a Reply message containing configuration parameters in response to an Information-request message.  A server sends a Reply message in response to a Confirm message confirming or denying that the addresses assigned to the client are appropriate to the link to which the client is connected.  A server sends a Reply message to acknowledge receipt of a Release or Decline message.

## DNSv6:
