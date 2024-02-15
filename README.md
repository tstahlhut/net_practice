# net_practice
A 42 project to learn about networks

### Level 1: What is an IP Address?

IP address is short for Internet Protocol Address. It is the address which identifies a device in a computer network that uses the Internet Protocol suite (TCP/IP) for communication, such as the internet.

#### IPv4

Most common are IPv4 IP addresses like this one:

  	192.168.110.222

IPv4 stands for Internet Protocol version 4. It defines the IP address as a 32-bit number. This means 4 times 8 bits. Thus an IPv4 address can range from 0.0.0.0 to 255.255.255.255. 

#### IPv6

As the internet is growing and there are not enough IPv4 addresses for all the devices, IPv6 addresses have been introduced. These use 128-bit numbers, such as:

  	2001:db8:0:1234:0:567:8:1

8 times 16 bits, represented by 4 hexadecimal numbers in 8 groups. As you can see, it is common practice to shorten IPv6 addresses by omitting the zeros. 

#### Purpose

As written in the beginning, an IP address is used to __identify__ a device (host) on a network which uses TCP/IP. To be more precise, to identify the devices network interface. 
<details> <summary> Network Interface Controller </summary>

A network interface controller (NIC) is a piece of hardware that connects the computer to a computer network. This can be a local area network (LAN) or the internet. It provides the physical layer and data link layer which are necessary for connections via Ethernet or Wi-Fi. 

The network interface controller is also known as network interface card, network adapter, LAN adapter or physical network interface. 

In the beginning, network interface controllers were implemented on expansion cards that were plugged in and connected to the motherboard, but nowadays, the network interface is often built directly into the motherboard. 

"The network controller implements the electronic circuity required to communicate using a specific physical layer and data link layer standard such as Ethernet or Wi-Fi. This provides a base for a full network protocol stack, allowing communication among computers on the same local area network (LAN) and large-scale network communications through routable protocols, such as Internet Protocol (IP)." ([Wikipedia](https://en/wikipedia.org/wiki/Network_interface_controller))
</details>

Furthermore, the IP address provides the __location__ of the host in the network so that a path from one host to another host can be established. When communicating through this network, a IP packets are send from one host to another host. In their header is the IP address from the sender and the destination. 

If two devices are on the same network, they share the first 3 positions, only the fourth differs. (This is all you need to know for level 1).
  
