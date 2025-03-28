# Aegis
Encrypted DNS server and stub resolver

This project has two main goals:
- a DNS server, being a sort-of combination of BIND/Unbound and Pi-Hole/AdGuard DNS, that features blacklists and encrypted DNS
- a Linux/Unix stub resolver, similar to systemd-resolved, that does standard and encrypted DNS lookups

They're packed into a single project, because a recursive DNS server needs to have some kind of resolution implemented anyway. So it's a single application where switching between a standalone DNS server and stub resolver for Linux is just a matter of configuration.

## DNS-Server
A simple overview of what Aegis will listen on and how it will be composed:

| Service | Protocols | Port | RFC |
| ------- | --------- | ---- | --- |
| DNS | DNS + UDP/TCP | 53/udp + 53/tcp | RFC 1035 |
| DNS over TLS | DNS + TLS + TCP | 853/tcp | RFC 7858 |
| DNS over HTTPS | HTTP/2 + TLS + TCP | 443/tcp | RFC 8484 |
| DNS over QUIC | DNS + QUIC | 853/udp | RFC 9250 |
| DNS over HTTPS3 | HTTP/2 + QUIC | 443/udp | - |

If you want to host a HTTPS server alongside, you need to use something like nginx, so that at least the `/dns-query`-path will be available.
An nginx config snippet will be available as soon as development is finished.

## DNS-Resolver
The resolver will do everything that's DNS and encrypted DNS. However, it is currently NOT planned to implement mDNS and certainly not LLMNR. mDNS can simultaneously be handled by systemd-resolved or Avahi, which also support DNS-SD.

What's missing: how the stub resolver and the given network manager interact with each other, related to DHCP and SLAAC. Either through shameless config file parsings or D-Bus, apparently.