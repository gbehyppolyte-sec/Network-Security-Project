
# Résultats

## 1. Test de connectivité entre VLANs

**Objectif :** vérifier que les VLANs ne communiquent pas entre eux par défaut.

| Test | Commande | Résultat |
|---|---|---|
| VLAN 10 → VLAN 20 | `ping 192.168.20.10` | ❌ Échec (attendu) |
| VLAN 10 → VLAN 30 | `ping 192.168.30.10` | ❌ Échec (attendu) |

➡️ Isolation entre VLANs confirmée.

---

## 2. Test du portail captif

| VLAN | Comportement observé |
|---|---|
| VLAN 10 (IT) | Accès direct à Internet, sans authentification |
| VLAN 20/30 (STAFF/INVITE) | Redirection automatique vers la page du portail captif — champs `Identifiant` / `Mot de passe` requis |

➡️ L'authentification obligatoire fonctionne comme prévu pour STAFF et INVITE.

---

## 3. Optimisation de la bande passante

### Résolution DNS de Facebook (bloqué)

```bash
$ nslookup www.facebook.com
Server:   192.168.30.1
Address:  192.168.30.1

Name:     www.facebook.com
Address:  10.10.10.1   # adresse virtuelle renvoyée par PfBlockerNG
```

➡️ La résolution réelle est interceptée par PfBlockerNG.

### Accès à un site de streaming bloqué

1. Avertissement de sécurité (certificat non fiable, trafic intercepté) ;
2. Page de blocage DNSBL :

```
This website [ www.cine974.com ] has been blocked by the Network Administrator!

Referer   : https://www.google.com/search?safe=strict&q=cine974
Client    : 192.168.30.10
User Agent: Mozilla/5.0 (X11; Linux i686; rv:33.0) Gecko/20100101 Firefox/33.0

Powered by: pfBlockerNG DNSBL
```

➡️ Blocage effectif et journalisé (IP client, référent, user-agent).

---

## 4. Difficultés rencontrées

| Difficulté | Détail |
|---|---|
| Configuration GNS3 | Paramétrage post-installation |
| Configuration pfSense | Interfaces logiques, VLAN tagging |
| Connexion machine hôte ↔ machines GNS3 | Établissement de la connectivité simulée |

---

