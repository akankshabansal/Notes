### Internet Protocol

* Connectionless 
* Unreliable 
* Best effort

_TTL_ : Time to live. Whenever a packet is routed from one router to another the counter TTL is reduced. 
This is done because its possible for the routers to have loops within them, and they want to make sure packets are not stuck forever in a loop.  

**Trace Routing** If I want to figure out which routers are being used for my source to destination, you can send a msg 
with the given packet and a TTL value of 1, send it again with TTL =2 and so on to figure out the exact traceroute for a packet. 
Use DNS to find the bank IP

_Routers_ : they try and ensure that packets reach their destination.
Ports are part of TCP header and not the IP header.

__Fragmentation__ : Its possible that packet received is of more size than what the link’s bandwidth can handle. 
The router is responsible for breaking and reassembling the packet. 

__ICMP__ (Internet control message protocol )errors : different standard msges used by internet protocol to indicate what’s 
going on. Error 0 => Echo back used for ping command 

**Security concerns** No source msg authentication. Its easy to override the incoming msg. It can flood machines with 
random msgs from different location, can cause infomration leakage.

**TCP protocol** Sits above the IP layer, TCP layer is responsible for getting packets in order and ensuring they 
get delivered. Reliable stream of data. Session is established between source and destination. Makes use of ACK’s and 
Sequence numbers to ensure data is received in order. Start off with a random sequence number in handshake. And if subsequent 
packets do not contain these same random numbers in the response received from opposite end, msgs are dropped. 
=> choosing these sequence numbers at random prevents some kind of attack.. 

**Security concerns** TCP is reliable against network errors and not against a adversary. 
Eavesdropping is very easily possible => _packet sniffing_ 
_Packet injection_ by malicious person is very easily possible. 
_Denial of service_ its hard for someone to figure out what's the sequence number used by the server however attacker can fool
server to cloe  a long lived connection with a client. Servers accept a longer range of messages which indicate TCP connection 
needs to be reset. So the attacker just needs to send multiple packets with random number generator with packet 
type => RESET and can potentially close a valid connection for the client.  

### Routing Security
__Autonomous systems__ : connected group of one or more Internet Protocol prefixes under a single routing policy (aka domain)
__ARP__ : Address resolution protocol . 
__OSPF__: used for routing within AS 
__BGP__: routing between AS 

