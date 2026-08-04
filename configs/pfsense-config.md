
# Configuration pfSense

## 1. Installation (GNS3)

1. `File > New Appliance` → rechercher `pfsense` → `Install` ;
2. Importer les fichiers ISO + QCOW2 (même version) ;
3. Démarrer, accepter la licence ;
4. `Should VLANs be set up now [y/n]?` → **`n`** (configuré via l'UI web ensuite) ;
5. Confirmer l'interface WAN (`em0`) → `Y`.

## 2. Accès à l'interface web

- IP initiale : `192.168.1.1/24` ;
- Connecter un poste sur la même plage → `https://192.168.1.1` ;
- Identifiants : `admin` / mot de passe changé à la première connexion.

## 3. Création des VLANs (`Interfaces > VLANs`)

| Champ | Exemple (VLAN 10) |
|---|---|
| Parent Interface | em2 |
| VLAN Tag | 10 |
| Description | IT |

## 4. Affectation à une interface logique

`Interfaces > Assignments > ADD` → sélectionner `VLAN 10 on em2 - opt4 (IT)`.

## 5. Activation et adressage

| Interface | Adresse |
|---|---|
| IT | 192.168.10.1/24 |
| STAFF | 192.168.20.1/24 |
| INVITE | 192.168.30.1/24 |
| SERVER | 192.168.40.1/24 |

## 6. Serveur DHCP (`Services > DHCP Server`)

Exemple pour IT :

| Champ | Valeur |
|---|---|
| Subnet | 192.168.10.0 |
| Subnet mask | 255.255.255.0 |
| Range | 192.168.10.10 → 192.168.10.200 |
| Gateway | 192.168.10.1 |

À répéter pour STAFF, INVITE, SERVER.

---

## 7. Portail captif (`Services > Captive Portal`)

### a) Créer une zone

| Champ | Valeur |
|---|---|
| Zone name | PERSONNEL |
| Description | PORTAIL CAPTIF POUR LE PERSONNEL |

### b) Configurer la zone

| Champ | Valeur |
|---|---|
| Interfaces | STAFF, INVITE |
| Idle timeout | 120 min |
| After authentication Redirection URL | http://www.google.com |
| Authentication Server | server (LDAP) / Local Database |

### c) Portail invités (base locale)

**Groupe** (`System > User Manager > Groups`) :
- Name: `INVITE`, Scope: Local
- Privilège ajouté : *"User - Services: Captive Portal login"*

**Compte** (`System > User Manager > Users`) :
- Username: `MJLDH`, Group membership: `INVITE`

---

## 8. PfBlockerNG

### Installation
`System > Package Manager > Available Packages` → `PfBlockerNG` (version `devel`) → `Install`.

### Activation (`Firewall > pfBlockerNG > General`)
- Enable pfBlockerNG ✅
- Keep Settings ✅
- CRON Settings: every 8 hours

### Flux DNSBL (`DNSBL > DNSBL Groups`)

| Champ | Valeur |
|---|---|
| Name | SOCIAL |
| Description | BLOCAGE RÉSEAUX SOCIAUX ET SITE ADULTE |

### Domaines personnalisés (`DNSBL Custom_List`)

```
www.cpasbien.ph/
www.sortiesdvd.com/
www.cine974.com/streaming/
```

### Application
`DNSBL > Update` → cocher `Update` → `Run`.

---
[⬅ Config switch](switch-config.md) | [Retour au README](../README.md)
