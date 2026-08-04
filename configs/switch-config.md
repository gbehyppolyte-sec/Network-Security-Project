# Configuration du Switch Cisco

## 1. Sécurisation basique

```
Switch>en
Switch#conf t
Switch(config)#enable password MJL
Switch(config)#do wr
```

## 2. Création des VLANs

```
Switch(config)#vlan 10
Switch(config-vlan)#name IT
Switch(config-vlan)#exit
Switch(config)#vlan 20
Switch(config-vlan)#name STAFF
Switch(config-vlan)#exit
Switch(config)#vlan 30
Switch(config-vlan)#name INVITE
Switch(config-vlan)#exit
Switch(config)#vlan 40
Switch(config-vlan)#name SERVER
Switch(config-vlan)#exit
Switch(config)#do wr
```

## 3. Affectation des ports aux VLANs

```
! VLAN 10
Switch(config)#interface range gigabitEthernet 0/1-3
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 10

! VLAN 20
Switch(config)#interface range gigabitEthernet 1/0-3
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 20

! VLAN 30
Switch(config)#interface range gigabitEthernet 2/0-3
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 30

! VLAN 40 (serveur)
Switch(config)#interface gigabitEthernet 3/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 40
```

## 4. Lien trunk vers pfSense (802.1Q)

```
Switch(config)#interface gigabitEthernet 0/0
Switch(config-if)#switchport trunk encapsulation dot1q
Switch(config-if)#switchport mode trunk
```

## Récapitulatif

| ID_VLAN | Nom | Adressage | Ports |
|---|---|---|---|
| 10 | IT | 192.168.10.0/24 | Gi0/1 – Gi0/3 |
| 20 | STAFF | 192.168.20.0/24 | Gi1/0 – Gi1/3 |
| 30 | INVITE | 192.168.30.0/24 | Gi2/0 – Gi2/3 |
| 40 | SERVER | 192.168.40.0/24 | Gi3/2 |
| — | Trunk → pfSense | — | Gi0/0 |

---

[⬅ Retour au README](../README.md) | [Config pfSense ➡](pfsense-config.md)
