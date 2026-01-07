# Projet Réseau – VLANs, EtherChannel et Routage Statique

## 📌 Description du projet
Ce projet consiste à concevoir et déployer une infrastructure réseau d’entreprise multisite
en utilisant **Cisco Packet Tracer**.  
Il met en œuvre les concepts fondamentaux des réseaux informatiques :
- Segmentation par VLAN
- Haute disponibilité avec EtherChannel (LACP)
- Routage inter-VLAN (Router-on-a-Stick)
- Routage statique WAN entre plusieurs sites

## 🏗️ Architecture du réseau
- 1 site principal (Siège)
- 2 sites distants
- 3 routeurs (R1, R2, R3)
- 2 switches (S1, S2)
- VLANs : 10, 20, 30, 50 (natif), 60 (Admin)

## 🌐 Plan d’adressage
### VLANs (Siège)
| VLAN | Usage | Réseau |
|----|------|--------|
| 10 | Utilisateurs 1 | 172.18.10.0/28 |
| 20 | Utilisateurs 2 | 172.18.20.0/28 |
| 30 | Utilisateurs 3 | 172.18.30.0/28 |
| 50 | VLAN natif | 172.18.50.0/28 |
| 60 | Admin/Gestion | 172.18.60.0/28 |

### WAN
- R1 – R2 : 10.0.30.176/30
- R1 – R3 : 10.0.30.180/30
- R2 – R3 : 10.0.30.184/30

## ⚙️ Technologies utilisées
- Cisco Packet Tracer
- VLAN & Trunking
- EtherChannel (LACP)
- Router-on-a-Stick
- Routage statique

## ✅ Tests réalisés
- Ping inter-VLAN
- Traceroute vers les sites distants
- Vérification des tables de routage
- Tests de gestion réseau

## 📁 Contenu du dépôt
- Fichier Packet Tracer (.pkt)
- Rapport du projet (PDF)
- Captures d’écran de configuration
- README.md

## 🎓 Auteur
**Nom :** El azzouzi Abdelmoughite  
**Année universitaire :** 2025 / 2026  
**Encadrant :** Prof. Azeddine KHIAT

