# Le routage

## 1. Introduction au routage et protocole IP

### 1.1 Fondamentaux du routage

Le routage est le mécanisme fondamental qui permet aux informations de transiter d'un réseau à un autre. Pour bien comprendre ce concept, nous devons explorer :

- Comment les données sont organisées au niveau de la couche 3
- Quel matériel permet la communication entre réseaux
- Comment les machines communiquent entre différents réseaux

> 💡 Note importante : Bien que nous mentionnions la commande `ifconfig` dans ce cours, celle-ci n'est plus installée par défaut sur les distributions récentes Debian/RedHat. Vous pouvez l'installer via :
>
> - Debian/Ubuntu : `sudo apt-get install net-tools`
> - RedHat/CentOS : `sudo yum install net-tools`
>
> Ce package vous donnera également accès à d'autres commandes utiles comme `netstat` et `route`.

### 1.2 Le protocole IP

#### Qu'est-ce qu'un protocole IP ?

Un protocole est essentiellement un langage permettant aux machines de communiquer entre elles. Pour la couche 3 du modèle OSI, nous utilisons le protocole IP (Internet Protocol), qui définit la manière dont les données sont formatées et transmises.

#### Les composants essentiels

Dans sa forme la plus basique, le protocole IP nécessite deux informations fondamentales :

- L'adresse IP source (émetteur)
- L'adresse IP destination (récepteur)

#### Le masque réseau dans IP

Une question importante se pose : devons-nous inclure le masque réseau dans l'en-tête IP ? Pour répondre à cette question, analysons un exemple concret :

Imaginons une machine A (192.168.0.1/24) souhaitant communiquer avec une machine B (192.168.1.1/24) :

- La machine A doit d'abord déterminer si B est sur son réseau
- Pour cela, A examine sa propre plage d'adresses (192.168.0.0 à 192.168.0.255)
- A constate que 192.168.1.1 n'appartient pas à sa plage
- A en déduit que B est sur un autre réseau et qu'il faut utiliser la couche 3

Cette analyse révèle que le masque de B n'est pas nécessaire pour la communication - seule l'adresse IP suffit.

### 1.3 Format du datagramme IP

Le datagramme (ou paquet) IP organise les informations de manière structurée. Sa structure de base est la suivante :

```plaintext
+----------------+----------------+------------------+
|  En-tête IP    | Adresse IP    | Adresse IP      |
|  (autres info) |    Source     |   Destination   |
+----------------+----------------+------------------+
```

#### Particularité de l'en-tête IP

Une caractéristique intéressante de l'en-tête IP est la position de l'adresse de destination, qui n'est pas au début de l'en-tête. Ceci peut sembler contre-intuitif, surtout si l'on compare avec la couche 2 où l'adresse MAC de destination est placée en premier.

Cette différence s'explique par le processus de traitement des paquets :

- À la couche 2, la machine doit rapidement déterminer si la trame lui est destinée
- À la couche 3, ce contrôle a déjà été effectué par la couche 2
- La position de l'adresse IP de destination est donc moins critique
- Ce positionnement permet d'optimiser le traitement avec les informations de la couche 4

## 2. L'encapsulation et les en-têtes

### 2.1 Principe de l'encapsulation

Pour comprendre comment les données transitent réellement sur le réseau, nous devons examiner le concept d'encapsulation. Ce mécanisme, directement lié au modèle OSI, est fondamental dans le fonctionnement des réseaux modernes.

