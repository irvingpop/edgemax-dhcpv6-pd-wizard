edgemax-dhcpv6-pd-wizard
========================

Feature Wizard for DHCPv6-PD on Ubiquiti EdgeMax Routers. Tested and validated on my home Comcast IPv6 environment.

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

2. Download and install the feature wizard

#### End user installation:
* Download the wizard .tar file: https://github.com/irvingpop/edgemax-dhcpv6-pd-wizard/releases/download/v0.0.1/edgemax-dhcpv6-pd-wizard-0.0.1.tar
* Uncompress it (`gzip -d`) so that it simply `master.tar`
* upload `master.tar` to https://your-router/#Wizard (click on the + symbol next to Feature Wizards in the left-hand pane)

#### Developer installation:
Git clone and scp these files into `/var/www/wizard/feature/wide-dhcpv6-client` on your EdgeMax device.

  ```
  /var/www/wizard/feature/wide-dhcpv6-client/validator.json
  /var/www/wizard/feature/wide-dhcpv6-client/wizard-run
  /var/www/wizard/feature/wide-dhcpv6-client/wizard.html
  ```

#### Settings:
* Internal interface:  Your LAN interface (currently only one supported)
* External interface:  Your WAN interface (currently only one supported)


TODO
====
* Handling of multiple Prefix Delegations
* Better integration with UBNT/Vayatta system (Persist this data and installation across firmware upgrades, CLI integration, etc)
* IPv6 status reporting in the Wizard UI
* Automatic detection/setting of router-advert settings
* Automatic detection/setting of non-foot-shooting IPv6 LOCAL Firewall rules.  Example:

  ```
  set firewall ipv6-name WAN6_LOCAL default-action drop
  set firewall ipv6-name WAN6_LOCAL description 'IPv6 Internet to router'
  set firewall ipv6-name WAN6_LOCAL enable-default-log
  set firewall ipv6-name WAN6_LOCAL rule 1 action accept
  set firewall ipv6-name WAN6_LOCAL rule 1 state established enable
  set firewall ipv6-name WAN6_LOCAL rule 1 state related enable
  set firewall ipv6-name WAN6_LOCAL rule 2 action accept
  set firewall ipv6-name WAN6_LOCAL rule 2 description ICMPv6
  set firewall ipv6-name WAN6_LOCAL rule 2 limit burst 1
  set firewall ipv6-name WAN6_LOCAL rule 2 protocol icmpv6
  set firewall ipv6-name WAN6_LOCAL rule 3 action accept
  set firewall ipv6-name WAN6_LOCAL rule 3 description 'for DHCPv6'
  set firewall ipv6-name WAN6_LOCAL rule 3 destination port 546-547
  set firewall ipv6-name WAN6_LOCAL rule 3 protocol udp
  set firewall ipv6-name WAN6_LOCAL rule 100 action drop
  set firewall ipv6-name WAN6_LOCAL rule 100 log enable
  set firewall ipv6-name WAN6_LOCAL rule 100 state invalid enable
  ```
