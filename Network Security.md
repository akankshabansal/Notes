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
__ARP__ : Address resolution protocol. Converts IP -> Ethernet address. Node A can confuse gateway into sending it traffic for Node B. By proxying traffic, node A can read/inject packets into B’s session (e.g. WiFi networks)
* __RPKI__ : AS obtains a cert. for its IP prefixes, from regional authority. Attaches ROA to all path advertisements. Advertisements without a valid ROA are ignored. Defends against a misbehaving AS (but not a network attacker). 
* __SBGP__ : Sign every hop of a path advertisement - Not used much. 

__OSPF__: used for routing within AS 
* Link State Advertisements (LSA): Flooded throughout AS so that all routers in the AS have a complete view of  topology. Links must be advertised by both ends
* Neighbor discovery : Routers dynamically discover direct neighbors on attached links ⇒ sets up an “adjacenty”
* OSPF message integrity (unlike BGP):  Every link can have its own shared secret (though its found to be using unsecure crupto tool). And makes use of same key for every msg. 
* If a single malicious router, valid LSAs may still reach dest. The“fight back” mechanism: if a router receives its own LSA with a newer timestamp than the latest it sent, it immediately floods a new LSA

__BGP__: routing between AS.  Security issues: unauthenticated route updates. Anyone can re‐route all traffic for a victim IP to attacker’s network. Anyone can hijack route to victim (next slides)


### DNS Domain Name System
Hierarchical service to determine the IP for a given address
* Root name servers for top‐level domain
* Authoritative name servers for subdomains (NS)
* Local name resolvers contact authoritative servers when they do not know a name

DNS record types (partial list):
- NS : name server (points to other server)
- A  : address record (contains IP address)
- MX : address in charge of handling email
- TXT: generic text (e.g. used to distribute site public keys (DKIM)) 

* DNS responses are cached for quick response 
* Also speeds up finding NS records for domains 
* DNS negative queries are cached for 24 hours. (Save time for nonexistent sites, e.g. common misspelling)
* Cached data periodically times out
* Lifetime (TTL) of data controlled by owner of data
* TTL passed with every record

Query Id : 16 bit random value, saved in DNS query. usually DNS queries are run over UDP. 

* Easdropping is possible 
* Packet injection is possible 
* Route Stealing 

### Network Security  

#### Link Layer : iProtocol 
Focused on trying to make wireless communication as secure as a wire connection. 
* __Supplicant__ => end user device. 
* __Authenticator__ => wireless access point service/device being accessed by the user. 
* __Authentication server__ : some server/endpoint controlling access 

In this step you want to enable communication, without revealing much about each.. 
__802.11__ association : No data encryption of security available at this point. 
__802.1X/RADIUS__ : authenticator using authenticating server determine if the the supplicant is allowed to use the service. 
__4 way handshake__ : setsup a key which ensures that the all data/communication between device and router is secure and no one else will be able to decrypt it. (WEP is not secure at this....)

#### TCP security
Need to isolate conversation from hosts which might be malicious which are routing data. TCP state is easy to guess so we would want to make sure attackers are not able to decrypt. 

### IPSec 
Replace IP header with IP sec headers this header can be encrypted. 

IPSec tunnel : Want to give same guaratees as if the machine was connected to same network. In tunnel mode, the entire IP packet is encrypted and authenticated. It is then encapsulated into a new IP packet with a new IP header. Tunnel mode is used to create virtual private networks for network-to-network communications (e.g. between routers to link sites), host-to-network communications (e.g. remote user access) and host-to-host communications (e.g. private chat).

Provides : Mobility Preserve network connections when a device moves to different physical portions of the network. 


### Firewall 
Packet filter (stateless, stateful), Application layer proxies
statefull : makes use of the TCP connection details to check if the data coming in correpomding to what was requested.. 
From port numbering : firewall can know about protocol. 
Telnet : port no 23. Firewall can match in this case : source and destination.. 
FTP : port 20 and 21 : one data connection and one communication. Though firewall has to follow through the communication to determine when the data transfer starts.. 

Packet Fragmentation attack : 
Different routers have different payloads therefore packets need to be broken multiple times. However maliciously one can break packets to fool the firewalls. 

Aplpication Proxy Firewalls : 
A machine which acts as a firewall, however it has the understansding of the underlying application for which the packet is meant. it will uses its undersatnding to determine if the packet contains any viruses etc. 
1. This mahine is highly vulnerable for attacks, so its imp that it only has least min-needed logic to run the application proxy code. 
2. This layer will slow down the packet delivery to the user, so its only useful for cases like Emails (SMTP), where its okay for the packets delivery to be slow. 


### Intrusion detection
Anomaly and misuse detection
two types : Network‐based, host‐based, or combination(most effective though most difficult)

* Maintain data on known attacks
* Look for activity with corresponding signatures
* Try to figure out what is “normal”› Report anomalous behavior (If its too strict : too many false alarms) 
** detect anomalies in packet headers
** packet defragmentation
** decode HTTP URI
** applies rules to packets
