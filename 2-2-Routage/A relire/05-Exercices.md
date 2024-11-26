# Le routage

## 5. Exercices pratiques de routage

### 5.1 Exercice de base : Configuration d'un routage simple

Commençons par un exercice fondamental pour bien comprendre les mécanismes de base.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Réseau A -->
  <rect x="50" y="150" width="150" height="100" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="125" y="180" text-anchor="middle">Réseau A</text>
  <text x="125" y="200" text-anchor="middle" font-size="12">192.168.1.0/24</text>
  
  <!-- Routeur -->
  <rect x="300" y="170" width="120" height="60" fill="#4CAF50" stroke="#333"/>
  <text x="360" y="195" text-anchor="middle">Routeur R1</text>
  <text x="360" y="215" text-anchor="middle" font-size="12">.254</text>
  
  <!-- Réseau B -->
  <rect x="520" y="150" width="150" height="100" fill="#FFE6E6" stroke="#F44336"/>
  <text x="595" y="180" text-anchor="middle">Réseau B</text>
  <text x="595" y="200" text-anchor="middle" font-size="12">192.168.2.0/24</text>
  
  <!-- PC -->
  <circle cx="100" cy="300" r="20" fill="#FFF3E0" stroke="#FF9800"/>
  <text x="100" y="340" text-anchor="middle" font-size="12">PC1</text>
  <text x="100" y="355" text-anchor="middle" font-size="12">.10</text>
  
  <!-- Serveur -->
  <rect x="570" y="280" width="40" height="40" fill="#FFF3E0" stroke="#FF9800"/>
  <text x="590" y="340" text-anchor="middle" font-size="12">SRV1</text>
  <text x="590" y="355" text-anchor="middle" font-size="12">.20</text>
  
  <!-- Connexions -->
  <line x1="200" y1="200" x2="300" y2="200" stroke="#333" stroke-width="2"/>
  <line x1="420" y1="200" x2="520" y2="200" stroke="#333" stroke-width="2"/>
</svg>

