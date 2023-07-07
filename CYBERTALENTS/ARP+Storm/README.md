# Description

An attacker in the network is trying to poison the arp table of 11.0.0.100, the admin captured this PCAP.

URL:https://hubchallenges.s3-eu-west-1.amazonaws.com/Forensics/ARP+Storm.pcap
# Solution

After loading the packet in Wireshark

we check the info section, you will find it contains some hex characters
![Screenshot](1)

We then use **t-shark** and **cut** to extract those characters

It can be seen that each packet uses a different opcode in hexadecimal format. We'll take the opcode of each package, then display it in ASCII format, using AWK with a command like this

```bash
└─$ tshark -r ARP+Storm.pcap -Tfields -e arp.opcode | awk '{printf("%c",$1)}'   
ZmxhZ3tnckB0dWl0MHVzXzBwY09kZV8xc19BbHdAeXNfQTZ1U2VkX3QwX3AwMXMwbn0=


└─$ echo 'ZmxhZ3tnckB0dWl0MHVzXzBwY09kZV8xc19BbHdAeXNfQTZ1U2VkX3QwX3AwMXMwbn0=' | base64 -d 
flag{gr@tuit0us_0pcOde_1s_Alw@ys_A6uSed_t0_p01s0n}                   
```
