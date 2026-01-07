# Configuration Avancée — VLANs, EtherChannel (LACP) & Routage (Router-on-a-Stick) | Cisco Packet Tracer

**Projet Packet Tracer** simulant une infrastructure réseau de PME avec segmentation, redondance et routage :
- **VLANs** (Direction / Technique / Commercial)
- **Trunks 802.1Q** (VLAN natif + VLANs autorisés)
- **EtherChannel (LACP)** entre commutateurs
- **Routage Inter‑VLAN (Router‑on‑a‑Stick)** via sous‑interfaces
- **Interconnexion WAN** + **routage statique**
- **Tests & validation** (ping + commandes `show`)

---

## 1) Objectif du dépôt (valorisation)

Ce dépôt a été construit pour répondre à l’exigence **“Dépôt GitHub (obligatoire pour valorisation)”** :

✅ **README détaillé** (ce fichier)  
✅ **Fichier Packet Tracer** (`.pkt`) + **exports de configuration** (`show running-config`)  
✅ **Étapes d’installation / déploiement** (ouvrir / rejouer la config)  
✅ **Ressources utiles** : schémas, captures, preuves de tests, diagrammes

---

## 2) Contenu recommandé du dépôt

> Adapte les noms de fichiers si besoin, mais garde une structure claire.

```
.
├── packet-tracer/
│   └── packeeet1.pkt
├── configs/
│   ├── SW1_running-config.txt
│   ├── SW2_running-config.txt
│   ├── R1_running-config.txt
│   └── R2_running-config.txt
├── docs/
│   └── rapport_cisco_packet_tracer.pdf
├── resources/
│   ├── topologie.png
│   ├── plan_adressage.png
│   └── captures_tests/
│       ├── ping_intra_vlan10.png
│       ├── ping_inter_vlan10_vers_vlan20.png
│       ├── ping_wan_vers_R2.png
│       ├── show_vlan_brief.png
│       ├── show_interfaces_trunk.png
│       ├── show_etherchannel_summary.png
│       └── show_ip_route.png
└── README.md
```

### Fichiers importants à inclure (minimum)
- `packet-tracer/packeeet1.pkt` (obligatoire)
- `docs/rapport_cisco_packet_tracer.pdf` (recommandé)
- `configs/*.txt` (très recommandé pour prouver la configuration)
- `resources/` (schémas et captures de validation)

---

## 3) Prérequis

- **Cisco Packet Tracer** (version 8.x recommandée)
- OS : Windows / Linux / macOS (selon support Packet Tracer)
- Aucun accès Internet requis (projet local)

---

## 4) Installation / Déploiement

### Option A — Déploiement rapide (ouvrir et tester)
1. Cloner le dépôt :
   ```bash
   git clone <URL_DU_REPO>
   cd <NOM_DU_REPO>
   ```
2. Ouvrir Cisco Packet Tracer
3. Charger le fichier :
   - `packet-tracer/packeeet1.pkt`
4. Passer en mode **Realtime**
5. Lancer les tests (section **9) Tests & validation**)

### Option B — Rejouer la configuration depuis `configs/` (si tu fournis les configs)
1. Ouvrir `packet-tracer/packeeet1.pkt`
2. Pour chaque équipement (SW1, SW2, R1, R2) :
   - Onglet **CLI**
   - Coller le contenu du fichier correspondant dans `configs/`
3. Vérifier avec :
   - `show vlan brief`
   - `show interfaces trunk`
   - `show etherchannel summary`
   - `show ip interface brief`
   - `show ip route`

---

## 5) Topologie (résumé)

### Équipements (typique PME)
- **SW1** : Switch principal (VLANs / trunks / EtherChannel)
- **SW2** : Switch secondaire (VLANs / trunks / EtherChannel)
- **R1** : Routeur passerelle (Inter‑VLAN — Router‑on‑a‑Stick)
- **R2** : Routeur distant (WAN)
- **PCs** : postes clients répartis par VLAN

📌 Ajoute dans `resources/topologie.png` une capture de ta topologie Packet Tracer.

---

## 6) Plan VLANs & adressage IP

> Bonne pratique : **dernière IP utilisable (.254)** utilisée comme **passerelle** (gateway) des VLANs.

| VLAN | Nom | Rôle | Réseau | Masque | Passerelle |
|------|-----|------|--------|--------|------------|
| 10 | DIRECTION | Département Direction | 192.168.10.0 | 255.255.255.0 (/24) | 192.168.10.254 |
| 20 | TECHNIQUE | Département Technique | 192.168.20.0 | 255.255.255.0 (/24) | 192.168.20.254 |
| 30 | COMMERCIAL | Département Commercial | 192.168.30.0 | 255.255.255.0 (/24) | 192.168.30.254 |
| 99 | NATIF_MGMT | VLAN natif (trafic non tagué / contrôle) | N/A (non routé) | N/A | N/A |
| WAN | - | Liaison R1–R2 | 10.0.0.0 | 255.255.255.252 (/30) | N/A |

