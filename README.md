# 🔐 Sécurisation du réseau local et optimisation de la bande passante — MJL

[![Statut](https://img.shields.io/badge/statut-terminé-brightgreen)]()
[![Option](https://img.shields.io/badge/option-Sécurité%20Informatique-orange)]()
[![Année](https://img.shields.io/badge/année-2019--2020-lightgrey)]()

> Projet réseau réalisé dans le cadre d'un mémoire de Licence en Informatique (IFRI, Université d'Abomey-Calavi, Bénin).
> Segmentation VLAN + portail captif + optimisation de bande passante pour le réseau du **Ministère de la Justice et de la Législation (MJL)**.

**Auteur :** Hyppolyte Sunday GBEDO


---

## 📋 Sommaire

- [Contexte](#-contexte)
- [Architecture](#-architecture)
- [Stack technique](#-stack-technique)
- [Structure du dépôt](#-structure-du-dépôt)
- [Mise en œuvre](#-mise-en-œuvre)
- [Résultats](#-résultats)
- [Perspectives](#-perspectives)

---

## 🎯 Contexte

Le réseau du MJL n'était pas segmenté (un seul domaine de diffusion) et le Wi-Fi était accessible sans authentification. Ce projet met en place :

- une **segmentation VLAN** par département,
- un **portail captif** pour authentifier les utilisateurs avant l'accès Internet,
- un **filtrage de bande passante** (blocage des sites de streaming/réseaux sociaux consommant les ressources).

---

## 🏗️ Architecture

| VLAN ID | Nom | Adressage |
|---|---|---|
| 10 | IT (Informatique) | 192.168.10.0/24 |
| 20 | STAFF (Personnel) | 192.168.20.0/24 |
| 30 | INVITE | 192.168.30.0/24 |
| 40 | SERVEUR | 192.168.40.0/24 |

```
                      INTERNET
                         │
                 ┌───────┴────────┐
                 │  ROUTER/FIREWALL │  (pfSense)
                 └───────┬────────┘
                         │
                 ┌───────┴────────┐
        ┌────────┤   SwitchCore    ├─────────┐
        │        └───┬─────┬───────┘         │
   ACCESSPOINT    VLAN10  VLAN20  VLAN30   SERVER/LDAP
   (Wi-Fi)          IT    STAFF   INVITE   (VLAN 40)
```

Le VLAN **IT** accède directement à Internet (rôle admin). Les VLANs **STAFF** et **INVITE** passent obligatoirement par le portail captif.

---

## 🛠️ Stack technique

| Outil | Rôle |
|---|---|
| **VLAN (802.1Q)** | Segmentation logique du réseau |
| **pfSense** | Routeur/pare-feu, portail captif, serveur DHCP |
| **PfBlockerNG** | Filtrage DNS, blocage de sites |
| **GNS3** | Simulation de l'architecture |
| **VMware** | Virtualisation |
| **Cisco IOS** | Configuration switch (VLAN, trunk) |
| **LDAP** | Authentification centralisée du personnel |

---

## 📁 Structure du dépôt

```
MJL-Network-Security-Project/
├── README.md
├── docs/
│   ├── 01-implementation.md    # Mise en œuvre complète (switch + pfSense + portail captif + PfBlockerNG)
│   └── 02-results.md           # Tests et résultats
├── configs/
│   ├── switch-config.md        # Commandes CLI Cisco
│   └── pfsense-config.md       # Étapes de configuration pfSense
└── assets/images/              # Captures d'écran (à compléter)
```

---

## 🚀 Mise en œuvre

Le détail complet des étapes est dans [`docs/01-implementation.md`](docs/01-implementation.md), avec les commandes exactes dans [`configs/`](configs/).

**Résumé des étapes :**
1. Sécurisation et création des VLANs sur le switch Cisco ;
2. Affectation des ports + configuration du lien trunk (802.1Q) vers pfSense ;
3. Installation de pfSense (GNS3) et déclaration des interfaces VLAN ;
4. Configuration du serveur DHCP par VLAN ;
5. Mise en place du portail captif (LDAP pour le personnel, base locale pour les invités) ;
6. Installation de PfBlockerNG et blocage de domaines (streaming, réseaux sociaux).

---

## ✅ Résultats

- Les VLANs sont **isolés entre eux** (ping inter-VLAN → échec, comme attendu) ;
- Les utilisateurs STAFF/INVITE sont **redirigés vers le portail captif** avant tout accès Internet ;
- Les sites bloqués (Facebook, sites de streaming) sont **inaccessibles**, interceptés par PfBlockerNG.

Détails et captures : [`docs/02-results.md`](docs/02-results.md)

---

## 🔭 Perspectives

- Authentification forte (MFA) sur le portail captif ;
- Blocage de sites selon un calendrier horaire ;
- Ajout de DLP, DMZ, VPN, IPS pour renforcer la sécurité globale.

---

<p align="center">
  <em>Hyppolyte Sunday GBEDO — IFRI, Université d'Abomey-Calavi 🇧🇯</em>
</p>
