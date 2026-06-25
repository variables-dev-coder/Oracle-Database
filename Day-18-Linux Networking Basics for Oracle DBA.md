# Day 18 – Linux Networking Basics for Oracle DBA

## 🎯 Objective

Learn the essential Linux networking commands every Oracle DBA should know:

- `ping`
- `ip addr`
- `ss`

These commands help troubleshoot database connectivity, network configuration, and Oracle Listener issues.

---

# Why Networking is Important for an Oracle DBA

Oracle databases communicate over a network.

Example:

Application → Oracle Listener → Oracle Database

If the network is down, users cannot connect to the database.

As a DBA, networking is one of the first things to check when troubleshooting connection problems.

---

# 1. ping Command

## What is ping?

`ping` checks whether another computer or server is reachable over the network.

It sends ICMP (Internet Control Message Protocol) packets to the destination and waits for a reply.

---

## Syntax

```bash
ping hostname
```

or

```bash
ping IP_ADDRESS
```

Example:

```bash
ping google.com
```

Output:

```text
PING google.com (142.250.xxx.xxx)

64 bytes from ...
64 bytes from ...
```

Every reply means the destination is reachable.

---

## Ping a Fixed Number of Times

```bash
ping -c 4 google.com
```

Example Output:

```text
4 packets transmitted
4 received
0% packet loss
```

---

## Ping an IP Address

```bash
ping 8.8.8.8
```

Used to verify internet connectivity.

---

## Ping Local Machine

```bash
ping localhost
```

or

```bash
ping 127.0.0.1
```

Used to verify the local network stack.

---

## Stop ping

Press

```text
Ctrl + C
```

---

## Important Output Fields

Example:

```text
64 bytes from ...
time=12 ms
ttl=115
```

### time

Response time.

Lower values indicate a faster network.

Example:

```text
time=12 ms
```

---

### ttl (Time To Live)

Limits how many network devices (routers) a packet can pass through.

Prevents packets from looping forever.

---

## Common Reasons for Ping Failure

- Internet disconnected
- Wrong IP address
- Firewall blocking ICMP
- Destination server is down

---

## DBA Use Case

Application cannot connect to Oracle Database.

First check:

```bash
ping dbserver
```

If ping fails, the issue is likely network-related rather than an Oracle problem.

---

# 2. ip addr Command

## What is ip addr?

Displays all network interfaces and their assigned IP addresses.

Think of it as:

> "Show my network configuration."

---

## Syntax

```bash
ip addr
```

Shortcut:

```bash
ip a
```

---

## Example

```bash
ip addr
```

Output:

```text
1: lo
2: eth0
3: wlan0
```

---

## Loopback Interface

```text
lo
```

IP Address:

```text
127.0.0.1
```

Used for communication within the same machine.

---

## Ethernet Interface

```text
eth0
```

Represents a wired network connection.

---

## Wireless Interface

```text
wlan0
```

Represents a Wi-Fi connection.

---

## Find Your IP Address

Look for:

```text
inet
```

Example:

```text
inet 192.168.1.105/24
```

Your IPv4 address:

```text
192.168.1.105
```

---

## Common Private IP Ranges

```text
10.x.x.x

172.16.x.x – 172.31.x.x

192.168.x.x
```

These are used inside local networks.

---

## DBA Use Case

A developer cannot connect to the database.

Check the server IP:

```bash
ip addr
```

Verify that the expected IP address is configured.

---

# 3. ss Command

## What is ss?

`ss` (Socket Statistics) displays:

- Active network connections
- Listening ports
- TCP connections
- UDP connections

It is the modern replacement for `netstat`.

---

## Syntax

```bash
ss
```

---

## Show Listening Ports

```bash
ss -l
```

Displays services waiting for incoming connections.

---

## Show TCP Connections

```bash
ss -t
```

---

## Show UDP Connections

```bash
ss -u
```

---

## Show Listening TCP Ports

```bash
ss -lt
```

---

## Show Listening TCP & UDP Ports

```bash
ss -tul
```

Example Output:

```text
tcp LISTEN

0.0.0.0:22

0.0.0.0:1521
```

---

## Show Listening Ports with Process Names

```bash
sudo ss -tulpn
```

Example:

```text
LISTEN

*:1521

users:(("tnslsnr"))
```

This indicates the Oracle Listener process owns port **1521**.

---

# Important Ports for Oracle DBA

| Port | Service |
|------|---------|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 1521 | Oracle Listener |
| 5500 | Oracle Enterprise Manager Express |

---

# Oracle Connectivity Flow

```text
Application
      │
      ▼
Network
      │
      ▼
Oracle Listener (1521)
      │
      ▼
Oracle Database
```

---

# Oracle DBA Troubleshooting Example

Problem:

```
Application cannot connect to Oracle Database.
```

Step 1

```bash
ping dbserver
```

If successful:

↓

Step 2

```bash
ss -tul
```

Check whether port **1521** is listening.

If port 1521 is missing:

- Oracle Listener is stopped.
- Start the listener.

---

# Practice Exercises

## Exercise 1

Check internet connectivity.

```bash
ping -c 4 google.com
```

---

## Exercise 2

Ping your own machine.

```bash
ping -c 4 localhost
```

---

## Exercise 3

Display all network interfaces.

```bash
ip addr
```

Find:

- Loopback interface
- Active network interface
- IPv4 address

---

## Exercise 4

Show listening ports.

```bash
ss -tul
```

---

## Exercise 5

Show listening ports with process names.

```bash
sudo ss -tulpn
```

Identify:

- SSH (22)
- Oracle Listener (1521)
- Any web services

---

# Interview Questions

### Q1. What does the `ping` command do?

It checks whether another host is reachable over the network and measures the response time.

---

### Q2. What is the purpose of `ip addr`?

It displays network interfaces and their assigned IP addresses.

---

### Q3. What is the difference between `ping` and `ip addr`?

- `ping` tests network connectivity.
- `ip addr` displays the local network configuration.

---

### Q4. What does the `ss` command do?

It displays active network connections and listening ports.

---

### Q5. Which port is used by the Oracle Listener?

Default Oracle Listener port:

```text
1521
```

---

# Command Summary

| Command | Purpose |
|----------|----------|
| `ping google.com` | Test network connectivity |
| `ping -c 4 google.com` | Send only 4 ping packets |
| `ping localhost` | Test local network stack |
| `ip addr` | Show IP addresses and interfaces |
| `ip a` | Shortcut for `ip addr` |
| `ss` | Display socket statistics |
| `ss -l` | Show listening ports |
| `ss -t` | Show TCP connections |
| `ss -u` | Show UDP connections |
| `ss -lt` | Show listening TCP ports |
| `ss -tul` | Show listening TCP & UDP ports |
| `sudo ss -tulpn` | Show listening ports with process names |

---

# Key Takeaways

- `ping` verifies whether a host is reachable.
- `ip addr` displays network interfaces and IP addresses.
- `ss` shows active connections and listening ports.
- Oracle Listener typically listens on **port 1521**.
- A common Oracle DBA troubleshooting sequence is:
  1. Ping the database server.
  2. Verify the server IP.
  3. Check whether port **1521** is listening.
  4. If not, restart the Oracle Listener.
