# Module 03: Advanced Networking Analysis and Troubleshooting (The Other 5%)

## Overview

This module covers deep networking diagnostics beyond basic `ping`, `ip`, and `curl`. These are the tools used when:

- A service is reachable but slow  
- Packets are being dropped  
- DNS behaves inconsistently  
- Traffic is blocked at a firewall or routing layer  
- TLS/SSL issues occur  
- You need packet-level inspection  

This module gives you the advanced networking skills expected from an experienced cloud engineer or Linux administrator.

---

## Learning Objectives

By the end of this module, you should be able to:

- Analyze packets using `tcpdump`  
- Inspect connections, states, and queues with `ss`  
- Debug routing problems using `ip route`, `tracepath`, and `mtr`  
- Debug DNS issues using `dig` and related flags  
- Diagnose TLS/SSL problems using `openssl`  
- Identify MTU and fragmentation issues  
- Use `/proc/net` for low-level network inspection  

---

## Lesson 1: ss — Socket Statistics (Advanced Usage)

Basic usage was covered in the 95% course. Now we go deeper.

List all listening sockets:
    ss -tuln

Show processes with sockets:
    ss -tulnp

Show TCP connection states:
    ss -s

Show all established connections:
    ss -tan state established

Show connections in TIME_WAIT:
    ss -tan state time-wait

Find which process is using port 443:
    ss -tulnp | grep 443

`ss` is an essential replacement for the older `netstat`.

---

## Lesson 2: tcpdump — Packet Capture and Inspection

Capture all traffic on interface eth0:
    sudo tcpdump -i eth0

Capture only TCP port 80:
    sudo tcpdump -i eth0 tcp port 80

Capture only traffic to/from a single host:
    sudo tcpdump host 10.0.0.5

Write capture to a file:
    sudo tcpdump -w capture.pcap

Read a saved capture:
    tcpdump -r capture.pcap

Show DNS traffic only:
    sudo tcpdump -i eth0 port 53

Show TLS handshake packets:
    sudo tcpdump -i eth0 port 443

Use this tool when:

- Traffic is reaching a server but responses are missing  
- You suspect firewalls dropping packets  
- DNS queries look inconsistent  
- TLS negotiations fail  

---

## Lesson 3: DNS Deep-Dive with dig

Query A record:
    dig example.com

Query all records:
    dig example.com ANY

Query specific nameserver:
    dig @8.8.8.8 example.com

Show only answer section:
    dig example.com +short

Trace DNS resolution:
    dig example.com +trace

Query MX records:
    dig example.com MX

Query TXT records:
    dig example.com TXT

This is vital when DNS behaves differently across environments.

---

## Lesson 4: Routing Diagnostics

Show current routing table:
    ip route

Trace hop-by-hop path:
    tracepath 8.8.8.8

More powerful alternative:
    mtr 8.8.8.8

Show routes for a specific table (advanced networking setups):
    ip route show table main

View ARP table:
    ip neigh

Detect routing loops using mtr:
    mtr --report 8.8.8.8

Routing problems often manifest as:

- “Works from one VM but not another”  
- “Only certain subnets fail”  
- “Traffic returns through wrong interface”  

---

## Lesson 5: Firewall and Netfilter Inspection

Show iptables rules:
    sudo iptables -L -n -v

Show nftables rules:
    sudo nft list ruleset

List connection tracking entries:
    sudo conntrack -L

Find what is dropping packets:
    sudo iptables -L -v -n | grep DROP

Use netfilter tools when:

- Packets disappear silently  
- NAT rules behave unexpectedly  
- Stateful connections behave inconsistently  

---

## Lesson 6: TLS/SSL Debugging

Test SSL certificate negotiation:
    openssl s_client -connect example.com:443

Check only certificate:
    echo | openssl s_client -connect example.com:443 2>/dev/null | openssl x509 -noout -dates -issuer -subject

Download certificate:
    echo | openssl s_client -connect example.com:443 | openssl x509 > cert.pem

Check expiration:
    openssl x509 -in cert.pem -noout -dates

Use TLS debugging when:

- API calls fail with SSL errors  
- Browsers work but CLI requests fail  
- Load balancers use outdated certs  
- Services break after certificate rotation  

---

## Lesson 7: MTU and Fragmentation Debugging

Check interface MTU:
    ip link show eth0

Test MTU path (do not fragment flag):
    ping -M do -s 1472 example.com

If packets drop:

- Lower MTU  
- Diagnose intermediate network devices  

You encounter MTU issues when:

- Services work for small payloads but fail for large ones  
- VPN tunnels are in use  
- Jumbo frames are misconfigured  

---

## Lesson 8: Advanced Network Information from /proc

Network connections:
    cat /proc/net/tcp

UDP connections:
    cat /proc/net/udp

ARP table:
    cat /proc/net/arp

Network interfaces:
    cat /proc/net/dev

Softnet stats (packet drops at kernel level):
    cat /proc/net/softnet_stat

Use `/proc/net` when tools like `ss` and `netstat` fail or when working inside containers with limited binaries.

---

## Lesson 9: Putting It All Together — Real Scenarios

### Scenario 1: DNS works on one server but not another

1. Compare resolv.conf:
       cat /etc/resolv.conf

2. Test DNS:
       dig example.com
       dig @8.8.8.8 example.com +short

3. Trace DNS resolution:
       dig example.com +trace

4. Inspect outbound port 53:
       ss -tulnp | grep 53

---

### Scenario 2: Intermittent packet loss

1. Ping with large packets:
       ping -s 1500 example.com

2. Use tracepath:
       tracepath example.com

3. Use mtr:
       mtr example.com

4. Use tcpdump:
       sudo tcpdump -i eth0 icmp

---

### Scenario 3: TLS handshake failure

1. Inspect certificate:
       openssl s_client -connect api.example.com:443

2. Check expiration:
       echo | openssl s_client -connect api.example.com:443 | openssl x509 -noout -dates

3. Check ciphers:
       openssl ciphers -v

---

## Lab Exercise

1. Identify which processes are listening on ports on your VM:
       ss -tulnp

2. Capture 10 seconds of DNS traffic:
       sudo tcpdump -i eth0 port 53 -w dns.pcap

3. Use dig to check TXT records:
       dig example.com TXT

4. Use mtr to diagnose path issues:
       mtr --report google.com

5. Inspect certificate for github.com:
       echo | openssl s_client -connect github.com:443 | openssl x509 -noout -dates -subject

---

## Quiz

1. What tool captures packets at a low level?  
2. How do you trace DNS resolution end-to-end?  
3. Which command shows listening sockets with process IDs?  
4. How do you test MTU without fragmentation?  
5. Which tool diagnoses routing loops more effectively than traceroute?  

---

## Notes and Observations

Use this section to record:

- Strange packet captures  
- DNS inconsistencies observed  
- TLS failures and root causes  
- Any surprising TIME_WAIT behavior  
- Topics requiring deeper study