Voici une représentation visuelle du processus d'encapsulation :

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Fond -->
  <rect width="800" height="400" fill="#ffffff"/>
  
  <!-- Couches -->
  <g transform="translate(50,50)">
    <!-- Couche 7 -->
    <rect x="200" y="0" width="300" height="50" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="220" y="30" fill="#000">Données Application</text>
    
    <!-- Couche 4 -->
    <rect x="150" y="80" width="400" height="50" fill="#FFE6E6" stroke="#F44336"/>
    <rect x="200" y="80" width="300" height="50" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="160" y="110" fill="#000">En-tête Transport + Données</text>
    
    <!-- Couche 3 -->
    <rect x="100" y="160" width="500" height="50" fill="#E6FFE6" stroke="#4CAF50"/>
    <rect x="150" y="160" width="400" height="50" fill="#FFE6E6" stroke="#F44336"/>
    <rect x="200" y="160" width="300" height="50" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="110" y="190" fill="#000">En-tête IP + Transport + Données</text>
    
    <!-- Couche 2 -->
    <rect x="50" y="240" width="600" height="50" fill="#FFF3E0" stroke="#FF9800"/>
    <rect x="100" y="240" width="500" height="50" fill="#E6FFE6" stroke="#4CAF50"/>
    <rect x="150" y="240" width="400" height="50" fill="#FFE6E6" stroke="#F44336"/>
    <rect x="200" y="240" width="300" height="50" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="60" y="270" fill="#000">Trame complète</text>
    
    <!-- Flèches -->
    <path d="M350,60 L350,70" stroke="#666" stroke-width="2" marker-end="url(#arrowhead)"/>
    <path d="M350,140 L350,150" stroke="#666" stroke-width="2" marker-end="url(#arrowhead)"/>
    <path d="M350,220 L350,230" stroke="#666" stroke-width="2" marker-end="url(#arrowhead)"/>
    
    <!-- Marqueur de flèche -->
    <defs>
      <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
        <polygon points="0 0, 10 3.5, 0 7" fill="#666"/>
      </marker>
    </defs>
  </g>
</svg>

```

L'encapsulation fonctionne comme un système d'enveloppes imbriquées :

- À chaque niveau du modèle OSI, une nouvelle couche d'informations est ajoutée
- Les données d'origine sont progressivement entourées d'en-têtes spécifiques
- Chaque couche traite le contenu des couches supérieures comme de simples données

### 2.2 Structure détaillée d'une trame

Une trame Ethernet est plus complexe qu'elle n'y paraît. Voici sa structure détaillée :

```svg
<svg viewBox="0 0 800 200" xmlns="http://www.w3.org/2000/svg">
  <!-- Trame -->
  <g transform="translate(50,50)">
    <!-- Segments de la trame -->
    <rect x="0" y="0" width="140" height="60" fill="#FFE6E6" stroke="#F44336"/>
    <rect x="140" y="0" width="140" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
    <rect x="280" y="0" width="100" height="60" fill="#E6F3FF" stroke="#2196F3"/>
    <rect x="380" y="0" width="250" height="60" fill="#FFF3E0" stroke="#FF9800"/>
    <rect x="630" y="0" width="70" height="60" fill="#F3E5F5" stroke="#9C27B0"/>
    
    <!-- Labels -->
    <text x="20" y="35" fill="#000">MAC DST</text>
    <text x="160" y="35" fill="#000">MAC SRC</text>
    <text x="290" y="35" fill="#000">Type</text>
    <text x="460" y="35" fill="#000">Données</text>
    <text x="645" y="35" fill="#000">CRC</text>
    
    <!-- Détail des données -->
    <rect x="380" y="80" width="250" height="30" fill="#E6F3FF" opacity="0.6" stroke="#2196F3"/>
    <text x="440" y="100" fill="#000" font-size="12">En-têtes + Données encapsulées</text>
  </g>
</svg>

