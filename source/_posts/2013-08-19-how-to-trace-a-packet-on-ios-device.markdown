---
layout: post
title: "How to Trace a packet on iOS Device"
date: 2013-08-19 21:29
comments: true
categories: [ios, unix] 
---
When we develop iOS program with network requirement, sometimes we may need to capture packet to debug a network problem. Apple has provide a [network share method and some hints](https://developer.apple.com/library/mac/qa/qa1176/_index.html#//apple_ref/doc/uid/DTS10001707-CH1-SECGETTINGSTARTEDWITHTCPDUMP) for packet trace. Beside this way, we can also use iOS device to capture packet.
## Jailbreak your iPhone(iPad) and install dependency
1. First we need to jailbreak ios device, this is compulsory.
2. Open cydia, search and install [openssh](http://www.openssh.org/). After openssh has been installed, now you can use ssh to connect your iphone if you are in the same sub-network. You can login ios device with root accout and the default password alpine 
{% codeblock %}
$ ssh root@192.168.1.1
{% endcodeblock %}
If you have installed openssh, you'd better to change the default password to protect your ios device </br>
{% img center /images/packet_trace/ssh.png %}
3. As ios system actually is a branch of unix(Darwin), we use unix command as usual. But now the system only provide basic command such like 'ls', so if we want to use tcpdump, we need to install it first. The easiest way to install command is to use apt-get. But as this is also inavailable, we can install apt-get command by cydia.
4. Seach apt in Cydia and install APT 6.0 transitional package. After installed, you can use apt-get command to install unix tool.
{% img center /images/packet_trace/install-tcpdump.png %}
## Start to capture packet
Now, we can start to cpature packet on ios device. Before that we'd better to check which network interface is used for send packet. 
{% img center /images/packet_trace/ifconfig.png %}
{% codeblock %}
$ tcpdump -i en0 -s 0 -w /tmp/DumpFile.pcap //if no -s 0, when analyse by wireshark, it will reports "packet size limited during capture"
{% endcodeblock %}
After dump packet to a local file, we can use scp to transfer the file to local and analyse with wireshark. 
