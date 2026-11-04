# AWS Student Builders Group — Week 2 Lab
## Networking & IAM Fundamentals

**Name:** Barkot Zerihun 
**Track:** Cloud & DevOps  

---

## Objective

The objective of this lab is to practice fundamental networking and AWS IAM concepts, including IP addressing, subnetting, DNS resolution, HTTPS inspection, HTTP status codes, and least-privilege IAM policies.

---

## Task 1 — Identify Your Network Information

**Operating System:** Windows  
**Active Network Adapter:** Wi-Fi

**Command used:**

```cmd
ipconfig
```

### Results

| Requirement | Result |
|---|---|
| Operating System | Windows |
| Active Network Adapter | Wi-Fi |
| Local IPv4 Address | Private IPv4 address (withheld for privacy) |
| Subnet Mask | `255.255.255.0` (`/24`) |
| Default Gateway | Local network gateway (withheld for privacy) |
| IPv4 Classification | Private |

### Explanation

The Wi-Fi adapter is the active network interface used for the current network connection.

Its IPv4 address belongs to the private `10.0.0.0/8` address range. Private IP addresses are used within local networks and are not directly routable over the public Internet.

The default gateway provides a route from the local network to other networks.

Other adapters shown in the `ipconfig` output are associated with virtual networking, loopback capture, or disconnected network interfaces.

**Privacy note:** Exact local network identifiers and MAC addresses have been omitted from this public report.

---

## Task 2 — Practice Subnetting

**Starting Network:** `10.0.0.0/24`

The objective is to divide the starting network into four equal-sized subnets.

### Calculations

**Step 1: Determine the number of subnet bits required.**

We need four subnets.

\[
2^n = 4
\]

\[
n = 2
\]

Therefore, we need 2 additional subnet bits.

**Step 2: Calculate the new subnet prefix.**

The original prefix is `/24`.

\[
24 + 2 = 26
\]

The new subnet prefix is `/26`.

**Step 3: Calculate the number of host bits.**

\[
32 - 26 = 6
\]

There are 6 host bits remaining.

**Step 4: Calculate the total addresses per subnet.**

\[
2^6 = 64
\]

Each subnet contains 64 IPv4 addresses.

**Step 5: Calculate conventional usable addresses.**

Conventionally, the network and broadcast addresses are reserved.

\[
64 - 2 = 62
\]

Each subnet has 62 conventional usable host addresses.

**Step 6: Calculate AWS usable IPv4 addresses.**

Under standard AWS subnet rules, AWS reserves five IPv4 addresses in every subnet.

\[
64 - 5 = 59
\]

Each subnet therefore has 59 usable IPv4 addresses under standard AWS rules.

### Subnet Results

| Subnet CIDR | Full Address Range | Conventional Usable Host Range | Broadcast Address | AWS Usable Addresses |
|---|---|---|---|---:|
| `10.0.0.0/26` | `10.0.0.0 – 10.0.0.63` | `10.0.0.1 – 10.0.0.62` | `10.0.0.63` | 59 |
| `10.0.0.64/26` | `10.0.0.64 – 10.0.0.127` | `10.0.0.65 – 10.0.0.126` | `10.0.0.127` | 59 |
| `10.0.0.128/26` | `10.0.0.128 – 10.0.0.191` | `10.0.0.129 – 10.0.0.190` | `10.0.0.191` | 59 |
| `10.0.0.192/26` | `10.0.0.192 – 10.0.0.255` | `10.0.0.193 – 10.0.0.254` | `10.0.0.255` | 59 |

### Explanation

The `/24` network was divided into four equal-sized `/26` subnets by borrowing two bits from the host portion of the address.

Each subnet contains 64 total addresses. The network and broadcast addresses are excluded from conventional host allocation.

AWS reserves five IPv4 addresses per subnet instead of the conventional two, leaving 59 usable IPv4 addresses in each subnet.

---

## Task 3 — Test DNS Resolution

**Domain Tested:** `example.com`

**Command used:**

```cmd
nslookup example.com
```

### Command Output

```text
Server:  UnKnown
Address:  192.168.100.1

Non-authoritative answer:
Name:    example.com
Addresses:  2606:4700:10::ac42:93f3
           2606:4700:10::6814:179a
           172.66.147.243
           104.20.23.154
```

### Results

- **Domain tested:** `example.com`
- **DNS resolution:** Successful
- **DNS server address:** `192.168.100.1`

**Returned IPv4 addresses:**

- `172.66.147.243`
- `104.20.23.154`

**Returned IPv6 addresses:**

- `2606:4700:10::ac42:93f3`
- `2606:4700:10::6814:179a`

### Explanation

DNS (Domain Name System) translates human-readable domain names into IP addresses that computers use to locate network destinations.

The DNS lookup was successful because the DNS server returned IP addresses for `example.com`.

The response was non-authoritative, meaning the responding DNS server was not the authoritative source for the domain's DNS records.

The DNS server was displayed as `Unknown` because Windows could not identify a hostname associated with its IP address. This does not indicate a DNS failure.

DNS returned both IPv4 and IPv6 addresses, allowing clients