---

## 7) Fonctionnalités techniques mises en œuvre

### 7.1 VLANs (segmentation)
- Création VLAN 10 / 20 / 30 / 99
- Nommage des VLANs
- Ports “access” affectés aux VLANs

### 7.2 Trunks sécurisés
- Trunks entre SW1 ↔ R1 et SW1 ↔ SW2 (via EtherChannel)
- **VLAN natif explicite** : VLAN 99
- **VLANs autorisés limités** : `10,20,30,99`

### 7.3 EtherChannel (LACP)
- Agrégation de 2 liens physiques (ex: Fa0/23-24) en un **Port‑Channel**
- Mode LACP **active**
- Trunk au niveau du Port‑Channel

### 7.4 Router‑on‑a‑Stick (Inter‑VLAN)
- Sous‑interfaces sur R1 : `G0/0.10`, `G0/0.20`, `G0/0.30`, `G0/0.99 (native)`
- Passerelles : `.254` pour chaque VLAN

### 7.5 WAN + Routage statique
- Lien série R1 ↔ R2 : `10.0.0.0/30`
- Routes statiques sur R1 et R2
- Exemple de réseau distant derrière R2 : `172.16.0.0/16` (si utilisé dans ton scénario)

---

## 8) Configuration IOS — “Tout en un” (templates copiables)

> ⚠️ Les numéros de ports peuvent varier selon ta maquette `.pkt`.  
> Adapte **uniquement** les interfaces (Fa/Gi/Se) si ton câblage est différent, le reste reste identique.

---

### 8.1 Configuration SW1 (Switch principal)

```cisco
enable
configure terminal

! --- VLANs ---
vlan 10
 name DIRECTION
vlan 20
 name TECHNIQUE
vlan 30
 name COMMERCIAL
vlan 99
 name NATIF_MGMT
exit

! --- Ports ACCESS (exemples) ---
interface fastEthernet0/1
 description PC_VLAN10
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
exit

interface fastEthernet0/2
 description PC_VLAN20
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
exit

! --- TRUNK vers le routeur R1 (ex: Gi0/1) ---
interface gigabitEthernet0/1
 description *** Lien vers Routeur R1 ***
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
exit

! --- EtherChannel LACP vers SW2 (ex: Fa0/23-24 -> Port-Channel 1) ---
interface range fastEthernet0/23 - 24
 description *** EtherChannel vers SW2 ***
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
 channel-group 1 mode active
exit

interface port-channel 1
 description *** Lien logique LACP vers SW2 ***
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
exit

end
write memory

! --- Vérification ---
show vlan brief
show interfaces trunk
show etherchannel summary
```

---

### 8.2 Configuration SW2 (Switch secondaire)

```cisco
enable
configure terminal

! --- VLANs (identiques à SW1) ---
vlan 10
 name DIRECTION
vlan 20
 name TECHNIQUE
vlan 30
 name COMMERCIAL
vlan 99
 name NATIF_MGMT
exit

! --- Ports ACCESS (exemples, adapte selon tes PCs) ---
interface fastEthernet0/1
 description PC_VLAN30
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
exit

interface fastEthernet0/2
 description PC_VLAN20
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
exit

! --- EtherChannel LACP vers SW1 (ports correspondants : Fa0/23-24) ---
interface range fastEthernet0/23 - 24
 description *** EtherChannel vers SW1 ***
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
 channel-group 1 mode active
exit

interface port-channel 1
 description *** Lien logique LACP vers SW1 ***
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
exit

end
write memory

! --- Vérification ---
show vlan brief
show interfaces trunk
show etherchannel summary
```

---

### 8.3 Configuration R1 (Routeur gateway — Router-on-a-Stick + WAN)

```cisco
enable
configure terminal

! --- Interface physique côté LAN (trunk vers SW1) ---
interface gigabitEthernet0/0
 description *** Trunk vers SW1 ***
 no shutdown
exit

! --- Sous-interfaces (Inter-VLAN routing) ---
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
exit

! --- WAN vers R2 (série /30) ---
interface serial0/0/0
 description *** Lien WAN vers R2 ***
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000
 no shutdown
exit

! --- Routage statique (exemple réseau distant derrière R2) ---
ip route 172.16.0.0 255.255.0.0 10.0.0.2

end
write memory

! --- Vérification ---
show ip interface brief
show ip route
```

---

### 8.4 Configuration R2 (Routeur distant — WAN + routes vers VLANs)

