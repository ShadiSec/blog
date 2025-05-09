# CGNAT

Created by: Shadi A
Created time: April 23, 2025 3:35 AM
Category: Networking

# Overview

While working on a personal project overseas using a cellular home router, I ran into an unexpected issue: I couldn't SSH into my laptop from my tablet, which was on a different cellular network. Every attempt to connect or ping the laptop resulted in dropped packets. At first, I assumed it was a misconfiguration on my end, so I tried setting up a port forward rule to allow inbound SSH traffic.

That’s when I discovered the problem wasn’t with my settings—it was with the network itself. I didn’t have control over a public IP address. My ISP was using Carrier-Grade NAT (CGNAT), which blocks unsolicited inbound traffic. This post explores how CGNAT works, why it limits remote access, and how I eventually worked around it using Tailscale.

# What is CGNAT?

**CGNAT (Carrier-Grade Network Address Translation)** is a technique used by Internet Service Providers (ISPs) to conserve public IPv4 addresses by allowing **multiple users to share a single public IP address**.

In simple terms, your device sits behind another layer of NAT—this time controlled by your provider, not just your home router. That means any inbound traffic (like SSH or server hosting) gets blocked unless you initiate the connection first. And because you're not in control of the public IP, port forwarding isn't an option.

By mapping internal private addresses to shared external ports, CGNAT makes it possible for hundreds or thousands of users to connect to the internet simultaneously—**without needing a unique public IP for each user**.

# Why Port Forwarding Breaks

With a regular home connection (Fiber or Cable), your router is assigned a dedicated public IP, allowing you to forward ports and host services like game servers and web apps. With CGNAT, however, you’re not assigned a true public IP address, so port forwarding becomes impossible without a workaround.

That’s because CGNAT only supports return traffic, meaning you need to initiate the connection first. Unsolicited inbound traffic will get dropped by the ISP’s NAT system because the entry does not exist in the NAT table.

![CGNAT Diagram](https://github.com/ShadiSec/Blog/blob/main/CGNAT/CGNAT-Diagram.png)

# Solutions

If you’re stuck behind a CGNAT, there are a few workarounds, but Tailscale is one of the easiest and most effective ones. Oh, and it’s FREE (for general use that is :).

Tailscale is a VPN solution that lets your devices communicate with each other over an encrypted channel without needing to mess with port forwarding or firewall rules.

**Why it works:**

One installed on both devices, Tailscale can initiate outbound connections from each device to one of its coordination servers. Since CGNAT allows outbound traffic, the connection between devices succeeds and lets Tailscale route traffic between the devices directly (via peer-to-peer) or through a relay server if it can’t establish a direct connection.