```

Dans une trame Ethernet :

1. L'en-tête contient :
   - L'adresse MAC destination (6 octets)
   - L'adresse MAC source (6 octets)
   - Le type de protocole (2 octets)

2. La partie données contient :
   - L'en-tête IP complet
   - L'en-tête de la couche transport
   - Les données applicatives

### 2.3 Observation pratique avec Wireshark

Wireshark est un outil essentiel pour visualiser concrètement l'encapsulation en action. Voyons comment une simple requête web se décompose à travers les différentes couches :

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Interface Wireshark -->
  <rect width="800" height="400" fill="#f5f5f5"/>
  
  <!-- En-tête -->
  <rect x="50" y="20" width="700" height="40" fill="#2196F3"/>
  <text x="70" y="45" fill="white">Wireshark - Capture de paquets</text>
  
  <!-- Zone de capture -->
  <g transform="translate(50,80)">
    <!-- Frame -->
    <rect x="0" y="0" width="700" height="60" fill="white" stroke="#ddd"/>
    <text x="10" y="35" fill="#666">Frame 187: 145 bytes on wire</text>
    
    <!-- Ethernet -->
    <rect x="20" y="70" width="680" height="60" fill="white" stroke="#ddd"/>
    <text x="30" y="105" fill="#666">Ethernet II, Src: Apple_12:34:56 (00:11:22:33:44:55), Dst: Broadcast</text>
    
    <!-- IP -->
    <rect x="40" y="140" width="660" height="60" fill="white" stroke="#ddd"/>
    <text x="50" y="175" fill="#666">Internet Protocol, Src: 192.168.1.100, Dst: 192.168.1.1</text>
    
    <!-- TCP -->
    <rect x="60" y="210" width="640" height="60" fill="white" stroke="#ddd"/>
    <text x="70" y="245" fill="#666">Transmission Control Protocol, Src Port: 54321, Dst Port: 80</text>
    
    <!-- HTTP -->
    <rect x="80" y="280" width="620" height="60" fill="white" stroke="#ddd"/>
    <text x="90" y="315" fill="#666">Hypertext Transfer Protocol (HTTP GET request)</text>
  </g>
</svg>

```

Lors de l'analyse d'une capture Wireshark, nous pouvons observer :

La décomposition en couches nous permet d'examiner chaque niveau de communication :

- Au niveau de la trame (Frame) : Nous voyons la totalité des données qui transitent sur le réseau, y compris les informations de synchronisation.

- Au niveau Ethernet : Wireshark nous révèle des informations précieuses sur les interfaces réseau :
  - Les adresses MAC source et destination
  - L'identification du fabricant de la carte réseau
  - Le type de protocole encapsulé

- Au niveau IP : Nous observons les détails du routage :
  - Les adresses IP source et destination
  - La taille du paquet
  - Les informations de fragmentation si présentes

### 2.4 La hiérarchie des couches en action

Pour bien comprendre comment ces couches interagissent, observons le parcours d'une requête web :

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- PC Client -->
  <g transform="translate(50,50)">
    <rect x="0" y="0" width="200" height="400" fill="#f5f5f5" stroke="#333"/>
    <text x="70" y="30" fill="#333">PC Client</text>
    
    <!-- Couches -->
    <rect x="20" y="50" width="160" height="60" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="60" y="85" fill="#333">Application</text>
    
    <rect x="20" y="130" width="160" height="60" fill="#FFE6E6" stroke="#F44336"/>
    <text x="60" y="165" fill="#333">Transport</text>
    
    <rect x="20" y="210" width="160" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
    <text x="70" y="245" fill="#333">Réseau</text>
    
    <rect x="20" y="290" width="160" height="60" fill="#FFF3E0" stroke="#FF9800"/>
    <text x="65" y="325" fill="#333">Liaison</text>
  </g>
  
  <!-- Serveur -->
  <g transform="translate(550,50)">
    <rect x="0" y="0" width="200" height="400" fill="#f5f5f5" stroke="#333"/>
    <text x="70" y="30" fill="#333">Serveur</text>
    
    <!-- Couches -->
    <rect x="20" y="50" width="160" height="60" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="60" y="85" fill="#333">Application</text>
    
    <rect x="20" y="130" width="160" height="60" fill="#FFE6E6" stroke="#F44336"/>
    <text x="60" y="165" fill="#333">Transport</text>
    
    <rect x="20" y="210" width="160" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
    <text x="70" y="245" fill="#333">Réseau</text>
    
    <rect x="20" y="290" width="160" height="60" fill="#FFF3E0" stroke="#FF9800"/>
    <text x="65" y="325" fill="#333">Liaison</text>
  </g>
  
  <!-- Flèches -->
  <g transform="translate(250,80)">
    <path d="M0,0 L300,0" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
    <path d="M300,80 L0,80" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
    <path d="M0,160 L300,160" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
    <path d="M300,240 L0,240" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  </g>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333"/>
    </marker>
  </defs>
</svg>

