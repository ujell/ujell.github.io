---
title: Mirror Server on LAN with Windows 
date: 2023-11-06 09:00:00 +0200
categories: [Unity, Mirror]
tags: [Unity, Mirror, Network, Multiplayer]   
---
This is just a small reminder, but it cost me some time and was not documented well: if you are running a [Mirror](https://mirror-networking.gitbook.io/docs/) (or probably any other framework) based multiplayer server on your Windows computer, you should manually allow incoming connections for your server on [Windows Firewall settings](https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/create-an-inbound-program-or-service-rule).

This might be quite obvious for many, but since I mainly manage servers on Linux and focus more on Unity development, it was easy for me to miss.