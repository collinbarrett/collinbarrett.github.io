---
id: 8456
title: 'DD-WRT: Connecting to the Optimal ProtonVPN Server with OpenVPN'
date: '2020-06-06T16:18:48-05:00'
author: 'Collin M. Barrett'
excerpt: 'I wrote a shell script triggered by cron on my DD-WRT router to automatically connect to the optimal ProtonVPN
server via the OpenVPN Client'
layout: post
guid: '/?p=8456'
permalink: /protonvpn-dd-wrt-api-script/
image: /assets/img/serverRack_collinmbarrett.jpg
categories:
- InfoSec
tags:
- Cloudflare
- DNS
- Encryption
- Hardware
- ISP
- Linux
- Performance
- Router
- Tracking
- VPN
- Wi-Fi
---

**Update 6.27.2022:**

Added most recent script to GitHub rather than updating this post. ProtonVPN now supports customized OpenVPN username
suffixes to specify exit node and additional features. See latest script
[here](https://github.com/collinbarrett/dd-wrt/blob/main/vpn-refresh.sh).

- - - - - -

**Update 7.28.2021:**

Updated and simplified the script to continue to support PBR per [this forum
post](https://forum.dd-wrt.com/phpBB2/viewtopic.php?p=1242050).

- - - - - -

In a [recent post](/protonvpn-dd-wrt-dns/), I outlined a solution that I was testing for configuring my DD-WRT router to
connect to an optimal ProtonVPN server. In short, I configured a domain name that I own to return a
[round-robin](https://www.cloudflare.com/learning/dns/glossary/round-robin-dns/) IP address from a set of generally
optimal servers for my location with Cloudflare.

This solution partially mimicked ProtonVPN’s country configuration option that allows you to connect to
`us.protonvpn.com` which should resolve to the most ideal US IP for your location and the current server load. In my
testing, however, this domain often connected me to servers that were physically far from my location with sub-par speed
and latency.

My custom domain solution worked alright but did not take into account the servers’ current loads. Server loads seem to
be fluctuating more wildly during the pandemic, and performance degradation can be quite noticeable during peak loads.
ProtonVPN exposes an API endpoint [here](https://api.protonmail.ch/vpn/logicals) that provides up-to-date `Load` and
`Score` metrics for each server. In this post, I outline how I automated selecting a more optimal server IP via a shell
script on DD-WRT. This solution assumes that you already have the DD-WRT OpenVPN Client enabled and connected to
ProtonVPN ([ProtonVPN docs](https://protonvpn.com/support/vpn-router-ddwrt/)).

## Entware

To parse the JSON results from the ProtonVPN API, I wanted to use the popular `<a
  href="https://stedolan.github.io/jq/">jq</a>` command-line tool. DD-WRT does not ship with this tool out of the box,
however, so I first had to install the Entware package manager.

The [DD-WRT wiki page](https://wiki.dd-wrt.com/wiki/index.php/Installing_Entware) provides fairly straightforward
instructions on how to format a USB flash drive and install the package manager. I connected to my router via
[telnet](https://wiki.dd-wrt.com/wiki/index.php/Telnet/SSH_and_the_Command_Line). The installation went smoothly for me
following that guide, but there could certainly be room for errors or hiccups, especially amongst various routers,
chipsets, and DD-WRT builds.

Once Entware was installed, I installed `jq` with:

```
<pre class="wp-block-preformatted">opkg install jq
```

## curl

The next step of the process was `curl`ing the ProtonVPN API and piping the results through a `jq` query to select the currently optimal IP address.

I am not sure if their API respects the `no-cache` header, but I decided to pass it along regardless in the hopes of always receiving the freshest results. The API returns JSON by default, but I prefer to be explicit and request a response of `application/json`.

```
<pre class="wp-block-preformatted">curl -s -H "Cache-Control: no-cache" -H "Accept: application/json" https://api.protonmail.ch/vpn/logicals
```

I have noticed that sometimes the ssl negotiation fails when calling the API with curl on DD-WRT. I suspect this to be an issue with my DD-WRT configuration. The workaround, although undoubtedly a bit insecure, is passing the `-k` (`--insecure`) flag to curl.

## jq

Below, I filter the JSON response with `jq`. I select a `Status` of `1` which means to only consider servers that are currently online. Since I pay for a Plus subscription, I only want to connect to Tier 2 servers which theoretically have more available capacity as they are reserved for higher-paying subscribers. I only want to connect to a server in one of the nearest metros to my current location to reduce latency. I specified that I do not want one of ProtonVPN’s tor nodes due to the much higher performance penalty that incurs (`.Features == 8`).

With that subset of servers selected, I applied a sort by Score and then by Load. Lastly, I selected the top IP address from the list.

```
<pre class="wp-block-preformatted">echo "$LOGICALS" | jq '.LogicalServers | map(select(.Status == 1 and .Tier == 2 and .Features == 8 and (.City | (contains("Atlanta") or contains("Dallas") or contains("Chicago"))))) | [sort_by(.Score, .Load)[]][0] | .Servers[0].EntryIP'
```

## Shell Script

Here is the final script I am using (also available as a [gist](https://gist.github.com/collinbarrett/abeaf6edeb1cfb49d9beacd6d325d3c2)). Since I run the script from cron, I needed to define the PATH so the script knows where to locate tools like `jq` and `curl`. Since ProtonVPN’s API factors in my current location in its `Score` values, the call to their API is the one network request that I do want to execute on my raw WAN connection.

After fetching and filtering to the optimal server IP, I update the remote in nvram and then restart the `openvpn` service.

```
<pre class="wp-block-preformatted">#!/bin/sh

# specify PATH to run from cron
PATH=/bin:/usr/bin:/sbin:/usr/sbin:/jffs/sbin:/jffs/bin:/jffs/usr/sbin:/jffs/usr/bin:/mmc/sbin:/mmc/bin:/mmc/usr/sbin:/mmc/usr/bin:/opt/sbin:/opt/bin:/opt/usr/sbin:/opt/usr/bin

# on router startup, wait for network to initially connect
# sleep 25

# verify not on VPN when fetching ProtonVPN server scores
# MYPUBLICIP=$(curl -s http://whatismyip.akamai.com/)
# echo "My public IP is ${MYPUBLICIP}"

# fetch ProtonVPN server info
LOGICALS=$(curl -sk -H "Cache-Control: no-cache" -H "Accept: application/json" https://api.protonmail.ch/vpn/logicals)

# query for optimal server
IPSTRING=$(echo "$LOGICALS" | jq '.LogicalServers | map(select(.Status == 1 and .Tier == 2 and .Features == 8 and (.City | (contains("Atlanta") or contains("Dallas") or contains("Chicago"))))) | [sort_by(.Score, .Load)[]][0] | .Servers[0].EntryIP')

if [ -n "$IPSTRING" ]
then
  IPSTRING="${IPSTRING%\"}"
  IP="${IPSTRING#\"}"

  CURRENTIP=$(nvram get openvpncl_remoteip)
  echo "The current OpenVPN server IP is ${CURRENTIP}"

  if [ "$IP" != "$CURRENTIP" ]
  then
    echo "Connecting to $IP..."

    # update OpenVPN server
    nvram set openvpncl_remoteip="${IP}"
    nvram commit

    # restart openvpn
    stopservice openvpn
    startservice openvpn
  else
    echo "The optimal ProtonVPN server ($IP) has not changed. Aborting..."
  fi
else
  echo "Failed to fetch ProtonVPN server info. Aborting..."
fi
```

## Cron

I use cron jobs to execute this script automatically. The DD-WRT GUI enables this configuration, which I wired up to refresh the connection twice a day (7:25 am and 5 pm, the beginning and end of my typical workday).

<figure class="wp-block-image size-large">![DD-WRT Cron](/assets/img/ddwrtCron_collinmbarrett.jpg)</figure>## Startup

Additionally, I have enabled DD-WRT’s watchdog service to ping a large public DNS provider (I use Cloudflare’s 1.1.1.1) every so often to ensure my router still has connection to the WAN. When WAN connection is down, the router will automatically reboot. When DD-WRT starts back up, the OpenVPN connection would use the default server configured

I put a copy of the VPN refresh script at `/jffs/etc/config/vpn-refresh.wanup`. The `wanup` extension flags it for automatic execution whenever DD-WRT’s WAN connection is up.

## Conclusion

I have been using this solution for about a week, and I have been running speed tests at various times of the day. Every time I have tested, I have been able to max out my ISP plan’s bandwidth while connected to a ProtonVPN server without any issue. I also have been able to connect to teleconferencing tools without any noticeable latency or quality degradation.

I have been documenting my full DD-WRT configuration [over on GitHub](https://github.com/collinbarrett/dd-wrt), if you want to learn more about how I use DD-WRT.