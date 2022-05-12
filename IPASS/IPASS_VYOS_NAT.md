# VYOS NAT

Last edited time: May 11, 2022 3:24 PM
Reviewed: No


als je een NAT interface aan je router hebt toegevoegd kan je misschien wel 1.1.1.1 pingen vanaf je router, maar niet vanaf je clients in het LAN, daarvoor moet je NAT inregelen.

1. specificeer de WAN interface (NAT kaart) en zeg dat deze de buitenkant van het netwerk is (nat outside) in dit geval eth3
`set nat source rule 100 outbound-interface eth3`
2. specificeer het WAN ip-address wat aan de WAN interface gegeven is. in dit geval 192.168.1.82
`set nat source rule 100 translation address 192.168.1.82`
3. maak een default static route aan naar het internet zodat al het verkeer (wat niet matched aan je rip/ospf regels) naar het internet kan. in dit geval is de next-hop 192.168.1.1
`set protocols static route 0.0.0.0/0 next-hop 192.168.1.1`
4. specificeer optioneel een dns server
`set system name-server 192.168.1.1`