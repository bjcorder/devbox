# kasm-devbox
Kasm container for IaC and development work, based on Ubuntu Core maintained by KASM Tech

---

## Use this Image in Kasmweb

Please insert the following argument in the Kasm web `Docker Run Config Override (JSON)` to allow the container to establish the VPN connection.

```
{"cap_add":["NET_ADMIN"],"devices":["dev/net/tun","/dev/net/tun"],"sysctls":{"net.ipv6.conf.all.disable_ipv6":"0"}}