```

#### Objectif
Configurer le routage entre deux réseaux pour permettre à PC1 (192.168.1.10) de communiquer avec SRV1 (192.168.2.20).

#### Données de configuration

- **Réseau A** : 192.168.1.0/24
- **Réseau B** : 192.168.2.0/24
- **Routeur R1** : 
  - Interface eth0 : 192.168.1.254
  - Interface eth1 : 192.168.2.254

#### Questions

1. Quelle sera la table de routage de PC1 ?
2. Quelle sera la table de routage du routeur R1 ?
3. Quelle sera la table de routage de SRV1 ?
4. Quel sera le chemin exact d'un paquet ICMP de PC1 vers SRV1 ?

#### Solution détaillée

##### 1. Table de routage de PC1

```plaintext
Destination       Masque          Passerelle      Interface
192.168.1.0      255.255.255.0   0.0.0.0         eth0
0.0.0.0          0.0.0.0         192.168.1.254   eth0
```

Explication :

- La première ligne est la route directe vers son propre réseau
- La seconde ligne est la route par défaut vers le routeur

##### 2. Table de routage de R1

```plaintext
Destination       Masque          Passerelle      Interface
192.168.1.0      255.255.255.0   0.0.0.0         eth0
192.168.2.0      255.255.255.0   0.0.0.0         eth1
```

Explication :

- Les deux routes sont directes car le routeur est connecté aux deux réseaux
- Pas besoin de route par défaut car il ne va pas sur Internet

##### 3. Table de routage de SRV1

```plaintext
Destination       Masque          Passerelle      Interface
192.168.2.0      255.255.255.0   0.0.0.0         eth0
0.0.0.0          0.0.0.0         192.168.2.254   eth0
```

##### 4. Chemin du paquet ICMP

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Infrastructure réseau de base -->
  <rect x="50" y="150" width="150" height="100" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="125" y="180" text-anchor="middle">Réseau A</text>
  
  <rect x="300" y="170" width="120" height="60" fill="#4CAF50" stroke="#333"/>
  <text x="360" y="195" text-anchor="middle">R1</text>
  
  <rect x="520" y="150" width="150" height="100" fill="#FFE6E6" stroke="#F44336"/>
  <text x="595" y="180" text-anchor="middle">Réseau B</text>
  
  <!-- Machines -->
  <circle cx="100" cy="300" r="20" fill="#FFF3E0" stroke="#FF9800"/>
  <text x="100" y="340" text-anchor="middle" font-size="12">PC1</text>
  
  <rect x="570" y="280" width="40" height="40" fill="#FFF3E0" stroke="#FF9800"/>
  <text x="590" y="340" text-anchor="middle" font-size="12">SRV1</text>
  
  <!-- Étapes du paquet -->
  <g id="packetPath">
    <!-- Étape 1 -->
    <path d="M100,280 L300,200" stroke="#2196F3" stroke-width="2" stroke-dasharray="5,5">
      <animate attributeName="stroke-dashoffset" from="10" to="0" dur="2s" repeatCount="indefinite"/>
    </path>
    <circle cx="200" cy="240" r="15" fill="white" stroke="#2196F3"/>
    <text x="200" y="245" text-anchor="middle">1</text>
    
    <!-- Étape 2 -->
    <path d="M420,200 L570,280" stroke="#4CAF50" stroke-width="2" stroke-dasharray="5,5">
      <animate attributeName="stroke-dashoffset" from="10" to="0" dur="2s" repeatCount="indefinite"/>
    </path>
    <circle cx="500" cy="240" r="15" fill="white" stroke="#4CAF50"/>
    <text x="500" y="245" text-anchor="middle">2</text>
  </g>
  
  <!-- Légende -->
  <rect x="50" y="400" width="700" height="80" fill="#f5f5f5" stroke="#ddd"/>
  <text x="70" y="420" fill="#333">Étape 1: PC1 → R1</text>
  <text x="70" y="440" fill="#666">MAC src: PC1, MAC dst: R1, IP src: 192.168.1.10, IP dst: 192.168.2.20</text>
  <text x="70" y="460" fill="#333">Étape 2: R1 → SRV1</text>
  <text x="70" y="480" fill="#666">MAC src: R1, MAC dst: SRV1, IP src: 192.168.1.10, IP dst: 192.168.2.20</text>
</svg>

```

Le paquet suivra ce chemin :

1. **PC1 → R1**
   - Consultation de la table de routage de PC1
   - Utilisation de la route par défaut
   - Encapsulation avec :
     * IP source : 192.168.1.10
     * IP destination : 192.168.2.20
     * MAC source : MAC de PC1
     * MAC destination : MAC de l'interface eth0 du routeur

2. **R1 → SRV1**
   - Le routeur reçoit le paquet et examine l'IP destination
   - Consultation de sa table de routage
   - Transmission via eth1 avec :
     * IP source et destination inchangées
     * Nouvelle encapsulation Ethernet :
       * MAC source : MAC de l'interface eth1 du routeur
       * MAC destination : MAC de SRV1

### 5.2 Exercice avancé : Routage avec redondance

