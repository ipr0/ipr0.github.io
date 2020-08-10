---
layout: post
title: Load Balancer
tags: network 
categories:
 - network
---
### Load balancers

Subj are typically thought as black boxes of magic. While pure hardware LBs are rigid, implementing them at scale can be challenging.

Small scale solutions are typically built on:
*  [haproxy](http://www.haproxy.org/)
*  [Apache mod_proxy_balancer](https://httpd.apache.org/docs/2.4/mod/mod_proxy_balancer.html)
*  [nginx](https://www.nginx.com/)

Interesting is the history of verical scaling, which boils down to [C10M problem](http://c10m.robertgraham.com/p/manifesto.html)

Horizontal LB scalig solution was implemented by Google as [Project Maglev](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/44824.pdf)
