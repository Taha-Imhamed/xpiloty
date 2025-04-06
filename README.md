IP Spoofing


1. Sniff and inject network traffic using raw sockets
2. Bypass IP address-based network authentication
IP Address Authentication:
IP addresses have been widely used as network principals for authentication. It was common in the early years of UNIX to control access to network services by adding IP addresses to an allow list. (For example, see man hosts.allow.) IP addresses still see wide use as network principals in firewalls and malware block lists. However, IP addresses are in many contexts not a reliable principal for several reasons, including churn, weak binding to identity, and – not least – the ability to forge packets with arbitrary source addresses using raw sockets.

Service Protocol:
This program exploits a vulnerable network service using IP address authentication. The vulnerable service implements a simple TCP protocol on port 10010 as follows:

C -> S : u16(|identifier|)||identifier
S -> C : u8(authorized)||(u16(|secret|)||secret||u16(|nonce|)||nonce)

The identifier will be unique to the user encoded as a UTF-8 string, secret and nonce are UTF-8 encoded strings, |x| is the length of a byte array x, u8(.) is a single byte. u16(.) encodes an integer x as two big-endian bytes, and || is concatenation.

After the client sends its ID, the server will check whether the sending IP address is authorized to use the service. If so, it will return a non-zero value for authorized as well as a secret and nonce. Otherwise, it will return authorized = 0 and no secret. 

