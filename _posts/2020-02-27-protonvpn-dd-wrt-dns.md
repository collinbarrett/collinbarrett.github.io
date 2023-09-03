---
title: 'Optimizing ProtonVPN Server Selection by DD-WRT with DNS'
date: '2020-02-27T06:55:12-06:00'
author: 'Collin M. Barrett'
excerpt: 'I created a custom URL to connect to ProtonVPN from DD-WRT that selects from a pool of premium servers in the
nearest city.'
layout: post-wp-import
guid: '/?p=7858'
permalink: /protonvpn-dd-wrt-dns
redirect_from: /protonvpn-dd-wrt-dns/
image: /assets/img/tunnel_collinmbarrett.jpg
categories:
- InfoSec
tags:
- Advertisements
- Cloudflare
- DNS
- Encryption
- ISP
- Performance
- Router
- Tracking
- VPN
- Wi-Fi
---

**Update 6.6.2020:**

While this solution still works, I now use a new shell script to optimize server selection based on current load. See
[here](/protonvpn-dd-wrt-api-script/).

- - - - - -

I have been a long-time user of virtual private networks for personal privacy. It is not a silver bullet, but at the
very least it makes it just a bit harder for ISPs, government agencies, and all of the sites we visit daily to profile
and surveil my family.

[ProtonVPN](https://protonvpn.com/) has worked very well for me for the past couple of years. Since I use
[ProtonMail](https://protonmail.com/), it makes sense to use their VPN as a bundled offering. ProtonVPN is also maybe
the only reputable provider offering a free tier which is a trait I like to support for those who cannot pay for premium
access.

Choosing a server to [connect to via DD-WRT](https://protonvpn.com/support/vpn-router-ddwrt/) on our router is a bit of
a challenge, however. This is how I have sought to optimize that choice for greater uptime, reduced latency, and
automatic failover.

## Selecting a Server

<div class="wp-block-image">
    <figure class="alignright size-medium">[![ProtonVPN Server
        Configs](/assets/img/protonVpnConfigs_collinmbarrett-300x239.jpg)](/assets/img/protonVpnConfigs_collinmbarrett.jpg)
        <figcaption>The ProtonVPN configuration download portal</figcaption>
    </figure>
</div>ProtonVPN has hundreds of servers classified by type (Secure Core, Tor, regular), class (basic or premium), and
location. For users who connect to the VPN through applications on their devices, selecting which server to connect to
is simple and easy to change through the polished UI. We can even see an estimate of the current load on each server so
we can select one likely to be more performant.

[DD-WRT](https://dd-wrt.com/)‘s OpenVPN client requires either an IP address or a URL to which it will connect. For
example, we can specify the IP address of their server named “US-TX#3” to connect to a specific premium server in Texas,
or we can specify “`us.protonvpn.com`” to connect to any server based in the U.S. Whichever server or URL we select is
sticky because it applies to our entire household and logging on to the router’s admin panel to change it is not
frictionless.

Unfortunately, I have found that setting a single static server to connect to from our router does not hold up long-term
with any VPN provider. Servers go offline for maintenance, have traffic spikes, get temporarily blacklisted by popular
services, and offer varying levels of latency at any given time. Shuffling exit nodes might even boost our privacy as
sites we visit see us coming from different IP addresses rather than the same one all of the time. Since I do not want
to manually log on to my router often to swap out IP addresses, I would prefer to just connect to something like
“`us.protonvpn.com`” and have ProtonVPN select a quality server for me.

That US URL, however, includes all servers in the US. Ideally, I would like to filter out the basic/free servers (more
heavily loaded) and the servers in cities that are far away from my location (higher latency).

## Targeting Optimal Servers via DNS

It does not seem like ProtonVPN has plans to provide more granular URLs that target all servers in a specific city or
only premium servers (or combinations of the two). So far, it seems they are limiting options to only country-based URLs
or server-specific IP addresses.

<div class="wp-block-image">
    <figure class="alignleft size-medium">[![Cloudflare DNS A Records for
        ProtonVPN](/assets/img/protonVpnCloudflareDns_collinmbarrett-300x98.jpg)](/assets/img/protonVpnCloudflareDns_collinmbarrett.jpg)
        <figcaption>Cloudflare DNS records</figcaption>
    </figure>
</div>Since I live in Memphis, the geographically closest city with ProtonVPN servers is Atlanta. So, I collected the IP
addresses of all of the premium servers in Atlanta (9 at the time of publishing) and created DNS A records for the same
subdomain on a domain I own at my DNS provider (Cloudflare). In DD-WRT, I have then configured the OpenVPN client
connection to this new custom URL. Every time the client connects, Cloudflare DNS serves it a different IP address via
[round-robin](https://www.cloudflare.com/learning/dns/glossary/round-robin-dns/).

DD-WRT provides a watchdog feature that is handy for addressing when a VPN server goes down. Every 300 seconds, DD-WRT
pings the ProtonVPN DNS server (only accessible via a ProtonVPN connection). If that ping fails, the router reboots
itself automatically. After that reboot, the OpenVPN client requests a new server IP address from DNS. We also have the
router configured to reboot automatically every night to refresh its connection and keep shuffling the ProtonVPN exit
node to which we are connected. During that downtime, we have a firewall configured on the router so that WAN
connections are blocked until the tunnel is re-established after the power cycle.

This solution has worked well for us for several months. We have the cheapest Xfinity connection and can stream a fair
bit of video and maintain work teleconferences without any issues.

## Next Steps

It would be nice if DNS would serve up the IP address of the server with the lowest current load or with the lowest
latency, rather than merely the next up in the round-robin rotation. Doing this would require standing up some kind of
custom DNS server with the capability to measure and decide which server meets that criteria. This is probably something
I would never invest the time in building unless there is an off-the-shelf tool that could do it?

Similarly, it would be great if DD-WRT could automatically detect when latency drops below a specified threshold at
which point it cycles to another IP address. I am not sure of any way of accomplishing this either, at the moment.