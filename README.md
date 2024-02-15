# net_practice
A 42 project to learn about networks

### Level 1: What is an IP Address?

IP address is short for Internet Protocol Address. It is the address which identifies a device in a computer network that uses the Internet Protocol suite (TCP/IP) for communication, such as the internet.

#### IPv4

Most common are IPv4 addresses which are represented in 4 decimal numbers, like this one:

  	192.168.110.222

IPv4 stands for Internet Protocol version 4. It defines the IP address as a 32-bit number. This means 4 times 8 bits, which is why they are called octets. Thus, an IPv4 address can range from 0.0.0.0 to 255.255.255.255. 

<details> <summary> How to find your IP address? </summary> 

On Linux:

	ifconfig

On Windows:

	ipconfig

</details>

#### IPv6

As the internet is growing and there are not enough IPv4 addresses for all the devices, the Internet Protocol has been redesigned in 1995. The result is IPv6 which has a much larger address space than IPv4. This is because IPv6 addresses are 128-bit numbers, such as:

  	2001:db8:0:1234:0:567:8:1

They are represented by 4 hexadecimal numbers in 8 groups, which is 8 times 16 bits. As you can see, it is common practice to shorten IPv6 addresses by omitting the zeros. 

Nowadays, both versions (IPv4 and IPv6) are in use. 

### Purpose

As stated in the beginning, an IP address is used to __identify__ a device (in networking called a __host__) on a network which uses TCP/IP. To be more precise, to identify the devices network interface. 
<details> <summary> Network Interface Controller </summary>

A network interface controller (NIC) is a piece of hardware that connects the computer to a computer network. This can be a local area network (LAN) or the internet. It provides the physical layer and data link layer which are necessary for connections via Ethernet or Wi-Fi. 

The network interface controller is also known as network interface card, network adapter, LAN adapter or physical network interface. 

In the beginning, network interface controllers were implemented on expansion cards that were plugged in and connected to the motherboard, but nowadays, the network interface is often built directly into the motherboard. 

