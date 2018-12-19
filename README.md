# SecureCRT Text Highlighting

During my CCIE studies, I created a bunch of regular expressions to highlight the output of show commands. These will work with the show commands on IOS and IOS-XE images.

The regular expressions are based on syntax used with Python. https://docs.python.org/2/library/re.html

SecureCRT does not support using spaces in regular expressions. Which sucks. Spaces would be handy for matching with the OSPF database, EIGRP topology, or the BGP table. There are many references in the VanDyke forums that the performance hit was too large. With SecureCRT, the following are considered word delimiters: `~!#$%^&*()+=:;<>,.?/\[]{}|’

Order matters. Generally, match the more specific (longer) before the less specific (shorter). Otherwise, you’ll get partial matches. For example, match “errors” before “error” before “err”. Unless, of course, you want to override more specific matches. For example, emergencies, alerts, and critical log messages with %\w*\-[012]\-\w*.

I gave up on writing my own single line IPv6 address regex and used one from the book Regular Expressions Cookbook, 2nd Edition by Steven Levithan, Jan Goyvaerts. While I was at it, I used one of their regex for IPv4 addresses.

There are some false matches. I’ve use the “Case match” option to reduce them as much as possible. Still, things such as “B”, “R”, and “DR” still show up in some odd places. Meh.

The colors I use are meant for a dark (black) background with white text. Because of this, dark colors are avoided. I’ve tried to group the output of some protocols together: BGP – blue, RIP – red, OSPF – orange, EIGRP – evergreen (but evergreen is too dark, so a lighter shade of green).

If you don’t want to manually configure the highlight keywords in SecureCRT, you can use this .ini file. For OS X, copy the .ini file into the following directory:
/Users/username/Library/Application Support/VanDyke/SecureCRT/Config/Keywords.

http://download.feralpacket.org/Lab%20Highlights.ini

Note: Using the highlighting can lead to a habit of expecting the colors point out bad stuff. The highlighting is not going to be available during the CCIE lab. At least a month before your lab date, you should stop using the highlighting.


## SecureCRT Settings:

- **Session Options**
  - **Terminal**
    - **Appearance**
      - **Current color scheme**
        - White / Black
      - **Highlight keywords**
        - Name: Lab Highlights
        - Style: Color is checked
        - **Keyword List Properties**
          - Match case is checked

## To set for the default session:
- **Global Options**
  - **General** 
    - **Default Session**
      - Edit Default Settings...

The regular expressions:
------------------------


**IPv4-mapped IPv6 Addresses**

```regexp
(::FFFF)?::?(?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])
```

**VPNv6 Addresses**

```regexp
\[\d{1,10}:\d{1,10}\](?:(?:[0-9A-Fa-f]{1,4}:){7}[0-9A-Fa-f]{1,4}|(?=(?:[0-9A-Fa-f]{0,4}:){0,7}[0-9A-Fa-f]{0,4}(?![:.\w]))(([0-9A-Fa-f]{1,4}:){1,7}|:)((:[0-9A-Fa-f]{1,4}){1,7}|:)|(?:[0-9A-Fa-f]{1,4}:){7}:|:(:[0-9A-Fa-f]{1,4}){7})(?![:.\w])
\[\d{1,3}(\.\d{1,3}){3}:\d{1,10}\](?:(?:[0-9A-Fa-f]{1,4}:){7}[0-9A-Fa-f]{1,4}|(?=(?:[0-9A-Fa-f]{0,4}:){0,7}[0-9A-Fa-f]{0,4}(?![:.\w]))(([0-9A-Fa-f]{1,4}:){1,7}|:)((:[0-9A-Fa-f]{1,4}){1,7}|:)|(?:[0-9A-Fa-f]{1,4}:){7}:|:(:[0-9A-Fa-f]{1,4}){7})(?![:.\w])
```

**VPNv4 Addresses**

```regexp
(\d{1,10}:){2}(?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])
(?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9]):\d{1,10}:(?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])
```

**IPv6 Addresses**

```regexp
(?:(?:[0-9A-Fa-f]{1,4}:){7}[0-9A-Fa-f]{1,4}|(?=(?:[0-9A-Fa-f]{0,4}:){0,7}[0-9A-Fa-f]{0,4}(?![:.\w]))(([0-9A-Fa-f]{1,4}:){1,7}|:)((:[0-9A-Fa-f]{1,4}){1,7}|:)|(?:[0-9A-Fa-f]{1,4}:){7}:|:(:[0-9A-Fa-f]{1,4}){7})(?![:.\w])
```

**Time**

```regexp
((2[0-3]|[01][0-9])(:([0-5]?[0-9])){2}\.\d{3}|(2[0-3]|[01][0-9])(:([0-5]?[0-9])){1,2}|\d{1,4}d\d{2}h)
```

**IP-address:nn RD or RT**

```regexp
(RT:)?(?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9]):\d{2,5}|(RT:)?(?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9]):[1-9])
```

**ASN:nn RD or RT**

```regexp
((RT:)?\d{1,10}:\d{2,10}|(RT:)?\d{1,10}:[1-9])
```

**IPv4 Addresses**

```regexp
(?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])
```

**Interfaces**

```regexp
(GigabitEthernet\d/\d{1,2}\.\d{1,10}|GigabitEthernet\d/\d{1,2}|Gi\d/\d{1,2}\.\d{1,10}|Gi\d/\d{1,2}|G\d/\d{1,2}\.\d{1,10}|G\d/\d{1,2})
(FastEthernet\d/\d{1,2}\.\d{1,10}|FastEthernet\d/\d{1,2}|Fa\d/\d{1,2}\.\d{1,10}|Fa\d/\d{1,2}|F\d/\d{1,2}\.\d{1,10}|F\d/\d{1,2})
(Ethernet\d/\d{1,2}\.\d{1,10}|Ethernet\d/\d{1,2}|Et\d/\d{1,2}\.\d{1,10}|Et\d/\d{1,2}|E\d/\d{1,2}.\d{1,10}|E\d/\d{1,2})
(Serial\d/\d\.\d{1,10}|Serial\d/\d|Se\d/\d\.\d{1,10}|Se\d/\d|S\d/\d\.\d{1,10}|S\d/\d)
(Loopback\d{1,10}|Lo\d{1,10}|L\d{1,10}|Tunnel\d{1,3}|Tu\d{1,3}|T\d{1,3}|Vlan\d{1,4}|Vl\d{1,4})
(Portchannel\d{1,2}|Po\d{1,2}|NVI\d{1,2}|Virtual-Template\d{1,3}|Virtual-Access\d{1,3}\.\d{1,10}|Virtual-Access\d{1,3}|Vi\d{1,3}\.\d{1,10}|Vi\d{1,3}|Multilink\d{1,10}|Mu\d{1,10}|Dialer\d{1,3}|Di\d{1,3}|BVI\d{1,3})
```

**Bad responses**

```regexp
(down|Down|DOWN|fail|failed|not|bad|never|BLK|fddi|n\-isl|isl|notconnect|blocking|\(tdp\)|tdp|TDP|denied|invalid|err\-disabled|unusable|DENIED)
(err\-disable|infinity|inaccessible|\*ROOT_Inc|BKN\*|\*LOOP_Inc|wrong|cannot|MM_NO_STATE|MM_KEY_EXCH|UP\-NO\-IKE)
(K[13]=(\d{2,3}|[02-9])|K[245]=(\d{2,3}|[1-9]))
```

**Good responses**

```regexp
(rstp|best|ldp|CIST|QM_IDLE|(IP|L|CDP)CP\+|CHAP\+|PAP\+|(IP|L|CDP)CP\[Open\]|our_master|UP\-ACTIVE)
(\*\>|FWD|root|Root|802\.1q|connected|LocalT|yes|\(ldp\)|ldp|\(SU\)|\(RU\)|forwarding|synchronized|active|rapid\-pvst|up|Up|UP|FULL)
```

**Possible warnings and unusual things that deserve attention**

```regexp
(errors|error|err|reset|act/unsup|dhcp|DHCP|mismatch|notconnect|drops|dropped|runts|CRC|collisions|collision|
LRN|learning|listening|LIS|unsynchronized)
(Peer\(STP\)|Shr|Edge|pvst|ieee|Bound\(PVST\)|TFTP|Mbgp|LAPB|l2ckt\(\d{1,10}\)|DCE|DTE|passive|\[ANY\]|r|RIB\-failure|discriminator|Standby)
(aggregate(d|\/\w*)|atomic\-aggregate|\[V\]|ATTEMPT|INIT|2WAY|EXCHANGE|LOADING|\(global\)|tag|key\-chain|md5|backup\/repair|repair|v2\/D|v2\/SD)
(Condition\-map|Advertise\-map|no\-advertise|no\-export|local\-AS|internet)
```

**BGP**

```regexp
(bgp|BGP|B|IGP|incomplete|\d{2,7}\/nolabel\(\w*\)|RR\-client|Originator|cluster\-id|Cluster\-id|Cluster|Route\-Reflector)
%BGP\-\d\-\w*
%BGP_SESSION\-\d\-\w*
```

**OSPFv2 and OSPFv3**

```regexp
(OSPF_VL\d{1,2}|OSPF_SL\d{1,2}|VL\d{1,2}|SL\d{1,2}|Type\-\d|ospf|OSPF|O|IA|E[12]|N[12]|P2P|P2MP|BDR|DR|ABR|ASBR|LOOP|DROTHER)
(POINT_TO_POINT|POINT_TO_MULTIPOINT|BROADCAST|NON_BROADCAST|LOOPBACK|SHAM_LINK|3101|1587|transit|Transit|nssa|NSSA|stub)
(Stub|Superbackbone|OSPFv3_VL\d{1,2}|OSPFv3\-\d{1,5}\-IPv6|ospfv3|OSPFv3|OI|OE[12]|ON[12]|V6\-Bit|E\-Bit|R\-bit|DC\-Bit|opaque|DROTH)
(%OSPF\-\d\-\w*|%OSPFV3\-\d\-\w*)
```

**EIGRP**

```regexp
(EIGRP\-IPv6|EIGRP\-IPv4|eigrp|EIGRP|EX|D|K[13]=1|K[245]=0|Internal|External)
%DUAL\-\d\-\w*
```

**RIP**

```regexp
(rip|RIP|R)
```

**PIM, MSDP, and IGMP**

```regexp
(PIM\/IPv4|RP\:|v2\/S|BSR)
%PIM\-\d\-\w*
%MSDP\-\d\-\w*
%IGMP\-\d\-\w*
```

**Lab hostnames**

```regexp
(R\d{1,2}%\w*|R\d{1,2}|SW\d{1,2}|BB\d|BB|PE\d{1,2}|CE\d{1,2}|P\d{1,2}|FRS)
```

**Routing table metrics**

```regexp
\[\d{1,3}/\d{1,12}\]
```

**EIGRP topology table metrics and ping responses**

```regexp
\(\d{1,12}/\d{1,12}\)
```

**show ip traffic | show ipv6 traffic - still testing**

```regexp
(fragmented|fragments|fragment|drop:|reverse,|problem|unknown)
```

**NHRP next-hop override**

```regexp
(DT2|T2)
```

**Bad - emergencies, alerts, and critical log messages**

```regexp
%\w*\-[012]\-\w*
```

**LDP**

```regexp
%LDP\-\d\-\w*
%LSD\-\d\-\w*
```

**IPv6 Neighbor Discovery**

```regexp
%IPV6_ND\-\d\-\w*
```

**Various log messages I turn yellow**

```regexp
%CDP\-\d\-\w*
%DHCP\-\d\-\w*
%SPANTREE\-\d\-\w*
%TDP\-\d\-\w*
%SW_DAI\-\d\-\w*
%SW_CLAN\-\d\-\w*
%PM\-d\-\w*
%STORM_CONTROL\-\d-\w*
%PV\-\d\-\w*
%SPANTREE_FAST\-\d\-\w*
%TRACK\-\d\-\w*
%TRACKING\-\d\-\w*
%FR_EEK\-\d\-\w*
%HSRP\-\d\-\w*
%SNAT\-\d\-\w*
%SEC\-\d\-\w*
%VRRP\-\d\-\w*
%IPRT\-\d\-\w*
%SYS\-\d\-LOGGINGHOST\w*
%PARSER\-\d\-CFGLOG\w*
%ENVMON\-\d\-\w*
%EC\-\d\-\w*
%GLBP\-\d\-\w*
%CRYPTO\-\d\-\w*
%NHRP\-\d\-\w*
%SNMP\-\d\-\w*
%CP\-\d\-\w*
%HA_EM\-\d\-\w*
%TCP\-\d\-\w*
%IP\-\d\-\w*
```

**Catch-all for all other log messages**

```regexp
%\w*\-\d\-\w*
```


**Original source and README from [Feral Packet's blog](http://feralpacket.org/?p=299).**
