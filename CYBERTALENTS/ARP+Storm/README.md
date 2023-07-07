# Description

An attacker in the network is trying to poison the arp table of 11.0.0.100, the admin captured this PCAP.

URL:https://hubchallenges.s3-eu-west-1.amazonaws.com/Forensics/ARP+Storm.pcap
# Solution

After loading the packet in Wireshark
```bash
16	0.004361	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0030
17	0.004655	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x004d
18	0.004899	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0048
19	0.005182	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0056
20	0.005433	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x007a
21	0.005786	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0058
22	0.006149	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x007a
23	0.006403	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0042
24	0.006715	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0077
25	0.006960	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0059
26	0.007296	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0030
27	0.007592	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0039
28	0.007838	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x006b
29	0.008127	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x005a
30	0.008471	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0056
31	0.008741	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0038
32	0.009054	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0078
33	0.009311	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0063
34	0.009717	VMware_1e:1d:81	Broadcast	ARP	42	Unknown ARP opcode 0x0031
```

we check the info section, you will find it contains some hex characters

We then use **t-shark** and **cut** to extract those characters

It can be seen that each packet uses a different opcode in hexadecimal format. We'll take the opcode of each package, then display it in ASCII format, using AWK with a command like this
```bash
└─$ tshark -r ARP+Storm.pcap -Tfields -e arp.opcode | awk '{printf("%c",$1)}'   
ZmxhZ3tnckB0dWl0MHVzXzBwY09kZV8xc19BbHdAeXNfQTZ1U2VkX3QwX3AwMXMwbn0=
```

# Base64 decode
```bash
└─$ echo 'ZmxhZ3tnckB0dWl0MHVzXzBwY09kZV8xc19BbHdAeXNfQTZ1U2VkX3QwX3AwMXMwbn0=' | base64 -d 
flag{gr@tuit0us_0pcOde_1s_Alw@ys_A6uSed_t0_p01s0n}                   
```
