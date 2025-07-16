# Linux Networking Essentials for DevOps

A deep dive into the essential command-line tools for network configuration, troubleshooting, and analysis on Linux systems.

### Table of Contents
1.  [Inspecting Network Interfaces & IPs](#1-inspecting-network-interfaces--ips)
2.  [Querying Sockets & Connections](#2-querying-sockets--connections)
3.  [Testing Connectivity](#3-testing-connectivity)
4.  [DNS Resolution](#4-dns-resolution)
5.  [Making Web Requests](#5-making-web-requests)

---

## 1. Inspecting Network Interfaces & IPs

These commands help you understand how your server is connected to the network.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `ip` | The modern, all-in-one tool for network configuration. | `ip addr show` (or `ip a`): Display IP addresses and properties of all network interfaces. <br> `ip route show`: Show the kernel's routing table. <br> `ip link set eth0 down`: Bring an interface down. |
| `ifconfig` | The legacy tool for network interface configuration. | `ifconfig`: Still useful for a quick overview, but `ip` is preferred for scripting and modern systems. |

> **Pro-Tip:** Use `ip -c a` for colored output, making it easier to read.

---

## 2. Querying Sockets & Connections

Find out which services are listening on which ports and who is connected.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `ss` | The modern tool for investigating sockets (network connections). It's faster and simpler than `netstat`. | `sudo ss -tulnp`: The go-to command. Shows **t**cp, **u**dp, **l**istening, **n**umeric ports, and the **p**rocess using the socket. <br> `ss -tp state established`: Show all established TCP connections with process info. |
| `netstat` | The legacy tool for network connections and stats. | `sudo netstat -tulnp`: Provides similar output to `ss`. Still found on many systems. |
| `lsof` | **L**ist **O**pen **F**iles. Can find the process using a specific port. | `sudo lsof -i :443`: Shows which process is listening on TCP port 443. |

---

## 3. Testing Connectivity

Check if your server can reach other hosts and trace the path it takes.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `ping` | Sends ICMP ECHO_REQUEST packets to a host to test basic reachability and latency. | `ping -c 5 8.8.8.8`: Sends **5** packets to Google's DNS server and then stops. |
| `traceroute` | Traces the network path (hops) to a destination host. | `traceroute google.com`: Shows the routers your packets pass through to reach the destination. Useful for diagnosing network bottlenecks. |
| `mtr` | **M**y **Tr**aceroute. Combines `ping` and `traceroute` into a single, real-time diagnostic tool. | `mtr google.com`: Provides a live, updating view of the latency to every hop. (May require `apt install mtr`). |

---

## 4. DNS Resolution

Tools for querying the Domain Name System (DNS) to resolve names to IPs and vice-versa.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `dig` | **D**omain **I**nformation **G**roper. The standard, detailed tool for DNS queries. | `dig A google.com`: Get the `A` (address) record. <br> `dig MX google.com`: Get the `MX` (mail exchange) records. <br> `dig @8.8.8.8 google.com`: Query a specific DNS server. |
| `host` | A simpler, friendlier tool for quick DNS lookups. | `host google.com` |
| `nslookup` | An older, interactive tool for DNS queries. | `nslookup google.com` |

---

## 5. Making Web Requests

Tools for interacting with web servers and APIs from the command line.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `curl` | **cURL**. A powerful and versatile tool for transferring data with URLs. The swiss-army knife for APIs. | `curl https://api.github.com/users/torvalds`: GET request to a JSON API. <br> `curl -I https://google.com`: Fetch **H**eaders only. <br> `curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' https://httpbin.org/post`: Send a POST request with JSON data. |
| `wget` | A non-interactive network downloader. Excellent for downloading files in scripts. | `wget -c https://releases.ubuntu.com/22.04/ubuntu-22.04.3-desktop-amd64.iso`: Download a large file, with the ability to **c**ontinue if interrupted. |