"The network controller implements the electronic circuity required to communicate using a specific physical layer and data link layer standard such as Ethernet or Wi-Fi. This provides a base for a full network protocol stack, allowing communication among computers on the same local area network (LAN) and large-scale network communications through routable protocols, such as Internet Protocol (IP)." ([Wikipedia](https://en/wikipedia.org/wiki/Network_interface_controller))
</details>

Furthermore, the IP address provides the __location__ of the host in the network so that a path from one host to another can be established. When communicating through this network, IP packets are send from one host to another host. In the IP packet's header is the IP address from the sender and the destination. 

### The Structure: How to read an IPv4 address?

IPv4 addresses can be split into two parts:  the **the network prefix** and the **host identifier** (or **interface identifier** in IPv6). The first bits are the most significant ones and identify a whole network or subnet, whereas the last bits are the least significant ones and identify a specific interface of a host on that network.

In a home network, normally, the first three octets stand for the network and the last octet identifies the host. So, if two devices are on the same network, they share the first 3 octets, only the fourth differs. (This is almost all you need to know for level 1).

#### Router (Default Gateway)

The router distributes the IP addresses in your home network. 
DHCP is what makes this work. 
Every IP address in a home network starts with 192.168.1. The router knows this thanks to the Netmask.

#### Netmask (Subnet Mask)

The IP address of the Netmask determines which octets may be changed by the router to generate IP addresses for your home network. In other words, it defines which portion of the IP address indicates the network and which the host. 255 means that the number cannot be changed, whereas 0 means that the number may range from 0 to 255.

		Default Gateway: 192.168.1.1
		Subnet Mask: 255.255.255.0

In the above example the first three octets (192.168.1) are not allowed to be changed. Whereas the last octet may have any number from 0 to 255. However, it is not that simple. Two numbers are already reserved. The first number (0) which is the __network address__:

		192.168.1.0

And the last number which is the __broadcast address__:

		192.168.1.255

If you send something to the broadcast address it will broadcast it to __all__ the other IP addresses. Moreover, the second number (1) is already given to the __router__, the default gateway, itself:

		192.168.1.1

This leaves 253 available addresses for your home network. 

#### Classes

To dive deeper into the structure of IPv4 addresses and why there are not enough for all the devices connected to the internet today, we have to look at the <details> <summary> __classful network__ </summary>

Just before the Internet Protocol Suite (TCP/IP) was standardized in 1982, the Internet Assigned Number Authority (IANA) divided up the IP addresses into classes and the classful network was introduced in 1981. Although, it was replaced by the __Classless Inter Domain Routing__ in 1993, the classes still explain some of the structures of IP addresses.

__Class A__ was given to governments and companies by IANA. As they thought that these would need a lot of IP addresses only the first 8 bits were reserved for the network and the last 24 bits for the host identifier. This gives each network over 16 million IP addresses! But allows only for 126 networks...

		Class A Range:	1.0.0.0 - 126.255.255.255
		Subnet Mask: 	255.0.0.0

(Of course, what they do nowadays is split them up into subnets.)

__Class B__ still allows a lot of hosts for one network - over 65,000:

		Class B range:	128.0.0.0 - 191.255.0.0
		Subnet Mask:	255.255.0.0

__Class C__ is the most familiar one that we use for our home networks:

		Class C range:	192.0.0.0 - 223.255.255.0
		Subnet Mask:	255.255.255.0

Nowadays, we mostly use classless IP addresses.
Class D and E do not have a subnet mask because the purpose for which they are used does not require a distinction between network and host. 

__Class D__ is reserved for multicast.

		Class D range: 224.0.0.0 - 239.255.255.255

__Class E__ are reserved for other reasons. [Network Chuck](https://www.youtube.com/watch?v=tcae4TSSMo8) says they are "experimental". 

__Loopback Addresses__

As you might have noticed all the IP addresses starting with 127 are missing. They are called loopback addresses and are used for network testing. We can test our network by pinging it:

		ping 127.0.0.1

You may also use any other IP address starting with 127 to test your network. 

</details>

### Bits and Bytes: Subnetting

Needless to say, that IPv4 addresses are in reality 32 bits and thus in binary. In order to really work with IP addresses (keyword: subnetting) you need to calculate with them in binary. 

<details> <summary> Convert IPv4 address from binary to decimal </summary>

In order to do this, it is helpful to write down the exponential sequence of 2:

		128 64 32 16 8 4 2 1

One could think like this: The first bit represents the number 128 (2^7), the second the number 64 (2^6) and so forth until the last bit which represents 2^0 which is 1. If there is a 1, you count this number, if not, not. 

		11000000.10101000.00000001.0

The first octet would then be: 128 + 64 = 192. The second octet: 128 + 0 + 32 + 0 + 8 = 168. So, the above IP address would be in decimal:

		192.168.1.0

</details>

<details> <summary> Convert IPv4 address from decimal to binary </summary>

As in the example above, it is helpful to write down the exponential sequence of 2:

		128 64 32 16 8 4 2 1

Now, if we have the following IP address:

		172.16.34.3

We will start by asking if 172 can be divided by 128 (-> yes) and mark down a 1. We substract 128 from 172 which is 44 and continue to the next number in the chart: 64. 44 is smaller than 64 thus we write down 0 and so forth. I think it is easiest to just ask if the decimal number we have is bigger than (or equal to) the number in the chart. If it is, we write down a 1 for that position. If not, we write down a 0:

	x					172 	44	44	12	12	4	0	0
	chart (n)			128 	64 	32 	16	8 	4 	2 	1
	x - n				 44		-	12	-	4	0	-	-
	is x bigger/equal?	yes		no	yes	no	yes	yes	no	no
	binary				1		0	1	0	1	1	0	0

Following this method we will get the following IP address:

	10101100.00010000.00100010.00000011

</details>

For subnetting it is best to work with the binary representation of IP addresses. If we look for example at a subnet mask, the division between network and host portion becomes more obvious:

		255.255.255.0
		11111111.11111111.11111111.00000000

All the bits with a 1 are reserved for the network and all the bits with a 0 may be used to generate IP addresses for the host interfaces in the network. In this network are 254 (2^8 - 2) IP addresses available. If we need more, we have to do subnetting, which just means that we take away bits reserved for the network:

		11111111.11111111.11111110.00000000
		255.255.254.0

This subnet mask allows us 510 (2^9 - 2) IP addresses for our home network. 
