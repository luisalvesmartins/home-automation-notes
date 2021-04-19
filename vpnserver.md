# Creating the VPN Server and client

1. Create the dynamic DNS record and configure it in the Unifi Controller
  - goto www.duckdns.org create the domain and copy the token
  - in Unifi Controller goto services->Dynamic DNS, create a new "dyndns
    - username: noone
    - password: the duckdns token
    - server: www.duckdns.org"
2. Create Radius Server
  - Services -> Radius -> Server, create secret (refered as SECRET)
3. Create Radius User
  - Services -> Radius -> Users, create user (USER) with password (PASSWORD)
4. Create Network
  - Networks -> Create New Network
    - Pre-Shared Key is SECRET
    - VPN Type: L2TP Server
    - Gateways IP/Subnet: 192.168.3.1/24 (no collision with existing)
5. Open Ports on the internet router
  - Forward UDP ports 500 and 4500 to the NSG IP address
6. Configure Client
  - Create VPN of type L2TP
    - server: the dns url
    - user: USER
    - password: PASSWORD
    - secret: SECRET
    - Send all traffic: on
