# Le routage

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
