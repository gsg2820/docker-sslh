SSLH - a protocol multiplexer in a docker container
===================================================

`sslh` accepts connections on specified ports, and forwards
them further based on tests performed on the first data
packet sent by the remote client.

Probes for HTTP, TLS/SSL (including SNI and ALPN), SSH,
OpenVPN, tinc, XMPP, SOCKS5, are implemented, and any other
protocol that can be tested using a regular expression, can
be recognised. A typical use case is to allow serving
several services on port 443 (e.g. to connect to SSH from
inside a corporate firewall, which almost never block port
443) while still serving HTTPS on that port. 

Hence `sslh` acts as a protocol demultiplexer, or a
switchboard. With the SNI and ALPN probe, it makes a good
front-end to a virtual host farm hosted behind a single IP
address.

`sslh` has the bells and whistles expected from a mature
daemon: privilege and capabilities dropping, inetd support,
systemd support, transparent proxying,
chroot, logging, IPv4 and IPv6, a fork-based and a
select-based model, and more.


Check more at: https://github.com/yrutschle/sslh

Available tags
--------------

https://quay.io/repository/riotkit/sslh?tab=tags

```bash
docker pull quay.io/riotkit/sslh:1.20-r3
```

Example usage
-------------

Please note the `network_mode` - in standard networking SSLH freezes on incoming connection.

```yaml
version: "3.4"

services:
    sslh:
        image: quay.io/riotkit/sslh:1.20-r3
        ports:
            - 4443:4443
        cap_add:
            - cap_net_admin
        network_mode: host
        environment:
            HTTP_TARGET: "127.0.0.1:8000"
            SSL_TARGET: "127.0.0.1:443"
            #SSL_TARGET: "{{ host }}:443" # example: {{ host }} will be automatically replaced with a gateway IP address
            #VPN_TARGET: ""
            #HTTP_TARGET: ""
            #SOCKS_TARGET: ""
            #ADDITIONAL_ARGS: "-v"

```
