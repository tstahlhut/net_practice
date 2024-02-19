# net_practice
A 42 project that let's you dive deeper into System Administration. You learn about networks with special focus on subnetting and routing tables. 

## 1. What is an IP Address?

IP address is short for Internet Protocol Address. It is the address which identifies a device in a computer network that uses the Internet Protocol suite (TCP/IP) for communication, such as the internet.

<details> <summary> TCP/IP </summary>
TCP/IP (Internet Protocol Suite) is a model for communication in computer networks. In the 1960's different models existed in each company but in the end, TCP/IP became the standard. Nowadays, every computer uses this model. 

TCP/IP is based on 5 different layers:

	Application
	Transport
	Network
	Data Link
	Physical

At the time when it was not yet decided that TCP/IP was to be the model all use, there was another popular model: the OSI model (Open Systems Interconnection). It had 7 layers and these are the layers we still refer to today:

	Application
	Presentation
	Session
	Transport
	Network
	Data Link
	Physical

So, today TCP/IP's 5th layer is been referred to as layer 7 and layer 5 and 6 of the OSI model are included in layer 7:

	[Layer 7:	Application
				protocol, e.g. HTTP
	 Layer 6:	Presentation -> how to present the data?
								Data Format (filetypes, e.g. HTML, JPG, PDF)
								Encryption (e.g. SSL)
	 Layer 5:	Session		-> open up communication
								e.g. L2TP, RTCP, H.245, Socks (Proxy)
	] 
							-> get data ready, put it in right format, open session

	- Layer 4:	Transport 	-> choose protocol
								TCP (more reliable: 3-way-handshake) or 
								UDP (faster)
							-> choose port
								e.g. 443 for HTTPS, 80 for HTTP, 21 FTP, 22 SSH, 3389 RDP
	- Layer 3:	Network		-> IP addresses; routers work on layer 3
	- Layer 2:	Data Link	-> MAC-Addresses; switches and bridges work on layer 2
	- Layer 1:	Physical	-> e.g. Ethernet cables; hubs and repeaters work primarily on layer 1 
													(they just repeat electrical signals)


<details> <summary> TCP (Transmission Control Protocol) </summary> 
TCP is more reliable than UDP because it establishes a connection first. 
This is called the 3-way Handshake:

	1. SYN: Synchronize Sequence Number (source to destination: I want to establish a connection)
	2. SYN/ACK (destination to source: acknowledged & sends sequence number)
	3. ACK (source sends to destination: acknowledged)

Thus, a reliable connection is established. Furthermore, TCP verifies if data reached destination. If it did not, it sends the data again </details>





<details> <summary> Example: Encapsulation </summary>

If we want to access a webpage, we use HTTP or HTTPS as protocol (Layer 7). This layer 7 is transferred down through all layers, until layer 1. This process is called encapsulation. A header with the necessary information for each layer is added to the data which is transferred.

	Layer 7: which protocol?		HTTP 			HTTPS				= 	Data		+= Data
	Layer 4: which port to use? 	HTTP: port 80	HTTPS: port 443		= L4 Header		 += Segment
	Layer 3: which IP addresses?	IP addresses (my PC & webpage)		= L3 Header		   += Packet
	Layer 2: which MAC addresses?	MAC addresses (my PC & my router)	= L2 Trailer & Header += Frame
	Layer 1: send all the information (Frame) over wire


When the router receives the frame, it de-encapsulates it, to get to the packet and the information of layer 3. It reads the destination IP address and changes the source and destination MAC address in layer 2 so that it is forwarded to the next switch which only reads the frame (layer 2). When it reaches its final destination the frame is de-encapsulated layer by layer until the right application reads the message and reacts to it. The encapsulation process starts again when forming the reply. 



</details>

</details>

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

### 1.1 Purpose

As stated in the beginning, an IP address is used to __identify__ a device (in networking called a __host__) on a network which uses TCP/IP. To be more precise, to identify the devices network interface. 
<details> <summary> Network Interface Controller </summary>

