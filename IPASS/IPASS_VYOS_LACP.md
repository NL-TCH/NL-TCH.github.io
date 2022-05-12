# LACP

Last edited time: May 11, 2022 3:24 PM
Reviewed: No


1. maak een interface aan genaamd bond<0-255> en geeft een ip adress
`set interfaces bond bond0 address 172.16.1.1/29`
2. plaats de fysieke netwerkkaarten als een member van de bond0 interface
`set interfaces bonding bond0 member interface eth3`
`set interfaces bonding bond0 member interface eth4`
3. zet de bond modus op 1 van de volgende modi

| Mode |  |
| --- | --- |
| 802.3ad | IEEE 802.3ad Dynamic link aggregation (Default) |
| active-backup | Fault tolerant: only one slave in the bond is active |
| broadcast | Fault tolerant: transmits everything on all slave interfaces |
| round-robin | Load balance: transmit packets in sequential order |
| transmit-load-balance | Load balance: adapts based on transmit load and speed |
| adaptive-load-balance | Load balance: adapts based on transmit and receive plus ARP |
| xor-hash | Distribute based on MAC address |

`set interfaces bonding bond0 mode active-backup`

1. zet een primare interface zodat vyos weet welke van de 2 fysieke interfaces de backup moet zijn
`set interfaces bonding bond0 primary eth3`
2. geef een description voor de bond0 interface
`set interfaces bond bond0 description WAN`