```

Ce processus d'interaction entre les couches se déroule en plusieurs étapes :

1. Au niveau Application :
   - L'utilisateur initie une requête web
   - Les données sont formatées selon le protocole HTTP
   - L'en-tête HTTP est ajouté

2. Au niveau Transport :
   - Les données sont segmentées si nécessaire
   - Un en-tête TCP ou UDP est ajouté
   - Des numéros de séquence sont attribués

3. Au niveau Réseau :
   - L'en-tête IP est ajouté
   - Les adresses source et destination sont définies
   - Le paquet est prêt pour le routage

4. Au niveau Liaison :
   - La trame Ethernet est construite
   - Les adresses MAC sont ajoutées
   - Le CRC est calculé

Cette organisation permet une grande flexibilité : si nous voulons changer la technologie de transmission physique, seule la couche liaison doit être modifiée, les couches supérieures restant inchangées.

## 3. Les routeurs et leur fonctionnement

### 3.1 Qu'est-ce qu'un routeur ?

Le routeur est l'élément clé qui permet la communication entre différents réseaux. Contrairement à un switch qui opère au niveau de la couche 2, le routeur travaille au niveau de la couche 3 du modèle OSI, ce qui lui permet de prendre des décisions basées sur les adresses IP.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Routeur central -->
  <rect x="300" y="150" width="200" height="100" fill="#E6FFE6" stroke="#4CAF50" stroke-width="2"/>
  <text x="370" y="200" fill="#333" text-anchor="middle">Routeur</text>
  
  <!-- Interfaces -->
  <rect x="280" y="170" width="20" height="20" fill="#2196F3"/>
  <rect x="500" y="170" width="20" height="20" fill="#2196F3"/>
  <rect x="380" y="130" width="20" height="20" fill="#2196F3"/>
  <rect x="380" y="250" width="20" height="20" fill="#2196F3"/>
  
  <!-- Réseaux connectés -->
  <circle cx="200" cy="180" r="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="200" y="185" fill="#333" text-anchor="middle">Réseau A</text>
  
  <circle cx="600" cy="180" r="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="600" y="185" fill="#333" text-anchor="middle">Réseau B</text>
  
  <circle cx="400" cy="80" r="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="400" y="85" fill="#333" text-anchor="middle">Réseau C</text>
  
  <circle cx="400" cy="320" r="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="400" y="325" fill="#333" text-anchor="middle">Réseau D</text>
  
  <!-- Connexions -->
  <line x1="300" y1="180" x2="260" y2="180" stroke="#666" stroke-width="2"/>
  <line x1="520" y1="180" x2="540" y2="180" stroke="#666" stroke-width="2"/>
  <line x1="390" y1="150" x2="390" y2="140" stroke="#666" stroke-width="2"/>
  <line x1="390" y1="270" x2="390" y2="260" stroke="#666" stroke-width="2"/>
</svg>

```

La particularité fondamentale d'un routeur est sa capacité à connecter plusieurs réseaux distincts. Pour accomplir cette tâche, il possède plusieurs caractéristiques essentielles :

- Plusieurs interfaces réseau, chacune connectée à un réseau différent
- Une adresse IP unique sur chaque interface
- Une table de routage pour diriger le trafic
- La capacité de traiter les paquets IP

#### La différence entre un routeur et un ordinateur multi-interfaces

Un aspect souvent méconnu est qu'un ordinateur équipé de plusieurs cartes réseau peut techniquement servir de routeur. Cependant, deux différences majeures existent :

1. Le traitement des paquets :
   - Un routeur accepte et retransmet les paquets qui ne lui sont pas destinés
   - Un ordinateur standard ignore ces paquets par défaut

2. L'optimisation :
   - Les routeurs sont optimisés pour le traitement rapide des paquets
   - Leur système d'exploitation est conçu spécifiquement pour cette tâche
   - Ils disposent de matériel spécialisé pour accélérer le routage

### 3.2 Fonctionnement d'un routeur

