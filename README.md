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
    
