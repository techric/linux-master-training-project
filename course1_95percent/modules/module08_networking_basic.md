# Module 08: Basic Networking and Connectivity in Linux

## Overview

This module covers the essential networking commands used to check connectivity, inspect interfaces, test DNS, verify routing, and troubleshoot network-related problems. As a cloud engineer, you will frequently investigate connectivity failures between servers, containers, load balancers, and external endpoints.

Networking fundamentals are required for nearly all cloud troubleshooting.

## Learning Objectives

By the end of this module, you should be able to:

- View network interfaces and IP addresses  
- Test connectivity using `ping`, `curl`, and `wget`  
- Inspect routing tables  
- View open network connections and listening ports  
- Test DNS resolution  
- Identify and troubleshoot common networking problems  

---

## Lesson 1: Viewing Network Interfaces

To list all network interfaces and their IP addresses:

    ip a

This shows:

- Interface names (eth0, ens5, lo)  
- IP addresses  
- MAC addresses  
- Interface state (up/down)  

To show only IPv4 addresses:

    ip -4 a

To show only IPv6:

    ip -6 a

---

## Lesson 2: Viewing Routing Information

To view the routing table:

    ip route

This shows:

- Default gateway  
- Network routes  
- Interface assignments  

Example output:

    default via 172.31.1.1 dev eth0
    172.31.0.0/16 dev eth0 proto kernel

---

## Lesson 3: Testing Connectivity with ping

Test whether a host is reachable:

    ping -c 4 google.com

`-c 4` sends 4 packets.

Test an IP directly:

    ping -c 4 8.8.8.8

Useful for determining whether DNS or network is failing.

---

## Lesson 4: Testing Connectivity with curl and wget

### Check HTTP/HTTPS endpoints

    curl http://example.com
    curl -I https://example.com

`-I` fetches headers only.

### Test API endpoints

    curl -v https://api.example.com/status

Verbose output (`-v`) reveals SSL issues, redirects, and handshake problems.

### Using wget (if installed)

    wget http://example.com

Shows more download progress information.

---

## Lesson 5: DNS Troubleshooting (dig and nslookup)

Check DNS records:

    dig google.com

Check specific record types:

    dig A google.com
    dig MX google.com
    dig TXT google.com

Check a specific DNS server:

    dig @8.8.8.8 google.com

Using `nslookup`:

    nslookup google.com

---

## Lesson 6: Viewing Open Ports and Listening Services

To see open ports and the processes using them:

    sudo ss -tulnp

Flags:

- `t` = TCP  
- `u` = UDP  
- `l` = listening  
- `n` = numeric output  
- `p` = process names  

Example output:

    LISTEN 0 128 0.0.0.0:22 sshd

This is extremely important for verifying whether services like SSH, Nginx, or custom applications are listening.

---

## Lesson 7: Checking Hostname and DNS Configuration

View hostname:

    hostname

Set hostname:

    sudo hostnamectl set-hostname newname

View DNS configuration:

    cat /etc/resolv.conf

This file typically contains nameserver entries.

---

## Lesson 8: Common Networking Issues and Fixes

### You can ping IPs but not domain names  
DNS failure.

Check:

    cat /etc/resolv.conf

### You cannot ping at all  
Firewall, routing, or network ACL failure.

Check:

    ip a
    ip route
    sudo ss -tulnp

### Service is running but unreachable  
Check its port:

    sudo ss -tulnp | grep 8080

Check firewall rules:

    sudo iptables -L -n

(Or cloud firewall rules / security groups)

### DNS resolves but connect fails  
Application might be offline, blocked, or misconfigured.

Test with:

    curl -v http://server:port

---

## Lab Exercise

1. View all network interfaces:

       ip a

2. View your IPv4 routing table:

       ip route

3. Ping a public DNS server:

       ping -c 4 8.8.8.8

4. Resolve a domain using `dig`:

       dig example.com

5. Check open ports:

       sudo ss -tulnp

6. Test an HTTP endpoint:

       curl -I https://www.google.com

7. View DNS configuration:

       cat /etc/resolv.conf

Record connectivity issues or unexpected output.

---

## Quiz

1. What command shows all network interfaces?  
2. What does `ip route` display?  
3. How can you determine whether DNS is failing vs. network connectivity?  
4. What command shows open listening ports?  
5. How do you test an HTTP/HTTPS endpoint?  

---

## Notes and Personal Observations

Use this area to record:

- Network issues you encountered  
- Commands that helped solve problems  
- Differences between tools like `ping`, `curl`, and `ss`  