Observons le traitement d'un paquet par un routeur :

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Étapes de traitement -->
  <g transform="translate(50,50)">
    <!-- Réception -->
    <rect x="0" y="0" width="150" height="80" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="75" y="45" text-anchor="middle" fill="#333">Réception du paquet</text>
    
    <!-- Analyse -->
    <rect x="200" y="0" width="150" height="80" fill="#E6FFE6" stroke="#4CAF50"/>
    <text x="275" y="45" text-anchor="middle" fill="#333">Analyse de l'en-tête IP</text>
    
    <!-- Consultation -->
    <rect x="400" y="0" width="150" height="80" fill="#FFE6E6" stroke="#F44336"/>
    <text x="475" y="45" text-anchor="middle" fill="#333">Consultation table de routage</text>
    
    <!-- Transmission -->
    <rect x="600" y="0" width="150" height="80" fill="#FFF3E0" stroke="#FF9800"/>
    <text x="675" y="45" text-anchor="middle" fill="#333">Transmission</text>
    
    <!-- Flèches -->
    <path d="M150,40 L200,40" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
    <path d="M350,40 L400,40" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
    <path d="M550,40 L600,40" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  </g>
  
  <!-- Détails du processus -->
  <g transform="translate(50,200)">
    <!-- Paquet entrant -->
    <rect x="0" y="0" width="700" height="250" fill="#f5f5f5" stroke="#ddd"/>
    <text x="20" y="30" fill="#333">Détails du traitement :</text>
    
    <!-- Contenu du paquet -->
    <rect x="20" y="50" width="660" height="180" fill="white" stroke="#ddd"/>
    <text x="40" y="80" fill="#666">1. Vérification de l'intégrité du paquet</text>
    <text x="40" y="120" fill="#666">2. Extraction de l'adresse IP destination</text>
    <text x="40" y="160" fill="#666">3. Recherche du meilleur chemin</text>
    <text x="40" y="200" fill="#666">4. Mise à jour des champs nécessaires</text>
  </g>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333"/>
    </marker>
  </defs>
</svg>

```

### 3.3 Processus de routage d'un paquet

Prenons un exemple concret pour comprendre comment un routeur traite un paquet. Imaginons une communication entre une machine A (192.168.0.1) qui souhaite communiquer avec une machine B (10.0.0.1) à travers un routeur.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Machine A -->
  <rect x="50" y="150" width="120" height="60" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="110" y="185" text-anchor="middle">Machine A</text>
  <text x="110" y="200" text-anchor="middle" font-size="12">192.168.0.1</text>
  
  <!-- Routeur -->
  <rect x="300" y="130" width="200" height="100" fill="#E6FFE6" stroke="#4CAF50"/>
  <text x="400" y="170" text-anchor="middle">Routeur</text>
  <text x="340" y="190" text-anchor="middle" font-size="12">192.168.0.254</text>
  <text x="460" y="190" text-anchor="middle" font-size="12">10.0.0.254</text>
  
  <!-- Machine B -->
  <rect x="630" y="150" width="120" height="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="690" y="185" text-anchor="middle">Machine B</text>
  <text x="690" y="200" text-anchor="middle" font-size="12">10.0.0.1</text>
  
  <!-- Réseaux -->
  <text x="220" y="250" text-anchor="middle">Réseau 192.168.0.0/24</text>
  <text x="580" y="250" text-anchor="middle">Réseau 10.0.0.0/24</text>
  
  <!-- Flèches et étapes -->
  <path d="M170,180 L300,180" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M500,180 L630,180" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  
  <!-- Étapes numérotées -->
  <circle cx="235" y="160" r="15" fill="white" stroke="#333"/>
  <text x="235" y="165" text-anchor="middle">1</text>
  
  <circle cx="400" y="100" r="15" fill="white" stroke="#333"/>
  <text x="400" y="105" text-anchor="middle">2</text>
  
  <circle cx="565" y="160" r="15" fill="white" stroke="#333"/>
  <text x="565" y="165" text-anchor="middle">3</text>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333"/>
    </marker>
  </defs>
</svg>

```

#### Le voyage du paquet étape par étape

1. **Réception du paquet**
   - Le routeur reçoit une trame sur son interface 192.168.0.254
   - Il vérifie que l'adresse MAC destination correspond à son interface
   - Il extrait le paquet IP de la trame Ethernet

2. **Analyse et décision**
   - Le routeur examine l'adresse IP destination (10.0.0.1)
   - Il consulte sa table de routage pour déterminer la meilleure route
   - Il identifie l'interface de sortie appropriée

3. **Transmission du paquet**
   - Le routeur crée une nouvelle trame Ethernet
   - Il met à jour les adresses MAC source et destination
   - Il transmet le paquet sur l'interface appropriée

