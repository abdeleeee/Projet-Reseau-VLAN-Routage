# Configuration avancée : VLANs, EtherChannel (LACP) & Routage (Router-on-a-Stick) — Cisco Packet Tracer

Ce dépôt contient une simulation **Packet Tracer** d’une infrastructure réseau de type PME, mettant en pratique :
- la **segmentation** via **VLANs**,
- la **mise en trunk** et la sécurisation des trunks (VLAN natif + VLANs autorisés),
- l’agrégation de liens **EtherChannel** avec **LACP** (redondance + bande passante),
- le **routage Inter-VLAN** (Router-on-a-Stick),
- une **liaison WAN** et du **routage statique**.

> Le rapport détaillé (topologie, plan d’adressage, commandes IOS, validation) est disponible ici : :contentReference[oaicite:0]{index=0}

---

## 📌 Contexte & objectifs

Le scénario simule une PME qui souhaite :
- isoler ses départements pour des raisons de **sécurité** et de **performance** (VLANs),
- fiabiliser l’interconnexion entre commutateurs (EtherChannel),
- permettre une communication contrôlée entre VLANs (Inter-VLAN routing),
- connecter le site principal à un réseau externe via un lien WAN et des routes statiques.

---

## 🧱 Topologie (résumé)

Équipements typiques (Packet Tracer) :
- **SW1 & SW2** : 2 commutateurs (Cisco 2960 ou équivalent)
- **R1** : routeur “gateway” (Cisco 2911 ou équivalent) pour le **router-on-a-stick**
- **R2** : routeur distant simulant un site/WAN
- **PCs** répartis sur plusieurs VLANs pour les tests

Un schéma de la topologie est présenté dans le rapport (section *Analyse de la Topologie*). :contentReference[oaicite:1]{index=1}

---

## 🗂️ VLANs & plan d’adressage

| VLAN | Nom | Rôle | Réseau | Masque | Passerelle |
|------|-----|------|--------|--------|------------|
| 10 | DIRECTION | Département Direction | 192.168.10.0 | /24 | 192.168.10.254 |
| 20 | TECHNIQUE | Département Technique | 192.168.20.0 | /24 | 192.168.20.254 |
| 30 | COMMERCIAL | Département Commercial | 192.168.30.0 | /24 | 192.168.30.254 |
| 99 | NATIF_MGMT | VLAN natif (trafic non tagué / contrôle) | N/A | N/A | N/A |
| WAN | - | Liaison R1–R2 | 10.0.0.0 | /30 | - |

> Convention : la **dernière IP utilisable (.254)** est réservée à la passerelle des VLANs.

---

## ✅ Technologies implémentées

- **VLANs** (création, nommage, ports access)
- **Trunk 802.1Q** (VLAN natif 99 + liste VLANs autorisés)
- **EtherChannel (LACP)** entre SW1 et SW2
- **Router-on-a-Stick** sur R1 (sous-interfaces dot1Q)
- **Routage statique** entre R1 et R2 (WAN /30)

---

## ⚙️ Configuration (extraits)

