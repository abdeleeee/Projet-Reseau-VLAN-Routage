# 🌐 Projet Réseau – VLAN, EtherChannel & Routage Statique

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue)
![Networking](https://img.shields.io/badge/Networking-VLAN%20%7C%20Routing-green)
![Status](https://img.shields.io/badge/Status-Completed-success)
![GitHub](https://img.shields.io/badge/GitHub-Project-black)

---

## 📌 Présentation générale
Ce projet consiste à **concevoir, configurer et valider** une infrastructure réseau
d’entreprise **multisites**, simulée à l’aide de **Cisco Packet Tracer**.

Il répond aux exigences pédagogiques du module **Réseaux Informatiques**
et met en œuvre des technologies fondamentales utilisées en entreprise.

---

## 🎯 Objectifs du projet
- Segmenter le réseau avec des **VLANs**
- Améliorer la disponibilité via **EtherChannel (LACP)**
- Assurer le **routage inter-VLAN** (Router-on-a-Stick)
- Interconnecter plusieurs sites via **routage statique WAN**
- Tester et valider la connectivité globale du réseau

---

## 🧩 Topologie réseau
L’infrastructure est composée de :
- 🏢 **Un site central (Siège)**
- 🏬 **Deux sites distants interconnectés (WAN)**

Les VLANs configurés au siège :
- VLAN 10 : Utilisateurs 1  
- VLAN 20 : Utilisateurs 2  
- VLAN 30 : Utilisateurs 3  
- VLAN 50 : VLAN natif  
- VLAN 60 : Administration  

📸 **Aperçu de la topologie :**
> *(Image à ajouter dans le dossier `Captures/`)*


yaml
Copier le code

---

## 🛠️ Technologies et outils utilisés
- **Cisco Packet Tracer**
- VLAN & Trunking
- EtherChannel (LACP)
- Router-on-a-Stick
- Routage statique WAN
- GitHub (versioning & documentation)

---

## 🧪 Tests et validation
Les tests suivants ont été réalisés avec succès :

- ✅ Ping inter-VLAN (VLAN 10 ↔ VLAN 20)
- ✅ Traceroute vers les sites distants
- ✅ Ping de gestion réseau
- ✅ Vérification des tables de routage (`show ip route`)

📸 Exemples de captures :


yaml
Copier le code

---

## 📁 Organisation du dépôt
Projet-Reseau-VLAN-Routage-Statique/
│
├── README.md
├── PacketTracer/
│ └── Projet_Reseau_Multisite.pkt
├── Captures/
│ ├── Topologie.png
│ ├── Ping_InterVLAN.png
│ ├── Traceroute_WAN.png
│ └── Show_IP_Route.png
└── Rapport/
└── Nouveau_Rapport_Projet_Reseau_VLAN_Routage.pdf

yaml
Copier le code

---

## 👨‍🎓 Informations académiques
- **Étudiant :** El azzouzi Abdelmoughite  
- **Encadrant :** Prof. Azeddine KHIAT  
- **Année universitaire :** 2025 / 2026  

---

## 🔗 Lien du dépôt GitHub
👉 https://github.com/abdeleeee/Projet-Reseau-VLAN-Routage

---

## ✅ Conclusion
Ce projet démontre la mise en place d’une **infrastructure réseau robuste, sécurisée et évolutive**,
en conformité avec les standards professionnels des réseaux informatiques.
