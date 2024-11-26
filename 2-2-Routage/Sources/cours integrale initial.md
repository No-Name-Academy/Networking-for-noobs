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

### 4.3 Sélection des routes et prise de décision

La sélection de la meilleure route est un processus crucial qui suit des règles précises. Lorsqu'un paquet arrive, le routeur doit prendre une décision rapide et efficace pour déterminer le meilleur chemin à utiliser.

#### Le processus de décision

```svg
<svg viewBox="0 0 800 600" xmlns="http://www.w3.org/2000/svg">
  <!-- Flux de décision -->
  <rect x="300" y="50" width="200" height="60" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="400" y="85" text-anchor="middle">Arrivée du paquet</text>
  
  <!-- Premier niveau -->
  <rect x="300" y="150" width="200" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
  <text x="400" y="175" text-anchor="middle">Recherche des</text>
  <text x="400" y="195" text-anchor="middle">correspondances exactes</text>
  
  <!-- Deuxième niveau -->
  <rect x="300" y="250" width="200" height="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="400" y="275" text-anchor="middle">Recherche des</text>
  <text x="400" y="295" text-anchor="middle">correspondances partielles</text>
  
  <!-- Troisième niveau -->
  <rect x="300" y="350" width="200" height="60" fill="#FFF3E0" stroke="#FF9800"/>
  <text x="400" y="375" text-anchor="middle">Application de la</text>
  <text x="400" y="395" text-anchor="middle">route par défaut</text>
  
  <!-- Routes non trouvées -->
  <rect x="300" y="450" width="200" height="60" fill="#FFEBEE" stroke="#D32F2F"/>
  <text x="400" y="485" text-anchor="middle">Paquet rejeté</text>
  
  <!-- Flèches -->
  <path d="M400,110 L400,150" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M400,210 L400,250" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M400,310 L400,350" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M400,410 L400,450" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  
  <!-- Résultats positifs -->
  <path d="M500,180 L600,180" stroke="#4CAF50" stroke-width="2" marker-end="url(#arrowhead)"/>
  <rect x="600" y="150" width="150" height="60" fill="#A5D6A7" stroke="#4CAF50"/>
  <text x="675" y="185" text-anchor="middle">Route trouvée</text>
  
  <path d="M500,280 L600,280" stroke="#4CAF50" stroke-width="2" marker-end="url(#arrowhead)"/>
  <rect x="600" y="250" width="150" height="60" fill="#A5D6A7" stroke="#4CAF50"/>
  <text x="675" y="285" text-anchor="middle">Route trouvée</text>
  
  <path d="M500,380 L600,380" stroke="#4CAF50" stroke-width="2" marker-end="url(#arrowhead)"/>
  <rect x="600" y="350" width="150" height="60" fill="#A5D6A7" stroke="#4CAF50"/>
  <text x="675" y="385" text-anchor="middle">Route trouvée</text>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333"/>
    </marker>
  </defs>
</svg>

```

#### Ordre de priorité des routes

Le routeur suit un ordre strict dans l'examen des routes potentielles :

1. **Correspondance exacte** : Le routeur cherche d'abord une route qui correspond exactement à l'adresse de destination. Par exemple, si le paquet est destiné à 192.168.1.10, il cherchera une route vers le réseau 192.168.1.0/24.

2. **Plus longue correspondance** : Si plusieurs routes correspondent, le routeur choisit celle qui a le masque le plus long (plus spécifique). Par exemple :
   - Route A : 192.168.0.0/16
   - Route B : 192.168.1.0/24
   Pour atteindre 192.168.1.10, la route B sera choisie car plus spécifique.

3. **Distance administrative** : En cas d'égalité, le routeur examine la source de la route :
   - Routes directes (0)
   - Routes statiques (1)
   - Routes OSPF (110)
   - Routes RIP (120)
Le chiffre entre parenthèses représente la distance administrative ; plus elle est basse, plus la route est préférée.

4. **Métrique** : Si plusieurs routes sont encore en concurrence, la métrique départage :
   - Nombre de sauts
   - Bande passante
   - Délai
   - Charge
   - Fiabilité

#### Exemple pratique de sélection

Prenons une table de routage typique :

```plaintext
Destination       Masque          Passerelle      Interface   Métrique
192.168.1.0      255.255.255.0   0.0.0.0         eth0        0
192.168.0.0      255.255.0.0     10.0.0.1        eth1        10
0.0.0.0          0.0.0.0         192.168.1.254   eth0        1
```