```cisco
enable
configure terminal

! --- WAN vers R1 ---
interface serial0/0/0
 description *** Lien WAN vers R1 ***
 ip address 10.0.0.2 255.255.255.252
 no shutdown
exit

! --- Routes statiques vers les VLANs du site principal ---
ip route 192.168.10.0 255.255.255.0 10.0.0.1
ip route 192.168.20.0 255.255.255.0 10.0.0.1
ip route 192.168.30.0 255.255.255.0 10.0.0.1

end
write memory

! --- Vérification ---
show ip interface brief
show ip route
```

---

## 9) Tests & validation (à documenter avec captures)

> Objectif : prouver que la segmentation, l’EtherChannel, le routage Inter‑VLAN et le WAN fonctionnent.

### Test 1 — Connectivité Intra‑VLAN
**But :** deux machines dans le même VLAN communiquent (niveau 2, sans passer par le routeur).  
**Exemple :**
- PC VLAN10 (192.168.10.10) → `ping 192.168.10.11`

✅ Attendu : réponses OK.

📸 Capture à fournir :
- `resources/captures_tests/ping_intra_vlan10.png`

---

### Test 2 — Connectivité Inter‑VLAN
**But :** valider le routage via R1 (Router-on-a-Stick).  
**Exemple :**
- PC VLAN10 (192.168.10.10) → `ping 192.168.20.10` (PC VLAN20)

✅ Attendu : réponses OK (le trafic transite par R1)  
ℹ️ Le **premier ping peut échouer** (temps ARP).

📸 Capture à fournir :
- `resources/captures_tests/ping_inter_vlan10_vers_vlan20.png`

---

### Test 3 — Connectivité WAN
**But :** valider les routes statiques et la liaison série R1–R2.  
**Exemple :**
- PC VLAN10 → `ping 10.0.0.2` (interface série de R2)

✅ Attendu : réponses OK.

📸 Capture à fournir :
- `resources/captures_tests/ping_wan_vers_R2.png`

---

### Vérifications “preuves” (commandes show)
À exécuter et capturer :

- VLANs :
  ```cisco
  show vlan brief
  ```
- Trunks :
  ```cisco
  show interfaces trunk
  ```
- EtherChannel :
  ```cisco
  show etherchannel summary
  ```
- Interfaces & routage :
  ```cisco
  show ip interface brief
  show ip route
  ```

📸 Captures à fournir :
- `show_vlan_brief.png`
- `show_interfaces_trunk.png`
- `show_etherchannel_summary.png`
- `show_ip_route.png`

---

## 10) Dépannage rapide

### Problème : Inter‑VLAN ne marche pas
- Vérifier trunk SW1 ↔ R1 :
  - VLAN natif **99**
  - VLANs autorisés : **10,20,30,99**
- Vérifier sous‑interfaces sur R1 :
  - `encapsulation dot1Q <VLAN>`
  - IP passerelles `.254`
- Vérifier passerelle sur les PCs :
  - VLAN10 → 192.168.10.254
  - VLAN20 → 192.168.20.254
  - VLAN30 → 192.168.30.254

### Problème : EtherChannel “down / suspended”
- Les ports agrégés doivent avoir la **même config** des deux côtés (trunk, native, allowed)
- Vérifier LACP :
  - `show etherchannel summary`

### Problème : WAN KO
- Vérifier `no shutdown` sur les interfaces série
- Vérifier **/30** et IP : `10.0.0.1` ↔ `10.0.0.2`
- Vérifier `clock rate` côté **DCE**
- Vérifier les routes statiques (`show ip route`)

---

## 11) Comment générer les fichiers `configs/*.txt` (preuve GitHub)

Sur chaque équipement :
1. CLI → taper :
   ```cisco
   show running-config
   ```
2. Copier/coller tout le résultat dans un fichier texte :
   - `configs/SW1_running-config.txt`
   - `configs/SW2_running-config.txt`
   - `configs/R1_running-config.txt`
   - `configs/R2_running-config.txt`

> Bonus : ajoute aussi `show vlan brief`, `show interfaces trunk`, etc. dans un fichier `verification.txt` si tu veux.

---

## 12) Auteur & infos

- **Projet réalisé par :** EL AZZOUZI Abdelmoghit  
- **Filière :** Cycle ingénieur en informatique  
- **Encadré par :** Prof. Azeddine KHIAT  
- **Année universitaire :** 2025/2026  

---

## 13) Licence

Projet à but pédagogique (simulation Cisco Packet Tracer).  
Ajoute une licence si nécessaire (MIT / Apache‑2.0 / autre).

---

## 14) Checklist avant de rendre (très important)

- [ ] `.pkt` présent dans `packet-tracer/`
- [ ] README complet (ce fichier)
- [ ] `docs/rapport_cisco_packet_tracer.pdf` ajouté
- [ ] `configs/*.txt` exportés (`show running-config`)
- [ ] `resources/` contient schéma topologie + captures de tests + captures des commandes `show`
- [ ] Tests ping OK (intra‑VLAN / inter‑VLAN / WAN)
- [ ] Dernier commit propre + push sur GitHub
