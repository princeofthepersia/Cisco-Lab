# Lab10_Dynamic_Routing_RIP - Configuration d’un routage dynamique grâce à RIP

## Objectif  
Configurer un réseau avec 3 LAN reliés par 3 routeurs Cisco via RIP. Chaque LAN a un PC en DHCP.

## Topologie
Routeur	LAN IP	Interface LAN	Interfaces séries
- R1	192.168.1.0/24	GigabitEthernet0/0	Serial0/0/0 (192.168.10.1/30), Serial0/1/1 (192.168.30.1/30)
- R2	192.168.2.0/24	GigabitEthernet0/0	Serial0/0/0 (192.168.10.2/30), Serial0/0/1 (192.168.20.1/30)
- R3	192.168.3.0/24	GigabitEthernet0/0	Serial0/0/1 (192.168.20.2/30), Serial0/1/1 (192.168.30.2/30)

## Configuration DHCP
### R1
```bash
ip dhcp excluded-address 192.168.1.1
ip dhcp pool LAN1
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1

```
### R2
```bash
ip dhcp excluded-address 192.168.2.1
ip dhcp pool LAN2
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
```

### R3
```bash
ip dhcp pool LAN3
 network 192.168.3.0 255.255.255.0
 default-router 192.168.3.1
```
## Configuration des interfaces

### R1
```bash
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
interface Serial0/0/0
 ip address 192.168.10.1 255.255.255.252
 no shutdown
interface Serial0/1/1
 ip address 192.168.30.1 255.255.255.252
 no shutdown
```

### R2
```bash
interface GigabitEthernet0/0
 ip address 192.168.2.1 255.255.255.0
 no shutdown
interface Serial0/0/0
 ip address 192.168.10.2 255.255.255.252
 no shutdown
interface Serial0/0/1
 ip address 192.168.20.1 255.255.255.252
 no shutdown
```

### R3
```bash
interface GigabitEthernet0/0
 ip address 192.168.3.1 255.255.255.0
 no shutdown
interface Serial0/0/1
 ip address 192.168.20.2 255.255.255.252
 no shutdown
interface Serial0/1/1
 ip address 192.168.30.2 255.255.255.252
 no shutdown
```

## Configuration de RIP
### R1
```bash
router rip
 version 2
 no auto-summary
 network 192.168.1.0
 network 192.168.10.0
 network 192.168.30.0
```

### R2
```bash
router rip
 version 2
 no auto-summary
 network 192.168.2.0
 network 192.168.10.0
 network 192.168.20.0
 ```

### R3
```bash
router rip
 version 2
 no auto-summary
 network 192.168.3.0
 network 192.168.20.0
 network 192.168.30.0
```

## Vérifications
- Vérifier que les PCs ont une IP via DHCP
- Ping inter-LAN (ex: PC R1 → PC R2, R3)
- show ip route rip sur chaque routeur
- show ip protocols pour voir RIP actif
<img width="261" alt="image" src="https://github.com/user-attachments/assets/544fa3fd-9f63-4b8b-8c80-a2f2a8c233ab" />
<img width="261" alt="image" src="https://github.com/user-attachments/assets/706f201a-8805-4941-aa95-a5891a977149" />
<img width="315" alt="image" src="https://github.com/user-attachments/assets/12255458-b46b-41c0-842b-8e3bf84bef80" />


