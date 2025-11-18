# BI-ST3
just a bunch of random notes
# Introduction
## Physical requirements
  * mini USB / serial
  * RJ-45
## Software requirements
  * console
  * tftp server
  * http server
## Device
  * FirePower 1010 / ASA 5505 
## rommon (without os) ASA 5505
  * configure network
    * prerequirement: create network with tftp server
    * commands:
      * ADDRESS=10.0.100.250
      * SERVER=10.0.1.19
      * GATEWAY=10.0.0.1
  * format flash
    * commands:
      * erase disk0:
      * IMAGE=asa917-32-k8.bin
      * PORT=Ethernet0/0
      * set
      * tftp
## ciscoasa
  * commands:
    * enable
    * conf t
    * interface VLAN10
    * nameif INSIDE
    * ip addr 10.0.100.255 255.255.0.0
    * no sh
    * exit
    * int ethernet0/0
    * switchport mode access
    * switchport access vlan 10
    * ping 10.0.1.19 # wont work, blocked
    * show int ip brief
    * format flash:
    * copy tftp flash # should work
    * remote host: 10.0.1.19
    * source filename: asa917-32-k8.bin
  * basic commands:
    * hostname cisco
    * interface VLAN100
      * ip address 10.90.0.1 255.255.0.0
      * security-level 100
      * nameif INSIDE
      * no sh
    * int ethernet 0/2
    * switchport mode access
    * switchport access vlan 100
    * int ethernet 0/3
    * ...
    * write
    * username bug password supersecretcisco
    * crypto key gen rsa modulus 2048
    * http server enable
    * http 0.0.0.0 0.0.0.0 ## allowed everyone
    * int vlan 10
    * security-level 0
    * nameif ISP1
    * ip address dhcp setroute # ip address 10.0.100.254 255.255.0.0
    * int ethernet 0/0
    * swichport mode access
    * switchport access vlan 10
    * no shutdown
    * show ip interface brief

  ## Edit java.security
    * comment all md5 references
    * install cisco asdm
    
# GUI
## BASIC
  * dont forget NTP
# SETUP 1
 * client -> (eth0/2) ASA 5505
 * ASA 5505 (eth0/0) -> Home router -> ISP
 * ASA 5505
  * NAT
  * DHCP client (WAN)
  * DHCP server (LAN)
## DHCP
 * Device Management -> DHCP server -> ...
## DNS
 * Device Management -> DNS server -> ...
 * specify interfaces for lookup
## NAT
 * Firewall -> NAT rules -> Add Network object (right panel)
   * RESERVE-INSIDE-NET
     * Type: Network (IPv4)
     * IP address: 10.90.0.0
     * Mask: 255.255.0.0
 * edit RESERVE-INSIDE-NET
   * Show NAT
   * Type: Dynamic PAT
   * Translated Addr: interface ISP1
   * save configuration
 # SETUP 2
  * DNS 10.0.0.8 -> FirePower 1010 (185.91.169.169)
  * IP SEC to SETUP 1
 ## SITE TO SITE VPN Client
  * Wizzards -> VPN wizzards -> Site-to-site VPN wizzard...
  * Peer IP address: 185.91.169.169
  * VPN access interface: ISP1
  * Traffic to protect:
    * Local Network: RESERVE-INSIDE-NET
    * Remote Network: NUPAKY--INSIDE-NETWORK
   * Security
     * Pre shared key
   * Finish
   * Edit
     * IKE Policy manage...
       * name: moucha
       * Encryption: aes-256
       * Hash: Sha256
     * IPsec proposal Select...
       * name: moucha
       * mode: Tun..
       * ...
    * Edit NAT to keep destination and source addressess between VPN networks
