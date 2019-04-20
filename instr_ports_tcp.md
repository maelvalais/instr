# How is a TCP connection uniquely identified

A TCP connection is identified by four things:

- source port
- source ip
- dest port
- dest ip

This is why there is no confusion between two requests to a web server on
80 port.

## Stateful vs. Stateless Firewall

This only applies to appliances in between two computers, for example
a router using some kind of firewall. Is it???? Is it only for NATs or
gateways?

    A -----> Router -----> B

If the router manages connection in a stateful way, here is what is going
to happen:

1. A connects to B via a TCP connection identified by IP_A:PORT_A + IP_B:PORT_B
2. Router remembers the source port of A so that when B sends things back,
   it can

References:

- Using linux iptables to implement a stateless packet filtering firewall:
  <https://security.stackexchange.com/questions/74529>

Example:

```iptables
-A INPUT -p tcp -s 192.168.1.0/24 -m tcp --dport 80 -j ACCEPT
```

- `-A CHAIN RULE` tells iptables to append this RULE at the end of this CHAIN.
  A chain is identified by a name such as INPUT or OUTPUT.
- `-p TCP` means protocol
- `-s` is the source. Here, 192.168.1.0/24 means any IP adress that matches
  with `192.*.*.*`
- `-m TCP` is for extensions (see `man iptables-extensions`) but I didn't
  find it...
- `--dport 80` means the destination port must be 80
- `-j ACCEPT` means that the target is the action "ACCEPT"

QUESTIONS: what is the link between protocol and state?

## SG vs. ACLs

Security Groups = stateful firewall at the VM level (= iptables)
Access Control Lists = stateless firewall at the VPC level.

## Protocol stack

The protocol stack is the piece of software that implements (among others)
the TCP/IP layers. A socket is the internal representation of an instance
of this TCP stack. A socket is identified with a number (file descriptor in
Unix terms).

TCP connection = 2 sockets = from(ip:port) + to(ip:port)
TCP socket = internal representation of the 'from' (ip:port)

## The stack

|   OSI   | Lv | TCP/IP |   Keywords   |          |
|---------|----|--------|--------------|----------|
| Applica | L7 | HTTPS  | LB           | Data     |
| Present |    | HTTPS  |              |          |
| Session |    | HTTPS  |              |          |
| Transpo | L4 | TCP    | Firewall     | Segments |
| Network | L3 | IP     | Router       | Packets  |
| Link    | L2 | MAC    | VLAN, Switch | Frames   |
| Physic  | L1 |        |              | Bits     |

**VLAN** = just like the good'old ethernet between multiple equipments.
Switches aliviates some of the issues with the one-cable-for-multiple-pc
with a table (forwarding table, NOT an ARP table) that allows a one-to-one
link with less noise on each link overall.

**TCP** = handles loss of packets using ACKs and SYN with sequence numbers.
SYN = please open a connection. ACK = acknowledge.

Segments vs packets??