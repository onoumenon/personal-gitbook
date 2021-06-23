# Tech Fundamentals

### YAML: human readable data serialisation language

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.38.38-pm.png)

### JSON: javascript object notation

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.43.47-pm.png)

JSON can be used for both Cloudformation and identity documents. YAML is used for Cloudformation.

### OSI model

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.47.40-pm.png)

Media layers: Layer 1 to 3

Host layers: 4 to 7

### Layer 1 - Physical

Physical connection \(ie: network cable\)

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.51.55-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.49.49-pm.png)

#### Attributes:

* No device addressing, all data is processed by all devices \(it's like shouting to a room of strangers\)
* If multiple devices transmit at once, a collision occurs
* No media access control or collision detection

### Layer 2 - Data Link

Builds upon layer 1, Layer 2 example: Ethernet

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.57.34-pm.png)

Frames are a format for sending info over, is a container of sorts.

MAC addresses are uniquely tied to the hardware. 

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.06.24-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.19.03-pm.png)

Layer 2 avoids collision as it checks carrier first before transmitting

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.21.22-pm.png)

CSMA/CD: [https://en.wikipedia.org/wiki/Carrier-sense\_multiple\_access\_with\_collision\_detection](https://en.wikipedia.org/wiki/Carrier-sense_multiple_access_with_collision_detection)

{% embed url="https://www.youtube.com/watch?v=iKn0GzF5-IU" %}

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.54.53-pm.png)

#### Attributes:

* Identifiable devices \(via MAC address\)
* Media access control \(sharing\)
* Collision detection
* Unicast \(1 to 1 instead of broadcasting to all\)
* Broadcast \(1 to all\)
* Switches \(similar to hubs, but able to direct messages to specific port\)

### Decimal &lt;=&gt; Binary

\(for IPv4 Addressing\)

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.59.29-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-9.33.17-pm.png)

133 is 10000101 in binary: you can use the binary table to easily convert.

### Layer 3 - Network

![](../../../.gitbook/assets/screenshot-2021-06-22-at-9.40.33-pm.png)

AKA inter-networking -&gt; internet, uses IP Protocol

In layer 2, information is encapsulated in frames, on layer 3, it's encapsulated in packets.

 IP packets contains certain fields:

* Source IP Address
* Destination IP Address
* Protocol \(TCP, ICMP, UDP\)
* Data
* Time to live \(maximum hops\)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-9.47.54-pm.png)

### IP address

eg: 133.33.3.7 -&gt; human readable ip address, dotted decimal notation

![](../../../.gitbook/assets/screenshot-2021-06-22-at-9.50.16-pm.png)

Each part is an 8bit binary \(octet\), which adds up to 32 bits \(4 bytes\), or 4 \* 8bits \(Octets\)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-9.52.25-pm.png)

IP addresses should be unique.

### Subnet Mask

allows host to check if ip address is local or remote, whether it needs to use gateway or not

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.17.20-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.18.09-pm.png)

### Route tables

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.20.12-pm.png)

Route table prefers more specific to least specific, so it can hop by hop get to the destination

### Address Resolution Protocol

When subnet mask is used to determine that a destination ip address is local, it will use ARP to send data.

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.26.31-pm.png)

 1. On layer 3, instructions to send data from 1 IP to another IP is received

2. Uses subnet mask to check if address is local

3. It's local, so it uses layer 2 ARP. ARP discovers which MAC = destination IP

4. Layer 2 broadcasts to check destination IP

5. Receives a response from a corresponding address

6. Layer 2 encapsulates data in frame to transmit on layer 1

7. L1 hands over data in frame to L2

8. L2 strips frame and passes payload to L3, which passes it to game



Note: Router has MAC address, and routes packages \(doesn't drop if it is not destination like a computer\)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.37.38-pm.png)

#### Attributes

* IP Addresses \(IPv4, IPv6\)
* ARP - find the MAC address, for this IP
* Route - where to foward packet
* Route Tables - multiple routes
* Router - moves packets from source to destination
* Device &lt;=&gt; device communication over internet
* No method for channels of communications \(src IP &lt;=&gt; dest IP only, cannot have different apps communicating at the same time\)
* Can be delivered out of order

### Layer 4 - Transport \(and maybe layer 5\)

As layers are conceptual, some distinctions are blurry.

This layer solves limitation of L3 where packets are out of order cos of different routes, or missing packets. L3 doesn't distinguish between apps/ channels.

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.44.41-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.46.07-pm.png)

IP packet wraps/ encapsulates TCP segments:

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.49.40-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.52.45-pm.png)

How it relates to AWS is that you need to set a range of tcp ports for ephemeral ports, and add firewall rules.

![](../../../.gitbook/assets/screenshot-2021-06-22-at-10.55.40-pm.png)

Then only does connection establish, and client can send data. It's to synchronise what sequence numbers the other 'party' will be using.

Stateless firewall: two rules, outbound and inbound

Network ACL \(aws\), it's basically this

Stateful firewall: \(maybe layer 5\) only cares about outbound, inbound is implicit

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.01.15-pm.png)



IPv4 is required to be unique, but was/is encountering shortage, so NAT was created \(only for IPv4\) \(IPv6 solves it by making more addresses avail\)

PAT \(likely used in your home router network\)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.04.07-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.08.43-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.10.29-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.13.38-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.14.56-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.17.06-pm.png)

#### Private addresses

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.18.40-pm.png)

Aim to allocate non-overlapping addresses for private use

IPv6 has a LOT more addresses

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.19.48-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.24.02-pm.png)

starts at midway point, entire internet is a /0 network, 0.0.0.0 matches entire network

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.28.22-pm.png)



### DDOS

![](../../../.gitbook/assets/screenshot-2021-06-22-at-11.29.47-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-23-at-9.44.38-am.png)

Attacker exploits the fact that requests are cheaper than responses. It's like throwing hand grenades.

![](../../../.gitbook/assets/screenshot-2021-06-23-at-9.46.03-am.png)

Attacker exploits 3-way handshake protocol means server is waiting for response \(the 2nd handshake\), but never gets any since SYNs are spoofed.

![](../../../.gitbook/assets/screenshot-2021-06-23-at-9.50.27-am.png)

Attacker exploits protocol where a response is significantly larger than the request, ie: spoofed dns requests.

{% embed url="https://www.varonis.com/blog/dns-cache-poisoning/" %}

{% embed url="https://www.computerworld.com/article/2516831/china-s-great-firewall-spreads-overseas.html" %}

Tricky part is block