Si un paquet arrive pour 192.168.1.100 :

1. La première route correspond exactement (192.168.1.0/24)
2. C'est une route directe (meilleure distance administrative)
3. Le paquet sera transmis directement via eth0

### 4.4 Dépannage et cas complexes de routage

Le dépannage du routage est une compétence cruciale pour tout administrateur réseau. Prenons des cas concrets pour comprendre les problèmes courants et leurs solutions.

#### Cas 1 : Le problème de la route asymétrique

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- PC A -->
  <rect x="50" y="150" width="100" height="50" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="100" y="180" text-anchor="middle">PC A</text>
  
  <!-- Routeur 1 -->
  <rect x="250" y="100" width="120" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
  <text x="310" y="135" text-anchor="middle">Routeur 1</text>
  
  <!-- Routeur 2 -->
  <rect x="250" y="250" width="120" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
  <text x="310" y="285" text-anchor="middle">Routeur 2</text>
  
  <!-- PC B -->
  <rect x="650" y="150" width="100" height="50" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="700" y="180" text-anchor="middle">PC B</text>
  
  <!-- Chemins -->
  <path d="M150,175 L250,130" stroke="#F44336" stroke-width="2" marker-end="url(#arrowhead-red)"/>
  <text x="180" y="140" fill="#F44336">Aller</text>
  
  <path d="M650,175 L370,280" stroke="#2196F3" stroke-width="2" marker-end="url(#arrowhead-blue)"/>
  <text x="500" y="250" fill="#2196F3">Retour</text>
  
  <!-- Marqueurs de flèche -->
  <defs>
    <marker id="arrowhead-red" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#F44336"/>
    </marker>
    <marker id="arrowhead-blue" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#2196F3"/>
    </marker>
  </defs>
</svg>

```

Dans un routage asymétrique, les paquets empruntent des chemins différents à l'aller et au retour. Bien que cela puisse fonctionner, cela peut causer plusieurs problèmes :

- Latence variable dans les deux sens
- Difficultés avec les pare-feux stateful
- Complexité accrue dans le dépannage
- Problèmes potentiels avec certains protocoles

Pour résoudre ce type de situation, plusieurs approches sont possibles :

1. **Ajuster les métriques de routage** pour favoriser le même chemin dans les deux sens
2. **Utiliser des routes statiques** pour forcer un chemin symétrique
3. **Configurer des politiques de routage** (PBR - Policy-Based Routing)

#### Cas 2 : Boucles de routage

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Routeurs en boucle -->
  <rect x="200" y="150" width="100" height="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="250" y="185" text-anchor="middle">R1</text>
  
  <rect x="400" y="150" width="100" height="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="450" y="185" text-anchor="middle">R2</text>
  
  <rect x="300" y="300" width="100" height="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="350" y="335" text-anchor="middle">R3</text>
  
  <!-- Flèches circulaires -->
  <path d="M300,180 L400,180" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M450,210 L400,300" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M300,300 L250,210" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  
  <!-- Paquet en boucle -->
  <circle cx="325" cy="180" r="10" fill="#4CAF50">
    <animate attributeName="cx" values="325;425;400;300;250" dur="3s" repeatCount="indefinite"/>
    <animate attributeName="cy" values="180;180;300;300;210" dur="3s" repeatCount="indefinite"/>
  </circle>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333"/>
    </marker>
  </defs>
</svg>

```

Les boucles de routage se produisent lorsque les paquets circulent indéfiniment entre plusieurs routeurs. Ce problème peut survenir pour plusieurs raisons :

1. **Configuration incorrecte des routes statiques**

```plaintext
Routeur 1 : route vers 10.0.0.0/24 via Routeur 2
Routeur 2 : route vers 10.0.0.0/24 via Routeur 3
Routeur 3 : route vers 10.0.0.0/24 via Routeur 1
```

2. **Problèmes de convergence** dans les protocoles de routage dynamique

3. **Routes par défaut mal configurées**

Pour détecter et résoudre ces boucles :

- Utiliser la commande `traceroute` pour voir le chemin des paquets
- Vérifier le TTL (Time To Live) des paquets
- Examiner les logs des routeurs pour les messages d'erreur
- Analyser les tables de routage de chaque routeur impliqué

### 4.5 Méthodes de résolution et cas complexes

#### Cas 3 : Routes fluctuantes (Route Flapping)

