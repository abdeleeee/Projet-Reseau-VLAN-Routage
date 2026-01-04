# Conception et Configuration d'une Infrastructure Réseau Avancée
### VLANs – EtherChannel – Routage Inter-VLAN

## 📌 Description du projet
Ce projet présente la conception, la configuration et la validation d’une infrastructure réseau d’entreprise simulée à l’aide de **Cisco Packet Tracer**.  
Il met en œuvre des technologies avancées de **switching** et **routing** utilisées dans les réseaux professionnels.

## 🧠 Objectifs pédagogiques
- Segmentation du réseau avec VLANs
- Sécurisation et optimisation via Trunking
- Agrégation de liens avec EtherChannel (LACP)
- Routage Inter-VLAN (Router-on-a-Stick)
- Routage statique WAN
- Validation par tests de connectivité

## 🖧 Topologie réseau
### Équipements utilisés
- 2 Switchs Cisco (2960)
- 2 Routeurs Cisco (2911)
- Plusieurs PC clients

## 🌐 Plan d’adressage IP

| VLAN | Département   | Réseau IP        | Passerelle        |
|-----:|--------------|------------------|-------------------|
| 10   | Direction    | 192.168.10.0/24  | 192.168.10.254    |
| 20   | Technique    | 192.168.20.0/24  | 192.168.20.254    |
| 30   | Commercial   | 192.168.30.0/24  | 192.168.30.254    |
| 99   | VLAN Natif   | Non routé        | —                 |
| WAN  | R1 – R2      | 10.0.0.0/30      | —                 |

## ⚙️ Technologies utilisées
- VLANs (802.1Q)
- Trunking sécurisé
- EtherChannel (LACP)
- Router-on-a-Stick
- Routage statique

## 🧪 Tests réalisés
- Connectivité intra-VLAN
- Routage inter-VLAN
- Communication WAN
- Vérification des tables de routage

## 📂 Contenu du dépôt
- `configs/` : configurations IOS des équipements
- `packet-tracer/` : fichier Packet Tracer (.pkt)
- `docs/` : rapport du projet

## 👤 Auteur
**EL AZZOUZI Abdelmoghit**  
Cycle Ingénieur Informatique  
Année universitaire : 2025 / 2026

## 🎓 Encadrant
**Prof. Azeddine KHIAT**
