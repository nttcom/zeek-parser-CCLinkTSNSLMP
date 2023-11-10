# Zeek-Parser-CCLinkTSNSLMP

## Overview

Zeek-Parser-CCLinkTSNSLMP is a Zeek plug-in that can analyze IP frame in CC-Link IE TSN of the [CC-Link family](https://www.cc-link.org/ja/cclink/index.html).

## Installation

### Manual Installation

Before using this plug-in, please make sure Zeek, Spicy has been installed.

````
# Check Zeek
~$ zeek -version
zeek version 5.0.0

# Check Spicy
~$ spicyz -version
1.3.16
~$ spicyc -version
spicyc v1.5.0 (d0bc6053)

# The path of zeek in this manual is based on the following output
~$ which zeek
/usr/local/zeek/bin/zeek
````

Use `git clone` to get a copy of this repository to your local environment.
```
~$ git clone https://github.com/nttcom/zeek-parser-CCLinkTSNSLMP.git
```

## Usage

### For manual installation

Compile source code and copy the object files to the following path.
```
~$ cd ~/zeek-parser-CCLinkTSNSLMP/analyzer
~$ spicyz -o cc_link_tsn_slmp.hlto cc_link_tsn_slmp.spicy cc_link_tsn_slmp.evt
# cc_link_tsn_slmp.hlto is generated
~$ cp cc_link_tsn_slmp.hlto /usr/local/zeek/lib/zeek-spicy/modules/
```

Then, copy the zeek file to the following paths.
```
~$ cd ~/zeek-parser-CCLinkTSNSLMP/scripts/
~$ cp main.zeek /usr/local/zeek/share/zeek/site/cc_link_tsn_slmp.zeek
```

Finally, import the Zeek plugin.
```
~$ tail /usr/local/zeek/share/zeek/site/local.zeek
... Omit ...
@load cc_link_tsn_slmp
```

This plug-in generates a `cclink-ie-tsn-slmp.log` by the command below:
```
~$ cd ~/zeek-parser-CCLinkTSNSLMP/testing/Traces
~$ zeek -Cr test.pcap /usr/local/zeek/share/zeek/site/cc_link_tsn_slmp.zeek
```

## Log type and description

This plug-in monitors all functions of IP frame in CC-Link IE TSN and outputs them as `cclink-ie-tsn-slmp.log`.

| Field | Type | Description |
| --- | --- | --- |
| ts | time | timestamp of the first communication |
| uid | string | unique ID for this connection |
| id.orig_h | addr | source IP address |
| id.orig_p | port | source port number |
| id.resp_h | addr | destination IP address  |
| id.resp_p | port | destination port number   |
| service | string | protocol name |
| flame_type | string | data frame name |
| pdu_type | string | protocol function name |
| cmd | string | specification of operations for the station with command and subcommand |
| number | int | number of packet occurrence |
| ts_end | time | timestamp of the last communication |

An example of `cclink-ie-tsn-slmp.log` is as follows:
```
#separator \x09
#set_separator	,
#empty_field	(empty)
#unset_field	-
#path	cclink-ie-tsn-slmp
#open	2023-11-08-08-01-46
#fields	ts	uid	id.orig_h	id.orig_p	id.resp_h	id.resp_p	service	flame_type	pdu_type	cmd	number	ts_end
#types	time	string	addr	port	addr	port	string	string	string	string	int	time
1695784130.196558	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-notification	ResNotification	10	1695784186.147303
1695784131.117451	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-networkConfigTslt	ReqNetworkConfigTslt	10	1695784187.077320
1695784131.151672	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-slaveConfig	ReqSlaveConfig	10	1695784187.112323
1695784130.563997	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-cyclicConfigRcvSrcInfo	ReqCyclicConfigRcvSrcInfo	10	1695784186.521313
1695784130.563923	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-cyclicConfigTrnSubPayload	ReqCyclicConfigTrnSubPayload	10	1695784186.521220
1695784130.563802	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-cyclicConfigMain	ReqCyclicConfigMain	10	1695784186.521153
#close	2023-11-08-08-01-46
```

## Related Software

This plug-in is used by [OsecT](https://github.com/nttcom/OsecT).