Ce problème se produit lorsqu'une route apparaît et disparaît rapidement de la table de routage, causant une instabilité dans le réseau.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Ligne de temps -->
  <line x1="100" y1="200" x2="700" y2="200" stroke="#333" stroke-width="2"/>
  
  <!-- Graduations -->
  <line x1="100" y1="195" x2="100" y2="205" stroke="#333" stroke-width="2"/>
  <line x1="300" y1="195" x2="300" y2="205" stroke="#333" stroke-width="2"/>
  <line x1="500" y1="195" x2="500" y2="205" stroke="#333" stroke-width="2"/>
  <line x1="700" y1="195" x2="700" y2="205" stroke="#333" stroke-width="2"/>
  
  <!-- Route status -->
  <path d="M100,150 L200,150 L200,250 L300,250 L300,150 L400,150 L400,250 L500,250 L500,150 L600,150 L600,250 L700,250" 
        stroke="#F44336" stroke-width="3" fill="none"/>
  
  <!-- Labels -->
  <text x="100" y="270" text-anchor="middle">T0</text>
  <text x="300" y="270" text-anchor="middle">T1</text>
  <text x="500" y="270" text-anchor="middle">T2</text>
  <text x="700" y="270" text-anchor="middle">T3</text>
  
  <!-- Status indicators -->
  <text x="150" y="140" fill="#4CAF50">Active</text>
  <text x="150" y="260" fill="#F44336">Inactive</text>
  
  <!-- Route identifier -->
  <text x="400" y="100" text-anchor="middle" font-weight="bold">Route vers 192.168.1.0/24</text>
</svg>

```

Les causes principales des routes fluctuantes incluent :

1. **Problèmes physiques**
   - Connexions instables
   - Interférences sur les liens sans fil
   - Matériel défectueux

2. **Problèmes de configuration**
   - Timers trop agressifs
   - Métriques mal configurées
   - Bande passante insuffisante

Pour résoudre ce problème, plusieurs techniques peuvent être appliquées :

```plaintext
# Exemple de configuration Cisco avec dampening
router bgp 65000
 address-family ipv4
  bgp dampening 15 750 2000 60
```

Cette configuration permet de :

- Pénaliser les routes instables
- Définir un seuil de suppression
- Établir un temps de récupération

#### Cas 4 : Redondance et Failover

La mise en place d'une redondance efficace nécessite une planification minutieuse :

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Serveurs -->
  <rect x="50" y="150" width="120" height="60" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="110" y="185" text-anchor="middle">Serveurs</text>
  
  <!-- Routeur principal -->
  <rect x="300" y="100" width="120" height="60" fill="#4CAF50" stroke="#333"/>
  <text x="360" y="135" text-anchor="middle">R1 Principal</text>
  
  <!-- Routeur backup -->
  <rect x="300" y="250" width="120" height="60" fill="#FF9800" stroke="#333"/>
  <text x="360" y="285" text-anchor="middle">R2 Backup</text>
  
  <!-- Internet -->
  <circle cx="600" cy="180" r="80" fill="#FFE6E6" stroke="#F44336"/>
  <text x="600" y="185" text-anchor="middle">Internet</text>
  
  <!-- Connexions -->
  <path d="M170,180 L300,130" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M170,180 L300,280" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M420,130 L520,180" stroke="#4CAF50" stroke-width="3" marker-end="url(#arrowhead-active)"/>
  <path d="M420,280 L520,180" stroke="#999" stroke-width="2" stroke-dasharray="5,5" marker-end="url(#arrowhead)"/>
  
  <!-- Status -->
  <text x="450" y="120" fill="#4CAF50">Active</text>
  <text x="450" y="270" fill="#999">Standby</text>
  
  <!-- VRRP info -->
  <text x="240" y="200" fill="#333">VRRP</text>
  <text x="240" y="220" fill="#666">VIP: 192.168.1.254</text>
  
  <!-- Marqueurs de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333"/>
    </marker>
    <marker id="arrowhead-active" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#4CAF50"/>
    </marker>
  </defs>
</svg>

```

Pour une redondance efficace, plusieurs mécanismes doivent être mis en place :

1. **Protocoles de redondance**
   - VRRP (Virtual Router Redundancy Protocol)
   - HSRP (Hot Standby Router Protocol)
   - GLBP (Gateway Load Balancing Protocol)

2. **Surveillance des liens**

   ```plaintext
   # Configuration VRRP basique
   interface eth0
    vrrp 1 ip 192.168.1.254
    vrrp 1 priority 200
    vrrp 1 preempt
   ```

3. **Mécanismes de basculement**
   - Détection de panne
   - Temps de basculement
   - Retour à la normale

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