### 3.4 Détail d'une décision de routage

Examinons plus en détail comment le routeur prend sa décision :

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Diagramme de décision -->
  <g transform="translate(50,50)">
    <!-- Table de routage -->
    <rect x="50" y="20" width="600" height="150" fill="#f5f5f5" stroke="#333"/>
    <text x="60" y="45" fill="#333" font-weight="bold">Table de routage</text>
    
    <!-- Entrées de la table -->
    <line x1="50" y1="60" x2="650" y2="60" stroke="#333"/>
    <text x="60" y="80" fill="#333">Réseau destination</text>
    <text x="250" y="80" fill="#333">Masque</text>
    <text x="400" y="80" fill="#333">Passerelle</text>
    <text x="550" y="80" fill="#333">Interface</text>
    
    <text x="60" y="110" fill="#333">192.168.0.0</text>
    <text x="250" y="110" fill="#333">255.255.255.0</text>
    <text x="400" y="110" fill="#333">0.0.0.0</text>
    <text x="550" y="110" fill="#333">eth0</text>
    
    <text x="60" y="140" fill="#333">10.0.0.0</text>
    <text x="250" y="140" fill="#333">255.255.255.0</text>
    <text x="400" y="140" fill="#333">0.0.0.0</text>
    <text x="550" y="140" fill="#333">eth1</text>
    
    <!-- Processus de décision -->
    <path d="M350,200 L350,250" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
    
    <!-- Boîtes de décision -->
    <rect x="250" y="250" width="200" height="60" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="350" y="285" text-anchor="middle">Correspondance</text>
    
    <rect x="250" y="350" width="200" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
    <text x="350" y="385" text-anchor="middle">Action</text>
  </g>
</svg>

```

Le processus de décision implique plusieurs vérifications :

1. **Vérification de l'adresse destination**
   - Le routeur extrait l'adresse IP destination du paquet
   - Il la compare avec chaque entrée de sa table de routage
   - Il applique les masques de sous-réseau pour trouver la meilleure correspondance

2. **Sélection de la meilleure route**
   - La route la plus spécifique est choisie (plus long préfixe correspondant)
   - En cas d'égalité, la métrique la plus faible l'emporte
   - Une route par défaut est utilisée si aucune correspondance n'est trouvée

3. **Préparation de la transmission**
   - Le routeur détermine l'interface de sortie
   - Il identifie l'adresse MAC du prochain saut
   - Il met à jour les champs nécessaires du paquet

## 4. Les tables de routage

### 4.1 Principes fondamentaux

La table de routage est le cœur du processus de routage. Elle représente une carte routière que le routeur consulte pour savoir où envoyer les paquets qu'il reçoit. Contrairement à ce qu'on pourrait penser, cette table ne contient pas une entrée pour chaque destination possible sur Internet - ce serait impossible à gérer. Elle fonctionne plutôt avec des réseaux de destination.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Table principale -->
  <rect x="50" y="50" width="700" height="300" fill="#f5f5f5" stroke="#333"/>
  
  <!-- En-tête -->
  <rect x="50" y="50" width="700" height="50" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="400" y="80" text-anchor="middle" fill="#333" font-weight="bold">Table de routage</text>
  
  <!-- Colonnes -->
  <line x1="50" y1="100" x2="750" y2="100" stroke="#333"/>
  
  <!-- Headers -->
  <text x="120" y="130" fill="#333">Réseau destination</text>
  <text x="300" y="130" fill="#333">Masque</text>
  <text x="450" y="130" fill="#333">Passerelle</text>
  <text x="600" y="130" fill="#333">Interface</text>
  
  <!-- Entrées -->
  <text x="120" y="180" fill="#333">192.168.1.0</text>
  <text x="300" y="180" fill="#333">/24</text>
  <text x="450" y="180" fill="#333">Connecté</text>
  <text x="600" y="180" fill="#333">eth0</text>
  
  <text x="120" y="230" fill="#333">10.0.0.0</text>
  <text x="300" y="230" fill="#333">/24</text>
  <text x="450" y="230" fill="#333">192.168.1.254</text>
  <text x="600" y="230" fill="#333">eth1</text>
  
  <text x="120" y="280" fill="#333">0.0.0.0</text>
  <text x="300" y="280" fill="#333">/0</text>
  <text x="450" y="280" fill="#333">192.168.1.1</text>
  <text x="600" y="280" fill="#333">eth0</text>
</svg>

```

