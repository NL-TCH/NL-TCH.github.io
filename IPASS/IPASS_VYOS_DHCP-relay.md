# DHCP relay

Last edited time: May 11, 2022 3:24 PM
Reviewed: No


1. zet een dhcp relay op de interne interfaces die de broadcasts ontvangen+ op de wan interfaces die de broadcasts door moeten sturen
`set service dhcp-relay interface <interface bijv eth0>`
2. zet optioneel de dhcp server statisch in de router
`set service dhcp-relay server <dhcp server ip>`
3. doe dit op alle routers waar de dhcp-broadcasts langs komen