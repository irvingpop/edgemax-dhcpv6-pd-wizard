edgemax-dhcpv6-pd-wizard
========================

Feature Wizard for DHCPv6-PD on Ubiquity EdgeMax Routers. Tested and validated on my home Comcast IPv6 environment.

Development status
==================
WIP: I'm figuring out how to make an EdgeMax feature wizard for the first time, borrowed heavily from the UPnP example.
Instructions here

Usage
=====
Known to work with 1.4.x and 1.5.x firmware

1. Apply the following configuration to your router to configure radvd (in this example, WAN=eth0,  LAN=br0)
  ```
  set interfaces bridge br0 ipv6 dup-addr-detect-transmits 1
  set interfaces bridge br0 ipv6 router-advert cur-hop-limit 64
  set interfaces bridge br0 ipv6 router-advert default-preference high
  set interfaces bridge br0 ipv6 router-advert link-mtu 0
  set interfaces bridge br0 ipv6 router-advert managed-flag true
  set interfaces bridge br0 ipv6 router-advert max-interval 600
  set interfaces bridge br0 ipv6 router-advert other-config-flag true
  set interfaces bridge br0 ipv6 router-advert prefix '::/64' autonomous-flag true
  set interfaces bridge br0 ipv6 router-advert prefix '::/64' on-link-flag true
  set interfaces bridge br0 ipv6 router-advert prefix '::/64' valid-lifetime 2592000
  set interfaces bridge br0 ipv6 router-advert reachable-time 0
  set interfaces bridge br0 ipv6 router-advert retrans-timer 0
  set interfaces bridge br0 ipv6 router-advert send-advert true
  set interfaces ethernet eth0 ipv6 dup-addr-detect-transmits 1
  set protocols static interface-route6 '::/0' next-hop-interface eth0
  ```

2. Install these files into `/var/www/wizard/feature/wide-dhcpv6-client` on your EdgeMax device.
  ```
  /var/www/wizard/feature/wide-dhcpv6-client/validator.json
  /var/www/wizard/feature/wide-dhcpv6-client/wizard-run
  /var/www/wizard/feature/wide-dhcpv6-client/wizard.html
  ```

TODO
====
* Handling of multiple Prefix Delegations
* Figure out better way to persist this data and installation across firmware upgrades
