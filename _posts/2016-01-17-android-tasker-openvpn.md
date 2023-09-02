---
id: 744
title: 'Automatic VPN on Android with Tasker and OpenVPN'
date: '2016-01-17T16:24:31-06:00'
author: 'Collin M. Barrett'
excerpt: 'How to automatically connect to VPN on Android using Tasker and OpenVPN Connect based on the network to which you are connected.'
layout: post
guid: '/?p=744'
permalink: /android-tasker-openvpn/
wp_featherlight_disable:
    - ''
image: /assets/img/androidTaskerOpenVpn_collinmbarrett.jpg
categories:
    - InfoSec
tags:
    - Android
    - Apple
    - Encryption
    - Google
    - HTML
    - ISP
    - NSA
    - Performance
    - Router
    - Tracking
    - Travel
    - VPN
    - Wi-Fi
---

In the Barrett house, our wireless router is running [DD-WRT](https://dd-wrt.com/ "DD-WRT") with the OpenVPN client to a 3rd-party VPN provider always enabled. To hinder tracking by ISPs and the NSA, I try always to connect to a VPN on Android. When I enable it while connected to my home network, however, I notice a performance hit because I am recursively tunneling through a tunnel already being created by our wireless router. Using [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm&hl=en "Tasker") (root is *not* required) in conjunction with [OpenVPN Connect](https://play.google.com/store/apps/details?id=net.openvpn.openvpn&hl=en "OpenVPN Connect"), I have automated the connection and disconnection of the VPN service on my Android device. However, I do not take the extra performance overhead of the second connection when I am at home. Here is how you can automatically connect to VPN on Android using this same strategy.

## Assumptions

You must have a subscription to an OpenVPN-compatible service. You should configure OpenVPN Connect on your Android phone per the instructions provided by your VPN service. You must also purchase and install Tasker before continuing.

I am not sure why, but it seems that Tasker must also have the Location Android permission enabled for the profiles to be triggered correctly.

## Tasker Tasks

Configure the following two tasks with one action each in Tasker. <sup>1</sup>Note: Items in curly brackets {} should be treated as variables and replaced.

### “VPN Connect” Task

Add action type “System”-&gt;“Send Intent” and configure as follows.

- Action: net.openvpn.openvpn.CONNECT
- Cat: None
- Mime Type: {blank}
- Data: {blank}
- Extra: net.openvpn.openvpn.AUTOSTART\_PROFILE\_NAME:PC {your\_profile\_name}
- Extra: net.openvpn.openvpn.AUTOCONNECT:true
- Extra: net.openvpn.openvpn.APP\_SECTION:PC
- Package: net.openvpn.openvpn
- Class: net.openvpn.unified.MainActivity
- Target: Activity
- Continue Task After Error: {unchecked}
- If: {blank}
- Label: {blank}

### “VPN Disconnect” Task

Add action type “System”-&gt;“Send Intent” and configure as follows.

- Action: net.openvpn.openvpn.DISCONNECT
- Cat: None
- Mime Type: {blank}
- Data: {blank}
- Extra: net.openvpn.openvpn.STOP:true
- Extra: {blank}
- Extra: {blank}
- Package: net.openvpn.openvpn
- Class: net.openvpn.unified.MainActivity
- Target: Activity
- Continue Task After Error: {unchecked}
- If: {blank}
- Label: {blank}

## Tasker Profiles

Configure the following two profiles in Tasker and link them to the task specified.

### “Wi-Fi Disconnected” State Profile

Add profile and set as follows. Link this profile to the “VPN Connect” task.

- SSID: {blank}
- MAC: {blank}
- IP: {blank}
- Active: Yes
- Inverted: {checked}

### “Wi-Fi Connected” State Profile

Add profile and configure as follows. Link this profile to the “VPN Disconnect” task.

- SSID: {blank}
- MAC: {blank}
- IP: {blank}
- Active: Yes
- Inverted: {unchecked}

*via [levgen](/android-tasker-openvpn/#comment-220)*