Passons maintenant à un exercice plus complexe impliquant de la redondance.

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Réseau principal -->
  <rect x="50" y="50" width="150" height="80" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="125" y="90" text-anchor="middle">LAN A</text>
  <text x="125" y="110" text-anchor="middle" font-size="12">172.16.1.0/24</text>
  
  <!-- Routeurs principaux -->
  <rect x="300" y="40" width="100" height="50" fill="#4CAF50" stroke="#333"/>
  <text x="350" y="70" text-anchor="middle">R1</text>
  
  <rect x="300" y="150" width="100" height="50" fill="#4CAF50" stroke="#333"/>
  <text x="350" y="180" text-anchor="middle">R2</text>
  
  <!-- Réseau intermédiaire -->
  <rect x="500" y="100" width="100" height="150" fill="#FFE6E6" stroke="#F44336"/>
  <text x="550" y="175" text-anchor="middle">DMZ</text>
  <text x="550" y="195" text-anchor="middle" font-size="12">172.16.2.0/24</text>
  
  <!-- Internet -->
  <circle cx="700" cy="175" r="50" fill="#E0E0E0" stroke="#9E9E9E"/>
  <text x="700" y="175" text-anchor="middle">Internet</text>
  
  <!-- Connexions -->
  <line x1="200" y1="90" x2="300" y2="65" stroke="#333" stroke-width="2"/>
  <line x1="200" y1="90" x2="300" y2="175" stroke="#333" stroke-width="2"/>
  <line x1="400" y1="65" x2="500" y2="175" stroke="#333" stroke-width="2"/>
  <line x1="400" y1="175" x2="500" y2="175" stroke="#333" stroke-width="2"/>
  <line x1="600" y1="175" x2="650" y2="175" stroke="#333" stroke-width="2"/>
</svg>

```

#### Énoncé

Dans cette configuration plus complexe, nous avons :

##### Architecture

- **LAN A** : 172.16.1.0/24
- **DMZ** : 172.16.2.0/24
- **Routeur R1** :
  - Interface LAN : 172.16.1.253/24
  - Interface DMZ : 172.16.2.253/24
- **Routeur R2** :
  - Interface LAN : 172.16.1.254/24
  - Interface DMZ : 172.16.2.254/24
- **Serveur Web dans la DMZ** : 172.16.2.10/24
- **Poste client dans LAN A** : 172.16.1.10/24

#### Objectifs

1. Configurer une redondance complète entre LAN A et la DMZ
2. Mettre en place un partage de charge entre R1 et R2
3. Assurer une continuité de service en cas de panne d'un routeur

```svg
<svg viewBox="0 0 800 600" xmlns="http://www.w3.org/2000/svg">
  <!-- Infrastructure de base -->
  <g transform="translate(0,50)">
    <!-- LAN A -->
    <rect x="50" y="50" width="150" height="80" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="125" y="90" text-anchor="middle">LAN A</text>
    <text x="125" y="110" text-anchor="middle" font-size="12">172.16.1.0/24</text>
    
    <!-- Routeurs -->
    <rect x="300" y="40" width="100" height="50" fill="#4CAF50" stroke="#333"/>
    <text x="350" y="65" text-anchor="middle">R1</text>
    <text x="350" y="80" text-anchor="middle" font-size="10">VRRP: Master</text>
    
    <rect x="300" y="150" width="100" height="50" fill="#4CAF50" stroke="#333"/>
    <text x="350" y="175" text-anchor="middle">R2</text>
    <text x="350" y="190" text-anchor="middle" font-size="10">VRRP: Backup</text>
    
    <!-- DMZ -->
    <rect x="500" y="100" width="100" height="150" fill="#FFE6E6" stroke="#F44336"/>
    <text x="550" y="175" text-anchor="middle">DMZ</text>
    
    <!-- VRRP Details -->
    <rect x="50" y="250" width="200" height="100" fill="#FFF3E0" stroke="#FF9800"/>
    <text x="150" y="270" text-anchor="middle" font-weight="bold">VRRP Config</text>
    <text x="60" y="290" font-size="12">VIP LAN: 172.16.1.1</text>
    <text x="60" y="310" font-size="12">VIP DMZ: 172.16.2.1</text>
    <text x="60" y="330" font-size="12">Priority R1: 200</text>
    <text x="60" y="350" font-size="12">Priority R2: 100</text>
    
    <!-- Métriques -->
    <rect x="300" y="250" width="200" height="100" fill="#E1BEE7" stroke="#9C27B0"/>
    <text x="400" y="270" text-anchor="middle" font-weight="bold">Métriques</text>
    <text x="310" y="290" font-size="12">R1 → DMZ: 10</text>
    <text x="310" y="310" font-size="12">R2 → DMZ: 20</text>
    <text x="310" y="330" font-size="12">Preempt: Enabled</text>
    
    <!-- États -->
    <rect x="550" y="250" width="200" height="100" fill="#B2DFDB" stroke="#009688"/>
    <text x="650" y="270" text-anchor="middle" font-weight="bold">États possibles</text>
    <text x="560" y="290" font-size="12">Normal: R1 principal</text>
    <text x="560" y="310" font-size="12">Failover: R2 prend le relais</text>
    <text x="560" y="330" font-size="12">Recovery: Retour à R1</text>
  </g>