### 1) Création des VLANs (SW1 & SW2)
```cisco
enable
configure terminal
vlan 10
 name DIRECTION
vlan 20
 name TECHNIQUE
vlan 30
 name COMMERCIAL
vlan 99
 name NATIF_MGMT
end

show vlan brief
2) Ports access (exemple SW1)
cisco
Copier le code
configure terminal
interface fastEthernet0/1
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
exit

interface fastEthernet0/2
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
exit
3) Trunk vers le routeur (SW1 → R1)
cisco
Copier le code
configure terminal
interface gigabitEthernet0/1
 description *** Lien vers Routeur R1 ***
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
end

show interfaces trunk
4) EtherChannel (LACP) entre SW1 et SW2
Exemple : Fa0/23 et Fa0/24 agrégés en Port-Channel 1.
cisco
Copier le code
configure terminal
interface range fastEthernet0/23 - 24
 description *** EtherChannel vers SW2 ***
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
 channel-group 1 mode active
exit

show etherchannel 1 summary
La même configuration est à appliquer côté SW2 sur les ports correspondants.
🧭 Routage (R1 & R2)
1) Router-on-a-Stick (R1)
cisco
Copier le code
enable
configure terminal
interface gigabitEthernet0/0
 no shutdown
exit

interface gigabitEthernet0/0.10
 description *** Gateway VLAN 10 (DIRECTION) ***
 encapsulation dot1Q 10
 ip address 192.168.10.254 255.255.255.0
exit

interface gigabitEthernet0/0.20
 description *** Gateway VLAN 20 (TECHNIQUE) ***
 encapsulation dot1Q 20
 ip address 192.168.20.254 255.255.255.0
exit

interface gigabitEthernet0/0.30
 description *** Gateway VLAN 30 (COMMERCIAL) ***
 encapsulation dot1Q 30
 ip address 192.168.30.254 255.255.255.0
exit

interface gigabitEthernet0/0.99
 description *** VLAN Natif ***
 encapsulation dot1Q 99 native
end

show ip interface brief
2) WAN /30 + routes statiques (R1 ↔ R2)
R1
cisco
Copier le code
configure terminal
interface serial0/0/0
 description *** Lien WAN vers R2 ***
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000
 no shutdown
exit

ip route 172.16.0.0 255.255.0.0 10.0.0.2
end

show ip route
R2
cisco
Copier le code
configure terminal
interface serial0/0/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown
exit

ip route 192.168.10.0 255.255.255.0 10.0.0.1
ip route 192.168.20.0 255.255.255.0 10.0.0.1
ip route 192.168.30.0 255.255.255.0 10.0.0.1
end

show ip route
🧪 Tests & validation
Depuis les PCs (commande ping) :
Intra-VLAN
Objectif : deux PCs du même VLAN communiquent (niveau 2).
Exemple : PC VLAN10 (192.168.10.10) -> ping 192.168.10.11
Inter-VLAN
Objectif : valider le routage via R1.
Exemple : PC VLAN10 -> ping PC VLAN20 (192.168.20.10)
Note : le premier ping peut échouer à cause de l’ARP (timeout initial).
WAN
Objectif : valider les routes statiques via la liaison R1–R2.
Exemple : PC VLAN10 -> ping 10.0.0.2
Commandes de vérification utiles :
cisco
Copier le code
show vlan brief
show interfaces trunk
show etherchannel summary
show ip interface brief
show ip route
🛠️ Dépannage rapide
Pas d’Inter-VLAN ?
Vérifier trunk SW1↔R1 (show interfaces trunk)
Vérifier encapsulation dot1Q et les IP des sous-interfaces sur R1
Vérifier que les VLANs sont bien autorisés sur le trunk (allowed vlan)
EtherChannel down / “suspended” ?
Vérifier que les ports des deux côtés ont la même config (mode trunk, native, allowed)
Vérifier LACP : show etherchannel summary
WAN KO ?
Vérifier no shutdown sur les interfaces série
Vérifier le clock rate côté DCE
Vérifier les routes statiques et le next-hop
📦 Contenu du dépôt (suggestion)
graphql
Copier le code
.
├── packeeet1.pkt               # Fichier Packet Tracer (à ajouter)
├── docs/
│   └── rapport.pdf             # Rapport du projet
└── README.md
👤 Auteur
EL AZZOUZI Abdelmoghit
Année universitaire : 2025/2026
Encadrant : Prof. Azeddine KHIAT
(Détails dans le rapport) 
2007800675519225856_rapport_cis…

📄 Licence
Projet à but pédagogique (Cisco Packet Tracer).
Ajoutez une licence (MIT / Apache-2.0 / etc.) selon vos besoins.
bash
Copier le code

Si tu me donnes le nom exact du fichier `.pkt` (ou l’arborescence de ton repo), je peux aussi adapter la section **“Contenu du dépôt”** et ajouter des captures/consignes ultra précises pour reproduire les tests dans Packet Tracer.