#### La composition d'une table de routage

Une table de routage comporte plusieurs colonnes essentielles, chacune ayant un rôle spécifique dans le processus de routage :

**Le réseau de destination** indique vers quel réseau les paquets doivent être acheminés. Contrairement à une idée reçue, on n'y trouve pas d'adresses de machines individuelles, mais des adresses de réseaux entiers. Par exemple, 192.168.1.0/24 représente tout un réseau pouvant contenir jusqu'à 254 machines.

**Le masque de sous-réseau** travaille en tandem avec l'adresse de réseau. Il permet au routeur de déterminer quelle portion de l'adresse correspond au réseau. Ce masque peut être noté de deux façons :

- En notation CIDR (par exemple /24)
- En notation décimale pointée (255.255.255.0)

**La passerelle** (ou "next hop") est l'adresse IP du prochain routeur sur le chemin vers la destination. Dans certains cas particuliers :

- La mention "Connecté" indique que le réseau est directement accessible
- Une adresse IP spécifique indique le prochain routeur à utiliser

**L'interface** spécifie quelle interface physique du routeur doit être utilisée pour atteindre la destination. Elle est généralement nommée selon des conventions comme :

- eth0, eth1 pour les interfaces Ethernet
- fa0/0, gi0/1 pour les interfaces FastEthernet ou GigabitEthernet
- wlan0 pour les interfaces sans fil

### 4.2 Types de routes

Il existe plusieurs types de routes, chacun ayant un rôle spécifique dans le fonctionnement du réseau :

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Routes directes -->
  <rect x="50" y="50" width="200" height="100" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="150" y="85" text-anchor="middle" fill="#333">Routes directes</text>
  <text x="150" y="105" text-anchor="middle" font-size="12">Réseaux directement</text>
  <text x="150" y="125" text-anchor="middle" font-size="12">connectés</text>
  
  <!-- Routes statiques -->
  <rect x="300" y="50" width="200" height="100" fill="#E6FFE6" stroke="#4CAF50"/>
  <text x="400" y="85" text-anchor="middle" fill="#333">Routes statiques</text>
  <text x="400" y="105" text-anchor="middle" font-size="12">Configurées</text>
  <text x="400" y="125" text-anchor="middle" font-size="12">manuellement</text>
  
  <!-- Routes dynamiques -->
  <rect x="550" y="50" width="200" height="100" fill="#FFE6E6" stroke="#F44336"/>
  <text x="650" y="85" text-anchor="middle" fill="#333">Routes dynamiques</text>
  <text x="650" y="105" text-anchor="middle" font-size="12">Apprises par</text>
  <text x="650" y="125" text-anchor="middle" font-size="12">protocoles de routage</text>
  
  <!-- Route par défaut -->
  <rect x="175" y="200" width="450" height="100" fill="#FFF3E0" stroke="#FF9800"/>
  <text x="400" y="235" text-anchor="middle" fill="#333">Route par défaut</text>
  <text x="400" y="255" text-anchor="middle" font-size="12">Utilisée quand aucune autre route ne correspond</text>
  <text x="400" y="275" text-anchor="middle" font-size="12">Notation : 0.0.0.0/0</text>
</svg>

```

### 4.2b Types de routes (détaillé)

#### Les routes directes (ou connectées)

Les routes directes sont les plus simples à comprendre car elles représentent les réseaux directement connectés aux interfaces du routeur. Ces routes sont créées automatiquement dès qu'une interface est configurée avec une adresse IP et activée.

Prenons un exemple concret :
Si un routeur possède une interface eth0 configurée avec l'adresse 192.168.1.1/24, il créera automatiquement une route directe vers le réseau 192.168.1.0/24 via cette interface. Le routeur sait qu'il peut atteindre directement toutes les machines de ce réseau, sans passer par un autre routeur intermédiaire.

#### Les routes statiques

Les routes statiques sont configurées manuellement par l'administrateur réseau. Elles sont particulièrement utiles dans plusieurs situations :

- Pour les petits réseaux avec peu de changements
- Pour définir un chemin spécifique qui ne doit pas changer
- Pour créer une route de secours (backup)

Exemple de configuration d'une route statique :

```bash
# Sur un routeur Linux
ip route add 10.0.0.0/24 via 192.168.1.254 dev eth0

