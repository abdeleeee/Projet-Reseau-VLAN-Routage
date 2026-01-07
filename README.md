# 🖧 Projet Réseau Multisites Segmenté – VLANs & Routage Statique

## 📌 Présentation du projet

Ce projet consiste à concevoir et déployer une **infrastructure réseau d’entreprise multisites** en utilisant **Cisco Packet Tracer**.  
L’objectif est de mettre en œuvre une architecture réseau **sécurisée, segmentée et interconnectée**, basée sur :

- VLANs  
- EtherChannel (LACP)  
- Router-on-a-Stick  
- Routage statique WAN  

**📚 Module** : Réseaux Informatiques  
**🎓 Année universitaire** : 2025 / 2026  
**👨‍🏫 Encadrant** : Prof. Azeddine KHIAT  
**👤 Étudiant** : El azzouzi Abdelmoughite  

---

## 🎯 Objectifs principaux

- 🔐 Segmentation logique du réseau avec des VLANs  
- 🚀 Haute disponibilité grâce à EtherChannel  
- 🔁 Communication inter-VLAN via Router-on-a-Stick  
- 🌐 Interconnexion multisites avec routage statique  
- ✅ Validation complète par des tests réseau  

---

## 🗺️ Topologie réseau

Le réseau est composé de :

- 1 site principal (**Siège**)  
- 2 sites distants  
- 3 routeurs (**R1, R2, R3**)  
- 2 switches (**S1, S2**)  
- Plusieurs VLANs utilisateurs et de gestion  
- Liaisons WAN série entre les routeurs  

---

## 🧩 Plan d’adressage – VLANs (Siège)

| VLAN | Usage            | Réseau IP       | Masque              | Passerelle       |
|------|------------------|-----------------|---------------------|------------------|
| 10   | Utilisateurs 1   | 172.18.10.0     | 255.255.255.240     | 172.18.10.14     |
| 20   | Utilisateurs 2   | 172.18.20.0     | 255.255.255.240     | 172.18.20.14     |
| 30   | Utilisateurs 3   | 172.18.30.0     | 255.255.255.240     | 172.18.30.14     |
| 50   | VLAN natif       | 172.18.50.0     | 255.255.255.240     | 172.18.50.14     |
| 60   | Admin / Gestion  | 172.18.60.0     | 255.255.255.240     | 172.18.60.14     |

---

## 🌍 Liaisons WAN (Routage Statique)

| Lien     | Réseau /30        | IP Routeur                  |
|----------|------------------|-----------------------------|
| R1 – R2  | 10.0.30.176/30   | R1: 10.0.30.177 / R2: .178 |
| R1 – R3  | 10.0.30.180/30   | R1: 10.0.30.181 / R3: .182 |
| R2 – R3  | 10.0.30.184/30   | R2: 10.0.30.185 / R3: .186 |

---

## ⚙️ Technologies utilisées

- **VLANs** : segmentation du réseau  
- **EtherChannel (LACP)** : agrégation de liens entre switches  
- **Trunk 802.1Q** avec VLAN natif  
- **Router-on-a-Stick** pour le routage inter-VLAN  
- **Routage statique** entre sites  
- **Cisco Packet Tracer**

---

## 🛠️ Étapes de configuration

### 1️⃣ Switching (Layer 2)

- Création des VLANs : 10, 20, 30, 50, 60  
- Affectation des ports aux VLANs  
- Configuration EtherChannel (Fa0/21 + Fa0/22)  
- Configuration du trunk sur Port-channel1  

### 2️⃣ Routage inter-VLAN (R1)

- Activation de l’interface Fa0/0  
- Création des sous-interfaces (Fa0/0.X)  
- Attribution des IP et encapsulation 802.1Q  

### 3️⃣ Routage statique WAN

- Routes statiques vers les réseaux distants sur R1  
- Route par défaut sur R2 et R3 vers R1  

---

## ✅ Tests & Validation

Les tests suivants ont été réalisés avec succès :

- 🔁 **Ping inter-VLAN** (VLAN 10 ↔ VLAN 20)  
- 🧭 **Traceroute** vers un site distant (passage par R1)  
- 🛡️ **Ping de gestion** (R1 → Switch S2)  
- 📋 **Vérification des tables de routage** :
  - Routes connectées (C)  
  - Routes statiques (S)  
  - Route par défaut (S*)  

---

## 📁 Contenu du dépôt GitHub
📦 Projet-Reseau-Multisite
┣ 📄 README.md
┣ 📄 Projet_Réseau_Segmenté.pdf
┣ 📄 packeeet1.pkt
┣ 📂 captures/

---

## 🎥 Présentation vidéo

Une **vidéo explicative (~4 minutes)** accompagne ce projet et présente :

- La topologie générale  
- La configuration des switches  
- Le routage et les tests  
- Une conclusion technique  

---

## 🏁 Conclusion

Ce projet démontre la mise en place d’une **infrastructure réseau robuste et professionnelle** :

✔️ Sécurité et organisation via VLANs  
✔️ Haute disponibilité avec EtherChannel  
✔️ Routage inter-VLAN maîtrisé  
✔️ Interconnexion WAN fonctionnelle  