</svg>

```

#### Questions

1. Configurer VRRP sur les deux routeurs pour assurer la redondance
2. Établir les tables de routage pour :
   - Le poste client
   - Les deux routeurs
   - Le serveur Web
3. Expliquer le processus de basculement en cas de panne de R1
4. Décrire le chemin des paquets dans les scénarios suivants :
   - Fonctionnement normal
   - Panne de R1
   - Retour de R1

#### Solution détaillée

Commençons par la configuration VRRP :

```bash
# Configuration R1
interface eth0
  vrrp 1 ip 172.16.1.1
  vrrp 1 priority 200
  vrrp 1 preempt

interface eth1
  vrrp 2 ip 172.16.2.1
  vrrp 2 priority 200
  vrrp 2 preempt

# Configuration R2
interface eth0
  vrrp 1 ip 172.16.1.1
  vrrp 1 priority 100
  vrrp 1 preempt

interface eth1
  vrrp 2 ip 172.16.2.1
  vrrp 2 priority 100
  vrrp 2 preempt
```

#### Tables de routage

##### Pour le poste client (172.16.1.10)

```plaintext
Destination       Masque          Passerelle      Interface   Métrique
172.16.1.0       255.255.255.0   0.0.0.0         eth0        0
0.0.0.0          0.0.0.0         172.16.1.1      eth0        0  # VIP VRRP
```

##### Pour le serveur Web (172.16.2.10)

```plaintext
Destination       Masque          Passerelle      Interface   Métrique
172.16.2.0       255.255.255.0   0.0.0.0         eth0        0
0.0.0.0          0.0.0.0         172.16.2.1      eth0        0  # VIP VRRP
```

#### Scénarios de basculement

```svg
<svg viewBox="0 0 800 600" xmlns="http://www.w3.org/2000/svg">
  <!-- Timeline -->
  <line x1="50" y1="100" x2="750" y2="100" stroke="#333" stroke-width="2"/>
  
  <!-- États du système -->
  <g transform="translate(0,50)">
    <!-- Normal Operation -->
    <rect x="50" y="0" width="200" height="150" fill="#E8F5E9" stroke="#4CAF50"/>
    <text x="150" y="-10" text-anchor="middle" font-weight="bold">État Normal</text>
    <circle cx="100" cy="50" r="20" fill="#4CAF50"/>
    <text x="100" y="55" fill="white" text-anchor="middle">R1</text>
    <circle cx="200" cy="50" r="20" fill="#FFB74D"/>
    <text x="200" y="55" fill="white" text-anchor="middle">R2</text>
    <path d="M100,80 L150,120" stroke="#4CAF50" stroke-width="2" marker-end="url(#arrowhead)"/>
    
    <!-- Failover -->
    <rect x="300" y="0" width="200" height="150" fill="#FFEBEE" stroke="#F44336"/>
    <text x="400" y="-10" text-anchor="middle" font-weight="bold">Panne R1</text>
    <circle cx="350" cy="50" r="20" fill="#F44336"/>
    <text x="350" y="55" fill="white" text-anchor="middle">R1</text>
    <circle cx="450" cy="50" r="20" fill="#4CAF50"/>
    <text x="450" y="55" fill="white" text-anchor="middle">R2</text>
    <path d="M450,80 L400,120" stroke="#4CAF50" stroke-width="2" marker-end="url(#arrowhead)"/>
    
    <!-- Recovery -->
    <rect x="550" y="0" width="200" height="150" fill="#E8F5E9" stroke="#4CAF50"/>
    <text x="650" y="-10" text-anchor="middle" font-weight="bold">Retour R1</text>
    <circle cx="600" cy="50" r="20" fill="#4CAF50"/>
    <text x="600" y="55" fill="white" text-anchor="middle">R1</text>
    <circle cx="700" cy="50" r="20" fill="#FFB74D"/>
    <text x="700" y="55" fill="white" text-anchor="middle">R2</text>
    <path d="M600,80 L650,120" stroke="#4CAF50" stroke-width="2" marker-end="url(#arrowhead)"/>
  </g>
  
  <!-- Détails des états -->
  <g transform="translate(0,300)">
    <rect x="50" y="0" width="700" height="250" fill="#f5f5f5" stroke="#ddd"/>
    <text x="60" y="30" font-weight="bold">Description des états :</text>
    
    <text x="60" y="70">1. Fonctionnement normal :</text>
    <text x="80" y="90" font-size="12">- R1 est Master (priorité 200)</text>
    <text x="80" y="110" font-size="12">- R2 est Backup (priorité 100)</text>
    <text x="80" y="130" font-size="12">- Trafic passe par R1</text>
    
    <text x="60" y="160">2. Panne de R1 :</text>
    <text x="80" y="180" font-size="12">- R2 détecte la panne via VRRP</text>
    <text x="80" y="200" font-size="12">- R2 devient Master</text>
    <text x="80" y="220" font-size="12">- Bascule en ~3 secondes</text>
  </g>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#4CAF50"/>
    </marker>
  </defs>