A network interface controller (NIC) is a piece of hardware that connects the computer to a computer network. This can be a local area network (LAN) or the internet. It provides the physical layer and data link layer which are necessary for connections via Ethernet or Wi-Fi. 

The network interface controller is also known as network interface card, network adapter, LAN adapter or physical network interface. 

In the beginning, network interface controllers were implemented on expansion cards that were plugged in and connected to the motherboard, but nowadays, the network interface is often built directly into the motherboard. 

"The network controller implements the electronic circuity required to communicate using a specific physical layer and data link layer standard such as Ethernet or Wi-Fi. This provides a base for a full network protocol stack, allowing communication among computers on the same local area network (LAN) and large-scale network communications through routable protocols, such as Internet Protocol (IP)." ([Wikipedia](https://en/wikipedia.org/wiki/Network_interface_controller))
</details>

Furthermore, the IP address provides the __location__ of the host in the network so that a path from one host to another can be established. When communicating through this network, IP packets are send from one host to another host. In the IP packet's header is the IP address from the sender and the destination. 

### 1.2 The Structure: How to read an IPv4 address?

IPv4 addresses can be split into two parts:  the **the network prefix** and the **host identifier** (or **interface identifier** in IPv6). The first bits are the most significant ones and identify a whole network or subnet, whereas the last bits are the least significant ones and identify a specific interface of a host on that network.

In a home network, normally, the first three octets stand for the network and the last octet identifies the host. So, if two devices are on the same network, they share the first 3 octets, only the fourth differs. (This is almost all you need to know for level 1).

#### Router (Default Gateway)

The router distributes the IP addresses in your home network. 
DHCP is what makes this work. 
Every IP address in most home network starts with 192.168.1. Or more generally speaking, the router most commonly has the first IP address after the network IP address (1 if we start with 0). The router knows this thanks to the Netmask.

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

## 2. Welcome to Subnetting!

		IPv4: 192.168.1.0
		Mask: 255.255.255.0

So far we know that a subnet mask like the above allows us a range of IP addresses between 192.168.1.0 and 192.168.1.255. Whereas the first (0) and the last (255) are reserved and the the second (1) typically stands for the router. We have 253 available IP addresses. But what if we need more hosts on our network or more networks? Then we enter the world of subnetting and with it the world of binary.

### 2.1 Bits and Bytes

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

For subnetting we need to work with the binary representation of IP addresses. If we look for example at a subnet mask, the division between network and host portion becomes more obvious:

		255.255.255.0
		11111111.11111111.11111111.00000000

All the bits with a 1 are reserved for the network (24 bits) and all the bits with a 0 may be used to generate IP addresses for the host interfaces in the network (8 bits). This leads us to another way of writing a subnet mask:

		/24

This indicates the number of bits reserved for the network. Thus, the two representations below provide us with the exact same information:

		192.168.1.0
		255.255.255.0

		192.168.1.0/24

### 2.2. Building Networks

#### How to calculate the number of hosts in a network?

If you want to know the amount of host IP addresses a network can have, you count the zeros in the subnet mask and take 2 to the power of the number of zeros:

		11111111.11111111.11111111.00000000
		2 ^ 8 = 254

Then you substract 2, one for the network address and one for the broadcasting address.

		2 ^ number_of_zeros_in_subnet_mask - 2

This network allows 254 host IP addresses. But what if we need more?

#### How to broaden the network to get more host IP addresses

If we need more IP addresses, we have to do subnetting, which just means that we take away bits reserved for the network (the ones):

		11111111.11111111.11111110.00000000
		255.255.254.0

This subnet mask allows us 510 (2^9 - 2) IP addresses for our home network. 

#### How to divide one network into several?

If we have the following network but need several networks, we can divide that network.

		192.168.1.0/24

When we want to broaden a network, we take away bits from the network portion and assign it to the host portion (we change bits with 1 into 0). If we want to divide a network, we do the reverse: we change zeros to ones. 

The number of bits we need to change depends on how many subnetworks we want to create. As we are working in binary, we can only divide the network in 2. This means that if we change 1 bit, we can create two subnetworks.

<details> <summary> Example: Divide 1 network into 2 networks </summary>

before: 1 network with 254 addresses

		192.168.1.0/24
		11111111.11111111.11111111.00000000
		range: 192.168.1.0 - 192.168.1.255

after:	2 networks with each 126 networks

		192.168.1.0/25
		11111111.11111111.11111111.10000000
		network 1: 192.168.1.0 - 192.168.1.127
		network 2: 192.168.1.128 - 192.168.1.255 

</details>


If we want to have even more networks, let's say 3, we have to divide them again. But as we can only divide by 2, this will leave us with 4 networks (2^2).

<details> <summary> Example: Divide 1 network into 3 networks </summary>

before: 1 network with 254 addresses

		192.168.1.0/24
		11111111.11111111.11111111.00000000
		range: 192.168.1.0 - 192.168.1.255

after:	4 networks with each 62 addresses

		192.168.1.0/26
		11111111.11111111.11111111.11000000
		network 1: 192.168.1.0 - 192.168.1.63		(00000000 - 00111111)
		network 2: 192.168.1.64 - 192.168.1.127		(01000000 - 01111111)
		network 3: 192.168.1.128 - 192.168.1.191	(10000000 - 10110101)
		network 4: 192.168.1.192 - 192.168.1.255	(11000000 - 11111111)

As always the first IP address is the IP address for the network, we just take this first IP address from each range to refer to the network and add the subnet mask (number of bits with 1):

		network 1: 192.168.1.0/26 (00 00 00 00)
		network 2: 192.168.1.64/26 (01 00 00 00)
		network 3: 192.168.1.128/26 (10 00 00 00)

Done! </details>

And this logic goes on and on ...

		nb of bits changed from 0 to 1		0	1	2	3	4	5	6	7	8
		networks							1	2	4	8	16	32	64	128	256
		addresses in each network			256	128	64	32	16	8	4	2	1


<details> <summary> Example: Divide 1 network into 5 networks </summary>

In order to get 5 networks, we have to divide again. Now we divided 3 times, which means that we have to change 3 bits in the subnet mask. Now, we have 8 networks with 32 addresses each (meaning 30 addresses we can use):

		11111111.11111111.11111111.11100000
		192.168.1.0/27
		network 1: 192.168.1.0 - 192.168.1.31
		network 2: 192.168.1.32 - 192.168.1.63
		network 3: 192.168.1.64 - 192.168.1.95
		network 4: 192.168.1.96 - 192.168.1.127
		network 5: 192.168.1.128 - 192.168.1.159
		network 6: 192.168.1.160 - 192.168.1.191
		network 7: 192.168.1.192 - 192.168.1.223
		network 8: 192.168.1.224 - 192.168.1.255

For our five networks, we take the first five:

		network 1: 192.168.1.0/27
		network 2: 192.168.1.32/27
		network 3: 192.168.1.64/27
		network 4: 192.168.1.96/27
		network 5: 192.168.1.128/27

</details>

### 2.3 Reverse Subnetting

Reverse subnetting is when you have an IP address, subnet mask and need to know if another IP address is part of the network or what the broadcast IP address is of that network. To figure this out you need ot find out the range of the network the given IP address lives in. To do that follow these steps:

	1. Write down the subnet mask in binary. 
	2. Write down the octets of the IP address that always belong to the network (255 in subnet mask).
	3. Find the increment in the octet that is mixed (ones and zeros). This is the range of the network.
	4. Start the first network at 0 and then increment by the increment. 
	5. Repeat until you find the network the IP address belongs to. 

<details> <summary> Example of Reverse Subnetting </summary>

We have the following IP address

		48.25.24.71/21

The subnet mask is 21 bits with ones and the rest zeros.

		11111111.11111111.11111000.00000000
		255.255.248.0

Thus, the first 2 octets always stay the same. So, we start the first network at

		48.25.0.0

Now we need to find the increment, which is the last bit with a "1" in the subnet mask.

		1	1	1	1	1	0	0	0
		128	64	32	16	8	4	2	1

In this case, the increment is 8. It shows us the range of the networks. As we increment in the 3rd octet, the fourth octet will be 0 in the beginning and 255 at the end. 

		48.25.0.0 - 48.25.7.255

And we continue incrementing until we find the range into which our IP address fits. 

		48.25.8.0 - 48.25.15.255
		48.25.16.0 - 48.25.23.255
		48.25.24.0 - 48.25.31.255

Our IP address is part of the third network. And as we know that the last IP address of a network is the broadcast address, 48.25.31.255 is our broadcast address.

</details>

### 2.4 VLSM (Variable Length Subnet Mask(ing))

What if we need subnetworks of different sizes. How do we divide a network then?

We start where the subnet mask allows us and then divide the network into subnets, starting from the biggest to the smallest. It is as easy as that :)

<details> <summary> Example </summary> 

This example is directly derived from [Network Chuck](https://www.youtube.com/watch?v=OD2vG5st4zI). I recommend to watch the full series on subnetting. It enabled me to write this chapter and understand subnetting.

We have one network which should be divided into 4 subnetworks of different sizes:

		network:	172.21.42.0/24
		mask:		255.255.255.0
		1 for 26 servers
		1 for 117 workers
		1 for 10 guests
		1 for 57 robots

We start where the mask allows us:

		172.21.42.0

Then we create the first network for the highest number of addresses needed (the workers):

		hosts required:	117 -> 128
		range:			0 + 128		172.21.42.0 - 172.21.42.127
		network 1:					172.21.42.0/25

Then the second for the robots. We start where network 1 ended.

		hosts required:	57 -> 64
		range:			128 + 64	172.21.42.128 - 172.21.42.191
		network 1:					172.21.42.128/26

Then the third for the servers:

		hosts required:	26 -> 32
		range:			192 + 32	172.21.42.192 - 172.21.42.223
		network 1:					172.21.42.192/27

And lastly, the fourth network for the guests:

		hosts required:	10 -> 16
		range:			224 + 16	172.21.42.224 - 172.21.42.239
		network 1:					172.21.42.224/28

Be aware that the subnet mask changes depending on the range of the network. 

</details>

## 3. Networks: TCP/IP, Hub, Switch, Router

Now that we have established our networks by giving them a range of IP addresses, we need to connect the hosts inside a network and the networks to one another so that they can communicate with each other and exchange data. 

Inside your __LAN__ (Local Area Network) you may connect all your devices via Ethernet Cables to a Switch.

### What is a switch?

A network switch is a networking hardware with multiple ports (8 to 80 and more). Most commonly, the ports are made for plugging in Ethernet cables. A switch is used to connect the hosts of a network to each other. It is normally the first connector in a network. It is thus a network bridge that forwards data.

The switch "learns" the MAC-address of each device connected to it and can thus forward network packets from one device to another. Thus, it operates on the data link layer (layer 2) of the OSI model (see TCP/IP). When receving a packet, it reads the source and destination MAC address and forwards it to the destination MAC address. Switches do not know anything about IP addresses. Except for special switches which also operate on the network layer (layer 3). These switches are called **layer-3 switches** or **multilayer switches** and have a routing table, just as routers do.

Now that we have our hosts in our network connected via a switch, we need a router to be able to connect to other networks, e.g. a webserver in the internet. 

### What does a Router do?

A router connects networks with networks. Therefore a router does operate on the network layer (layer 3 of the OSI model) which means it works with IP addresses. In the packet it receives, it reads the destination IP address and forwards the packet to it. If the IP address is not on its own network, it forwards it to the router the network address belongs to. The router itself has a different IP address for each gateway port it has. And normally, this IP address is the first IP address in the network it is connected to on that port. 

The process of determining the path an IP packet should take is called **routing**. For this process routers use a **routing table**.

### Routing Table

Each router has a routing table which is like a map it uses to forward IP packets. In this routing table, the router stores all the routes it knows. So, when an IP packet arrives, it searches for the destination IP address in its routing table and forwards it. If the IP address is not listed in the routing table, the router uses its default gateway.  

Routers can learn new paths by two methods: dynamic routing and static routing. 

#### Pre-configure a router

Go to global config

	conf t

Configure interface(s) and enable it

	interface g0/0
 	ip address 192.168.1.1 255.255.255.0
  	no shutdown

    	interface g0/1
     	ip address 192.168.2.1 255.255.255.0
      	no shutdown

       	show ip int br


The model for communcation which is used nowadays is TCP/IP.

### TCP/IP (Internet Protocol Suite)

TCP/IP (Internet Protocol Suite) is a model for communication in computer networks. In the 1960's different models existed in each company but in the end, TCP/IP became the standard. Nowadays, every computer uses this model. 

TCP/IP is based on 5 different layers:

	Application
	Transport
	Network
	Data Link
	Physical

At the time when it was not yet decided that TCP/IP was to be the model all use, there was another popular model: the **OSI model (Open Systems Interconnection)**. It had 7 layers and these are the layers we still refer to today:

	Layer 7:	Application
	Layer 6:	Presentation
	Layer 5:	Session
	Layer 4:	Transport
	Layer 3:	Network
	Layer 2:	Data Link
	Layer 1:	Physical

So, today TCP/IP's 5th layer is been referred to as layer 7 and layer 5 and 6 of the OSI model are included in layer 7:

	[Layer 7:	Application	 -> which application & protocol?
								e.g. HTTP, HTTPS
	 Layer 6:	Presentation -> how to present the data?
								Data Format (filetypes, e.g. HTML, JPG, PDF)
								Encryption (e.g. SSL)
	 Layer 5:	Session		-> open up communication
								e.g. L2TP, RTCP, H.245, Socks (Proxy)
	] 
							-> get data ready, put it in right format, open session

	- Layer 4:	Transport 	-> choose protocol
								TCP (more reliable: 3-way-handshake) or 
								UDP (faster)
							-> choose port
								e.g. 443 for HTTPS, 80 for HTTP, 21 FTP, 22 SSH, 3389 RDP
	- Layer 3:	Network		-> IP addresses; routers work on layer 3
	- Layer 2:	Data Link	-> MAC-Addresses; switches and bridges work on layer 2
	- Layer 1:	Physical	-> e.g. Ethernet cables; hubs and repeaters work primarily on layer 1 
													(they just repeat electrical signals)


<details> <summary> TCP (Transmission Control Protocol) </summary> 
TCP is more reliable than UDP because it establishes a connection first. 
This is called the 3-way Handshake:

	1. SYN: Synchronize Sequence Number (source to destination: I want to establish a connection)
	2. SYN/ACK (destination to source: acknowledged & sends sequence number)
	3. ACK (source sends to destination: acknowledged)

Thus, a reliable connection is established. Furthermore, TCP verifies if data reached destination. If it did not, it sends the data again </details>





<details> <summary> Example: Encapsulation </summary>

If we want to access a webpage, we use HTTP or HTTPS as protocol (Layer 7). This layer 7 is transferred down through all layers, until layer 1. This process is called encapsulation. A header with the necessary information for each layer is added to the data which is transferred.

	Layer 7: which protocol?		HTTP 			HTTPS				= 	Data		+= Data
	Layer 4: which port to use? 	HTTP: port 80	HTTPS: port 443		= L4 Header		 += Segment
	Layer 3: which IP addresses?	IP addresses (my PC & webpage)		= L3 Header		   += Packet
	Layer 2: which MAC addresses?	MAC addresses (my PC & my router)	= L2 Trailer & Header += Frame
	Layer 1: send all the information (Frame) over wire


When the router receives the frame, it de-encapsulates it, to get to the packet and the information of layer 3. It reads the destination IP address and changes the source and destination MAC address in layer 2 so that it is forwarded to the next switch which only reads the frame (layer 2). When it reaches its final destination the frame is de-encapsulated layer by layer until the right application reads the message and reacts to it. The encapsulation process starts again when forming the reply. 



</details>





		
