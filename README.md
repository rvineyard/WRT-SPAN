WRT-SPAN
========

OpenWRT Switch Port Analyzer (SPAN) Project

http://vinsec.blogspot.com

I've got a friend helping me turn this into something a bit more legible (and preferably in SVG format so I can revise it if necessary), but here is a rough draft of what's going on inside my makeshift aggregating ethernet tap:<br />
<br />
<div class="separator" style="clear: both; text-align: center;">
<a href="http://2.bp.blogspot.com/-TcDXwOw56Zo/T6wMje3iiyI/AAAAAAAAAEU/vZJ2iNpUStA/s1600/kenny-draft1.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img alt="" border="0" height="320" src="http://2.bp.blogspot.com/-TcDXwOw56Zo/T6wMje3iiyI/AAAAAAAAAEU/vZJ2iNpUStA/s400/kenny-draft1.png" title="" width="400" /></a></div>
<br />
Everything on my network is named after a character from South Park, and Kenny seemed like an appropriate name for this little fella because he died many, many horrible deaths along the way to getting this right.

I'm hoping to have the full writeup completed today or tomorrow, with pretty diagrams to follow shortly.
<br />
<h2>Ethernet and Bridge Configuration</h2>
<pre>

root@kenny:~# dmesg | grep Ethernet
ssb: Core 1 found: Fast Ethernet (cc 0x806, rev 0x06, vendor 0x4243)
ssb: Core 2 found: Fast Ethernet (cc 0x806, rev 0x06, vendor 0x4243)
eth0: Broadcom 44xx/47xx 10/100BaseT Ethernet 00:1d:7e:43:e1:66
eth1: Broadcom 44xx/47xx 10/100BaseT Ethernet 00:88:88:88:00:2a



root@kenny:~# ifconfig
br-bridge Link encap:Ethernet  HWaddr 00:1D:7E:43:E1:66
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:75498956 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1537411031 (1.4 GiB)  TX bytes:0 (0.0 B)

eth0      Link encap:Ethernet  HWaddr 00:1D:7E:43:E1:66
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:75525684 errors:0 dropped:0 overruns:0 frame:0
          TX packets:84951464 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:3219450758 (2.9 GiB)  TX bytes:3884550799 (3.6 GiB)
          Interrupt:4

eth0.1    Link encap:Ethernet  HWaddr 00:1D:7E:43:E1:66
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:53110718 errors:0 dropped:0 overruns:0 frame:0
          TX packets:22388245 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:2504502284 (2.3 GiB)  TX bytes:3943308308 (3.6 GiB)

eth0.2    Link encap:Ethernet  HWaddr 00:1D:7E:43:E1:66
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:22388245 errors:0 dropped:0 overruns:0 frame:0
          TX packets:53110714 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:3629872878 (3.3 GiB)  TX bytes:3248052080 (3.0 GiB)

eth0.3    Link encap:Ethernet  HWaddr 00:1D:7E:43:E1:66
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:9407187 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 B)  TX bytes:625278740 (596.3 MiB)

eth0.4    Link encap:Ethernet  HWaddr 00:1D:7E:43:E1:66
          inet addr:192.168.2.2  Bcast:192.168.2.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:6794 errors:0 dropped:0 overruns:0 frame:0
          TX packets:8698 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:505751 (493.8 KiB)  TX bytes:2418973 (2.3 MiB)

eth1      Link encap:Ethernet  HWaddr 00:88:88:88:00:2A
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:95697991 errors:0 dropped:0 overruns:0 frame:0
          TX packets:64587510 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:1880088266 (1.7 GiB)  TX bytes:3105379375 (2.8 GiB)
          Interrupt:5

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:1 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:91 (91.0 B)  TX bytes:91 (91.0 B)



root@kenny:~# brctl show
bridge name     bridge id               STP enabled     interfaces
br-bridgetap    8000.001d7e43e166       no              eth0.1
                                                        eth0.2
                                                        eth0.3
                                                        
</pre>