</svg>

```

#### Détail des scénarios

1. **Fonctionnement normal**
   - R1 est maître VRRP (priorité 200)
   - R1 répond aux adresses virtuelles 172.16.1.1 et 172.16.2.1
   - R2 surveille l'état de R1 via les messages VRRP
   - Tout le trafic passe par R1

2. **Panne de R1**
   - R2 ne reçoit plus les messages VRRP de R1
   - Après 3 secondes (timer par défaut), R2 passe en état Master
   - R2 commence à répondre aux adresses virtuelles
   - Les tables ARP des clients sont mises à jour
   - Le trafic bascule automatiquement vers R2

3. **Retour de R1**
   - R1 redémarre et envoie des messages VRRP
   - Comme preempt est activé et R1 a une priorité supérieure
   - R1 reprend son rôle de Master
   - R2 repasse en backup
   - Le trafic rebascule vers R1

### Configurations détaillées des équipements

#### Configuration R1 complète

```bash
# Configuration des interfaces
interface eth0
    ip address 172.16.1.253/24
    description "LAN Interface"
    # Configuration VRRP LAN
    vrrp 1 ip 172.16.1.1
    vrrp 1 priority 200
    vrrp 1 preempt
    vrrp 1 track eth1 weight 50
    no shutdown

interface eth1
    ip address 172.16.2.253/24
    description "DMZ Interface"
    # Configuration VRRP DMZ
    vrrp 2 ip 172.16.2.1
    vrrp 2 priority 200
    vrrp 2 preempt
    vrrp 2 track eth0 weight 50
    no shutdown

# Configuration du routage
ip route 0.0.0.0/0 172.16.2.1
```

#### Configuration R2 complète

```bash
# Configuration des interfaces
interface eth0
    ip address 172.16.1.254/24
    description "LAN Interface"
    # Configuration VRRP LAN
    vrrp 1 ip 172.16.1.1
    vrrp 1 priority 100
    vrrp 1 preempt
    vrrp 1 track eth1 weight 50
    no shutdown

