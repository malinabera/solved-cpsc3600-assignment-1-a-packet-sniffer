Download Link: https://assignmentchef.com/product/solved-cpsc3600-assignment-1-a-packet-sniffer
<br>
This assignment will introduce the concept of packet sniffing, header processing, and packet manipulation. Packets can be sniffed using Python’s built in <a href="https://docs.python.org/3/library/socket.html#socket.SOCK_RAW">raw sockets</a><a href="https://docs.python.org/3/library/socket.html#socket.SOCK_RAW">,</a> which allow you to read all packets coming in on a particular network interface as well as directly manipulate the different layer headers for packets sent over the raw socket. However, Windows does not natively support raw sockets, so we will be using the library <strong><em>scapy </em></strong>to help us sniff packets.

<h1>1           Assignment Instructions</h1>

You’ll find seven Python files in the project template. You will need to complete code in six of these.

<ol>

 <li><strong>py</strong>: This class is the actual packet sniffer that detects packets and routes them to your remaining code for processing and unpacking. You will need to complete a few lines of code in this file that handle what header type should be unpacked nexted, based on a value stored in the current header.</li>

 <li><strong>layer header.py</strong>: This is an abstract class defining properties and functions that will be used in the header classes defined below. You should not edit this file.</li>

 <li><strong>link layer headers/ethernet header.py</strong>: This class represents an Ethernet header. The properties of the Ethernet header are already defined. You will need to complete the constructor to unpack the header bytes and assigned them to the defined properties of the class.</li>

 <li><strong>network </strong><strong>layer headers/ipv4 header.py</strong>: This class represents an IPv4 header. The properties of the IPv4 header are already defined. You will need to complete the constructor to unpack the header bytes and assigned them to the defined properties of the class.</li>

 <li><strong>network layer headers/arp header.py</strong>: This class represents an ARP header. The properties of the ARP header are already defined. You will need to complete the constructor to unpack the header bytes and assigned them to the defined properties of the class.</li>

 <li><strong>transport layer headers/tcp header.py</strong>: This class represents an TCP header. The properties of the TCP header are already defined. You will need to complete the constructor to unpack the header bytes and assigned them to the defined properties of the class.</li>

 <li><strong>transport layer headers/udp header.py</strong>: This class represents an UDP header. The properties of the UDP header are already defined. You will need to complete the constructor to unpack the header bytes and assigned them to the defined properties of the class.</li>

</ol>

<h2>1.1         Installing Scapy</h2>

Scapy can be installed using pip, python’s package management system. It should already be set up if you have installed python. You should be able to install scapy by opening a terminal and running the command <strong>pip install scapy</strong>. If you are using a Windows machine, you will also need to install <a href="https://nmap.org/npcap/">Npcap</a><a href="https://nmap.org/npcap/">.</a> This is likely already installed if you have Wireshark on your computer.

Be aware that python maintains different package repositories for each version of python installed on a machine, as well as for each virtual machine you’ve configured. If you install scapy and it is not recognized in your development environment, you likely have your IDE set to use a different installation of python than you command window defaults to.

<h1>2           Protocol Headers</h1>

Your sniffer neesd to be able to handle five different header types. I’ve included the format for these headers in this section. We haven’t discussed two of these headers yet, however both are straightforward to work with.

<h2>2.1         TCP Header</h2>

TCP’s header is composed of a fixed-length header of 20 bytes and a variable-length set of options. The typical

TCP header does not include any options, however, since it is always a possibility you will always need to check to see if any are present. This can be done using the <em>data offset </em>field.

<h2>2.2         UDP Header</h2>

UDP’s header is always 8 bytes in length.

<h2>2.3         IPv4 Header</h2>

IPv4’s header is composed of a fixed-length header of 20 bytes and a variable-length set of options. The typical IPv4 header does not include any options, however, since it is always a possibility you will always need to check to see if any are present. This can be done using the <em>IHL </em>field.

<h2>2.4         ARP Header</h2>

ARP’s header is technically variable length, however for our purposes we’ll assume that it is always 28 bytes in length. The HLEN and PLEN values store the length of the hardware and protocol addresses. ARP messages typically communicate MAC addresses (associated with the link layer) and IP addresses (associated with the network layer). When this is true, the hardware address fields are composed of six bytes and the protocol address fields are composed of four bytes, meaning that HLEN is set to 6 and PLEN is set to 4. If different addresses are communicated then the header may change length depending on how many bytes are used to represent those addresses.

<h2>2.5         Ethernet</h2>

Ethernet’s header is always 14 bytes in length.

<h1>3           Test cases</h1>

The autograder will test each of the parameters of each packet header separately. It does this by creating a packet with a known value for a given header parameter (the value is chosen randomly) and then sending that packet across the network while your sniffer is running. It sends the packet twice in case one were to be missed by your packet sniffer. It then examines the packets returned by your sniffer to see if one of them contains the expected value for the target header parameter.

It should be noted that it is technically possible for a test to fail due to packet loss or corruption. If a test randomly fails, try re-submitting your project. If the test fails again, it is likely a problem with your code.