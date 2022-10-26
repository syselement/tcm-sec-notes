# Networking at the Command Line

- Focus on some network utilities used for troubleshooting, testing and diagnosing network connectivity issues.

## Commands

### ping

- **`ping`** - send one ore more ICMP ECHO_REQUEST packages to network hosts.
  - it is part of the `iputils` package

```bash
ping google.com
    PING google.com (142.251.209.14) 56(84) bytes of data.
    64 bytes from mil04s50-in-f14.1e100.net (142.251.209.14): icmp_seq=1 ttl=128 time=37.6 ms
    64 bytes from mil04s50-in-f14.1e100.net (142.251.209.14): icmp_seq=2 ttl=128 time=33.2 ms
    64 bytes from mil04s50-in-f14.1e100.net (142.251.209.14): icmp_seq=3 ttl=128 time=32.9 ms
    64 bytes from mil04s50-in-f14.1e100.net (142.251.209.14): icmp_seq=4 ttl=128 time=33.6 ms
	# It continuee the ping until stop (Ctrl+C)

# Ping a certain number of times
ping -c 2 google.com
    PING google.com (142.251.209.46) 56(84) bytes of data.
    64 bytes from mil04s51-in-f14.1e100.net (142.251.209.46): icmp_seq=1 ttl=128 time=34.5 ms
    64 bytes from mil04s51-in-f14.1e100.net (142.251.209.46): icmp_seq=2 ttl=128 time=34.7 ms

    --- google.com ping statistics ---
    2 packets transmitted, 2 received, 0% packet loss, time 1002ms
    rtt min/avg/max/mdev = 34.493/34.606/34.719/0.113 ms
```

### ifconfig

- **`ifconfig`** - configure a network interface
  - it is part of the `net-tools` package


```bash
sudo apt install net-tools -y

ifconfig
    ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 192.168.31.133  netmask 255.255.255.0  broadcast 192.168.31.255
            inet6 fe80::4d26:4984:fcf7:6061  prefixlen 64  scopeid 0x20<link>
            ether 00:0c:29:c9:89:de  txqueuelen 1000  (Ethernet)
            RX packets 37168  bytes 54380681 (54.3 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 20049  bytes 1330857 (1.3 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
            inet 127.0.0.1  netmask 255.0.0.0
            inet6 ::1  prefixlen 128  scopeid 0x10<host>
            loop  txqueuelen 1000  (Local Loopback)
            RX packets 657  bytes 70806 (70.8 KB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 657  bytes 70806 (70.8 KB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
# Two network interfaces listed: ens33 (lan interfaces) and lo (loop-back interface)

iwconfig
# for wireless adapters
```

### ip

- **`ip`** - powerful tool for configuring network interfaces

```bash
ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host 
           valid_lft forever preferred_lft forever
    2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 00:0c:29:c9:89:de brd ff:ff:ff:ff:ff:ff
        altname enp2s1
        inet 192.168.31.133/24 brd 192.168.31.255 scope global dynamic noprefixroute ens33
           valid_lft 1186sec preferred_lft 1186sec
        inet6 fe80::4d26:4984:fcf7:6061/64 scope link noprefixroute 
           valid_lft forever preferred_lft forever

# Print statistics
ip -s link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    RX:  bytes packets errors dropped  missed   mcast           
         71259     661      0       0       0       0 
    TX:  bytes packets errors dropped carrier collsns           
         71259     661      0       0       0       0 
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 00:0c:29:c9:89:de brd ff:ff:ff:ff:ff:ff
    RX:  bytes packets errors dropped  missed   mcast           
     219198302  146066      0       0       0       0 
    TX:  bytes packets errors dropped carrier collsns           
       4575119   74085      0       0       0       0 
    altname enp2s1

# Print basic info in color
ip -br -c a

# -br = brief basic information
# -c = colored output

# For more help on an option use help
ip a help

# Check the ip link option. Temporary disable/enable a device
sudo ip link set dev ens33 down

# Display routing table
sudo ip route list
```

### route

- **`route`** - show and manipulate the IP routing table

```bash
route
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    default         _gateway        0.0.0.0         UG    100    0        0 ens33
    link-local      0.0.0.0         255.255.0.0     U     1000   0        0 ens33
    192.168.31.0    0.0.0.0         255.255.255.0   U     100    0        0 ens33

# Add a route
sudo ip route add 192.168.1.0/24 via 192.168.31.1
sudo ip route delete 192.168.1.0/24 via 192.168.31.1
```

### nslookup / dig

- **`nslookup`** - query Internet name servers interactively
- **`dig`** - DNS lookup utility. More information than `nslookup`

```bash
nslookup google.com
    Server:		127.0.0.53
    Address:	127.0.0.53#53

    Non-authoritative answer:
    Name:	google.com
    Address: 142.251.209.14
    Name:	google.com
    Address: 2a00:1450:4002:410::200e
```

```bash
dig google.com
    ; <<>> DiG 9.18.1-1ubuntu1.2-Ubuntu <<>> google.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 42021
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 65494
    ;; QUESTION SECTION:
    ;google.com.			IN	A

    ;; ANSWER SECTION:
    google.com.		5	IN	A	142.251.209.14

    ;; Query time: 31 msec
    ;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
    ;; WHEN: Wed Oct 26 19:29:30 CEST 2022
    ;; MSG SIZE  rcvd: 55

# Perform a reverse lookup
dig -x 8.8.8.8
    ; <<>> DiG 9.18.1-1ubuntu1.2-Ubuntu <<>> -x 8.8.8.8
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15982
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 65494
    ;; QUESTION SECTION:
    ;8.8.8.8.in-addr.arpa.		IN	PTR

    ;; ANSWER SECTION:
    8.8.8.8.in-addr.arpa.	5	IN	PTR	dns.google.

    ;; Query time: 36 msec
    ;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
    ;; WHEN: Wed Oct 26 19:31:02 CEST 2022
    ;; MSG SIZE  rcvd: 73
```

### netstat

- **`netstat`** - print network connections, routing tables, interface statistics, masquerade connections, and multicast member‐ships

```bash
# Show all the open TCP connections
netstat -at

# Show all the listening TCP ports
netstat -lt
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State      
    tcp        0      0 localhost:ipp           0.0.0.0:*               LISTEN
    tcp        0      0 localhost:domain        0.0.0.0:*               LISTEN
    tcp6       0      0 ip6-localhost:ipp       [::]:*                  LISTEN
```

### ss

- **`ss`** - it replaces the netstat command

```bash
ss -tunlp
    Netid       State        Recv-Q        Send-Q               Local Address:Port                Peer Address:Port       Process
    udp         UNCONN       0             0                          0.0.0.0:5353                     0.0.0.0:*
    udp         UNCONN       0             0                    127.0.0.53%lo:53                       0.0.0.0:*
    udp         UNCONN       0             0                          0.0.0.0:631                      0.0.0.0:*
    udp         UNCONN       0             0                          0.0.0.0:35782                    0.0.0.0:*
    udp         UNCONN       0             0                             [::]:5353                        [::]:*
    udp         UNCONN       0             0                             [::]:50159                       [::]:*
    tcp         LISTEN       0             4096                 127.0.0.53%lo:53                       0.0.0.0:*
    tcp         LISTEN       0             128                      127.0.0.1:631                      0.0.0.0:*
    tcp         LISTEN       0             128                          [::1]:631                         [::]:*
```

> 📌 More on checking for listening ports [here](https://linuxize.com/post/check-listening-ports-linux/).

------