interface eth1
    ip address 172.16.2.254/24
    description "DMZ Interface"
    # Configuration VRRP DMZ
    vrrp 2 ip 172.16.2.1
    vrrp 2 priority 100
    vrrp 2 preempt
    vrrp 2 track eth0 weight 50
    no shutdown

# Configuration du routage
ip route 0.0.0.0/0 172.16.2.1
```

### Scénarios de dépannage

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Infrastructure -->
  <g transform="translate(0,50)">
    <!-- Réseau de base -->
    <rect x="50" y="50" width="150" height="80" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="125" y="90" text-anchor="middle">LAN A</text>
    
    <!-- Routeurs avec états -->
    <g id="router1">
      <rect x="300" y="40" width="100" height="50" fill="#4CAF50" stroke="#333"/>
      <text x="350" y="65" text-anchor="middle">R1</text>
      <circle cx="320" cy="60" r="8" fill="#4CAF50"> <!-- Status indicator -->
        <animate attributeName="fill" values="#4CAF50;#F44336;#4CAF50" dur="4s" repeatCount="indefinite"/>
      </circle>
    </g>
    
    <g id="router2">
      <rect x="300" y="150" width="100" height="50" fill="#4CAF50" stroke="#333"/>
      <text x="350" y="175" text-anchor="middle">R2</text>
      <circle cx="320" cy="170" r="8" fill="#FFB74D"/> <!-- Status indicator -->
    </g>
    
    <!-- DMZ -->
    <rect x="500" y="100" width="100" height="150" fill="#FFE6E6" stroke="#F44336"/>
    <text x="550" y="175" text-anchor="middle">DMZ</text>
    
    <!-- Connexions -->
    <path d="M200,90 L300,65" stroke="#333" stroke-width="2" stroke-dasharray="5,5"/>
    <path d="M200,90 L300,175" stroke="#333" stroke-width="2" stroke-dasharray="5,5"/>
  </g>
  
  <!-- Panneau de diagnostic -->
  <g transform="translate(50,300)">
    <rect width="700" height="150" fill="#f5f5f5" stroke="#ddd"/>
    <text x="20" y="30" font-weight="bold">Points de vérification :</text>
    <text x="40" y="60">1. État VRRP</text>
    <text x="40" y="90">2. Connectivité réseau</text>
    <text x="40" y="120">3. Tables de routage</text>
    
    <rect x="250" y="20" width="400" height="110" fill="white" stroke="#ddd"/>
    <text x="270" y="40" font-family="monospace" font-size="12">
      # vrrp show status
      Interface: eth0
      VRID: 1  State: MASTER/BACKUP
      Virtual IP: 172.16.1.1
    </text>
  </g>
</svg>

```

#### Scénario 1 : Perte de connectivité LAN

Symptômes :

- Les clients du LAN ne peuvent plus accéder à la DMZ
- VRRP reste actif sur R1

Étapes de dépannage :

1. Vérifier l'état des interfaces

```bash
# Sur R1
show interface eth0
show interface eth1
```

2. Examiner les logs VRRP

```bash
# Sur R1 et R2
show vrrp
show log | grep VRRP
```

3. Vérifier la table ARP

```bash
# Sur les clients
arp -a
```

#### Scénario 2 : Basculement lent

Symptômes :

