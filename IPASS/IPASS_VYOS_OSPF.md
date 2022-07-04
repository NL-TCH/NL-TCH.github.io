# OSPF

Last edited time: May 11, 2022 3:24 PM
Reviewed: No

1. zet alle WAN en LAN netwerken in 1 OSPF area (in dit geval area 10)
`set protocols ospf area 10 network 10.1.0.0/16`
`set protocols ospf area 10 network 10.2.0.0/16`
2. zet optioneel het router-id op de WAN interface
`set protocols ospf parameters router-id 172.16.1.1`
3. zeg dat OSPF direct verbonden netwerken moet adverteren
`set protocols ospf redistribute connected`
4. specificeer de OSPF neighbour
 `set protocols OSPF neighbor
5. verifieer de instelligen
`show ip route`
`show ip ospf neighbor`