# Sur un routeur Cisco
ip route 10.0.0.0 255.255.255.0 192.168.1.254
```

#### Les routes dynamiques

Les routes dynamiques sont apprises automatiquement grâce aux protocoles de routage comme RIP, OSPF, ou BGP. Ces protocoles permettent aux routeurs d'échanger des informations sur les réseaux qu'ils connaissent.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Routeur 1 -->
  <rect x="100" y="150" width="150" height="80" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="175" y="190" text-anchor="middle">Routeur 1</text>
  
  <!-- Routeur 2 -->
  <rect x="550" y="150" width="150" height="80" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="625" y="190" text-anchor="middle">Routeur 2</text>
  
  <!-- Flèches d'échange -->
  <path d="M250,170 C400,170 400,170 550,170" stroke="#4CAF50" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M550,210 C400,210 400,210 250,210" stroke="#4CAF50" stroke-width="2" marker-end="url(#arrowhead)"/>
  
  <!-- Messages -->
  <text x="400" y="150" text-anchor="middle" fill="#333">"J'ai accès au réseau 192.168.1.0/24"</text>
  <text x="400" y="240" text-anchor="middle" fill="#333">"Je connais le réseau 10.0.0.0/24"</text>
  
  <!-- Réseaux -->
  <circle cx="100" cy="300" r="40" fill="#FFE6E6" stroke="#F44336"/>
  <text x="100" y="305" text-anchor="middle" font-size="10">192.168.1.0/24</text>
  
  <circle cx="700" cy="300" r="40" fill="#FFE6E6" stroke="#F44336"/>
  <text x="700" y="305" text-anchor="middle" font-size="10">10.0.0.0/24</text>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#4CAF50"/>
    </marker>
  </defs>
</svg>

```

Les avantages du routage dynamique sont nombreux :

- Adaptation automatique aux changements de topologie réseau
- Configuration simplifiée sur les grands réseaux
- Capacité à trouver des chemins alternatifs en cas de panne

Cependant, il présente aussi quelques inconvénients :

- Consommation de bande passante pour les échanges d'informations
- Utilisation de ressources processeur supplémentaires
- Temps de convergence lors des changements de topologie

#### La route par défaut

La route par défaut est l'élément le plus important après les routes directes. Elle joue le rôle de "sortie de secours" quand aucune autre route ne correspond à la destination recherchée.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Réseau local -->
  <rect x="50" y="150" width="200" height="100" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="150" y="190" text-anchor="middle">Réseau local</text>
  <text x="150" y="210" text-anchor="middle" font-size="12">192.168.1.0/24</text>
  
  <!-- Routeur -->
  <rect x="300" y="170" width="100" height="60" fill="#4CAF50" stroke="#2196F3"/>
  <text x="350" y="205" text-anchor="middle">Routeur</text>
  
  <!-- Internet -->
  <circle cx="600" cy="200" r="100" fill="#FFE6E6" stroke="#F44336"/>
  <text x="600" y="205" text-anchor="middle">Internet</text>
  
  <!-- Flèches -->
  <path d="M250,200 L300,200" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M400,200 L500,200" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  
  <!-- Route par défaut -->
  <text x="400" y="150" text-anchor="middle" fill="#333">Route par défaut</text>
  <text x="400" y="170" text-anchor="middle" fill="#666">0.0.0.0/0</text>
</svg>

```

La route par défaut se caractérise par :

- Son réseau de destination : 0.0.0.0/0
- Sa capacité à correspondre à toutes les adresses
- Sa priorité la plus basse dans la table de routage

Dans un réseau d'entreprise typique, la route par défaut pointe généralement vers le routeur qui fournit l'accès à Internet. Cette configuration simple permet de garantir que tout le trafic destiné à Internet trouve son chemin vers l'extérieur du réseau local.