- Le temps de basculement dépasse les 3 secondes standard
- Interruption temporaire des services réseau

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Timeline -->
  <line x1="50" y1="100" x2="750" y2="100" stroke="#333" stroke-width="2"/>
  
  <!-- Time markers -->
  <line x1="50" y1="95" x2="50" y2="105" stroke="#333" stroke-width="2"/>
  <text x="50" y="120" text-anchor="middle">0s</text>
  
  <line x1="200" y1="95" x2="200" y2="105" stroke="#333" stroke-width="2"/>
  <text x="200" y="120" text-anchor="middle">3s</text>
  
  <line x1="350" y1="95" x2="350" y2="105" stroke="#333" stroke-width="2"/>
  <text x="350" y="120" text-anchor="middle">6s</text>
  
  <line x1="500" y1="95" x2="500" y2="105" stroke="#333" stroke-width="2"/>
  <text x="500" y="120" text-anchor="middle">9s</text>
  
  <!-- Events -->
  <circle cx="50" cy="100" r="5" fill="#F44336"/>
  <text x="50" y="80" text-anchor="middle">Panne R1</text>
  
  <circle cx="200" cy="100" r="5" fill="#FF9800"/>
  <text x="200" y="80" text-anchor="middle">Détection</text>
  
  <circle cx="350" cy="100" r="5" fill="#4CAF50"/>
  <text x="350" y="80" text-anchor="middle">Transition R2</text>
  
  <circle cx="500" cy="100" r="5" fill="#2196F3"/>
  <text x="500" y="80" text-anchor="middle">Service restauré</text>
  
  <!-- Analysis panel -->
  <rect x="50" y="150" width="700" height="200" fill="#f5f5f5" stroke="#ddd"/>
  <text x="70" y="180" font-weight="bold">Points de contrôle :</text>
  <text x="90" y="210">1. Timers VRRP</text>
  <text x="90" y="240">2. Charge CPU</text>
  <text x="90" y="270">3. Trafic réseau</text>
  <text x="90" y="300">4. État des connexions</text>
</svg>

```

Procédure de diagnostic :

1. **Vérification des timers VRRP**

```bash
# Sur R1 et R2
show vrrp interface eth0 timers
```

2. **Analyse de la charge CPU**

```bash
# Sur les deux routeurs
top
vmstat 1
```

3. **Examen du trafic réseau**

```bash
# Sur l'interface concernée
tcpdump -i eth0 vrrp
```

#### Scénario 3 : Split-brain

Symptômes :

- Les deux routeurs deviennent simultanément Master
- Conflit d'adresses IP sur le réseau

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Network segments -->
  <g transform="translate(0,50)">
    <!-- LAN A segment -->
    <rect x="50" y="50" width="200" height="100" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="150" y="90" text-anchor="middle">LAN A</text>
    
    <!-- Problem visualization -->
    <g id="splitBrain">
      <rect x="300" y="40" width="120" height="60" fill="#F44336" stroke="#333"/>
      <text x="360" y="75" text-anchor="middle" fill="white">R1 (MASTER)</text>
      
      <rect x="300" y="150" width="120" height="60" fill="#F44336" stroke="#333"/>
      <text x="360" y="185" text-anchor="middle" fill="white">R2 (MASTER)</text>
      
      <!-- Conflict indicators -->
      <path d="M360,100 L360,150" stroke="#FFD700" stroke-width="3" stroke-dasharray="5,5">
        <animate attributeName="stroke-opacity" values="1;0;1" dur="1s" repeatCount="indefinite"/>
      </path>
    </g>
    
    <!-- Warning signs -->
    <g id="warnings">
      <path d="M500,100 L520,140 L480,140 Z" fill="#FFD700" stroke="#333"/>
      <text x="500" y="135" text-anchor="middle">!</text>
      
      <rect x="460" y="160" width="200" height="80" fill="white" stroke="#ddd"/>
      <text x="560" y="180" text-anchor="middle">Conflit d'adresses IP</text>
      <text x="560" y="200" text-anchor="middle">VIP: 172.16.1.1</text>
    </g>
  </g>
  
  <!-- Diagnostic steps -->
  <g transform="translate(50,300)">
    <rect width="700" height="150" fill="#f5f5f5" stroke="#ddd"/>
    <text x="20" y="30" font-weight="bold">Résolution du split-brain :</text>
    <text x="40" y="60">1. Isoler la cause de la rupture de communication</text>
    <text x="40" y="90">2. Vérifier la configuration VRRP</text>
    <text x="40" y="120">3. Rétablir la communication entre routeurs</text>
  </g>
</svg>

```

Actions correctives :

1. **Identifier la rupture de communication**

```bash
# Vérifier la connectivité entre routeurs
ping -I eth0 172.16.1.254  # Depuis R1
ping -I eth0 172.16.1.253  # Depuis R2
```

2. **Examiner les configurations VRRP**

```bash
# Sur les deux routeurs
show running-config | section vrrp
```

3. **Vérifier les priorités et l'état**

```bash
# Sur R1 et R2
show vrrp brief
show vrrp detail
```

#### Scénario 4 : Instabilité du basculement

Symptômes :

- Basculements fréquents entre R1 et R2
- Interruptions intermittentes des services
- Logs montrant des changements d'état VRRP répétés

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Timeline avec états changeants -->
  <line x1="50" y1="100" x2="750" y2="100" stroke="#333" stroke-width="2"/>
  
  <!-- États oscillants -->
  <path d="M50,100 Q150,50 250,100 Q350,150 450,100 Q550,50 650,100 Q750,150 750,100" 
        stroke="#F44336" stroke-width="2" fill="none"/>
  
  <!-- Indicateurs d'état -->
  <g transform="translate(0,20)">
    <text x="100" y="60" fill="#4CAF50">R1 Master</text>
    <text x="300" y="130" fill="#FF9800">R2 Master</text>
    <text x="500" y="60" fill="#4CAF50">R1 Master</text>
    <text x="700" y="130" fill="#FF9800">R2 Master</text>
  </g>
  
  <!-- Panneau de diagnostic -->
  <rect x="50" y="200" width="700" height="180" fill="#f5f5f5" stroke="#ddd"/>
  <text x="70" y="230" font-weight="bold">Diagnostic de l'instabilité :</text>
  
  <!-- Indicateurs de performance -->
  <g transform="translate(70,250)">
    <rect width="150" height="40" fill="#FFE0B2" stroke="#FF9800"/>
    <text x="75" y="25">CPU: 89%</text>
    
    <rect x="200" y="0" width="150" height="40" fill="#FFCDD2" stroke="#F44336"/>
    <text x="275" y="25">Latence: 2.5s</text>
    
    <rect x="400" y="0" width="150" height="40" fill="#C8E6C9" stroke="#4CAF50"/>
    <text x="475" y="25">Paquets perdus: 15%</text>
  </g>
</svg>

```

Procédure de diagnostic et correction :

1. **Analyser les ressources système**

```bash
# Surveillance des ressources
top -b -n 1
iostat
vmstat 1
```

2. **Vérifier la stabilité réseau**

```bash
# Test de latence
ping -f -c 1000 172.16.1.254  # Depuis R1
mtr 172.16.1.254              # Test continu
```

3. **Ajuster les paramètres VRRP**

```bash
# Augmenter les timers
vrrp 1 timers advertise 2
vrrp 1 timers delay minimum 10
```

#### Conclusion du module

Les points clés à retenir :

1. **Architecture redondante**

- La redondance est essentielle pour la haute disponibilité
- VRRP permet une bascule automatique et transparente
- La configuration doit être soigneusement planifiée

2. **Surveillance et maintenance**

- Monitoring constant des ressources
- Logs détaillés pour le dépannage
- Tests réguliers des mécanismes de basculement

3. **Bonnes pratiques**

- Documenter toutes les configurations
- Maintenir des procédures de dépannage à jour
- Effectuer des tests de basculement planifiés
- Surveiller les performances du réseau

4. **Points de vigilance**

- Éviter les configurations asymétriques
- Maintenir des ressources système adéquates
- Planifier la capacité du réseau
- Prévoir des procédures de restauration

Cette configuration redondante, bien que plus complexe qu'un routage simple, offre une meilleure résilience et une continuité de service essentielle pour les environnements de production.