# L'adressage IPv4

## 01. L'adresse IP

### Pourquoi une adresse IP ?

Une adresse IP joue un rôle similaire à une adresse postale dans le monde réel : elle permet d'identifier de manière unique chaque "habitant" du réseau et de lui acheminer les informations qui lui sont destinées.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Notation décimale -->
  <g transform="translate(50,50)">
    <rect x="0" y="0" width="160" height="60" fill="#E6F3FF" stroke="#2196F3"/>
    <rect x="160" y="0" width="160" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
    <rect x="320" y="0" width="160" height="60" fill="#FFE6E6" stroke="#F44336"/>
    <rect x="480" y="0" width="160" height="60" fill="#FFF3E0" stroke="#FF9800"/>
    
    <text x="80" y="35" text-anchor="middle">192</text>
    <text x="240" y="35" text-anchor="middle">168</text>
    <text x="400" y="35" text-anchor="middle">1</text>
    <text x="560" y="35" text-anchor="middle">10</text>
    
    <!-- Notation binaire -->
    <text x="80" y="90" text-anchor="middle" font-size="12">11000000</text>
    <text x="240" y="90" text-anchor="middle" font-size="12">10101000</text>
    <text x="400" y="90" text-anchor="middle" font-size="12">00000001</text>
    <text x="560" y="90" text-anchor="middle" font-size="12">00001010</text>
  </g>
  
  <!-- Explications -->
  <g transform="translate(50,200)">
    <text x="0" y="0" font-weight="bold">Structure :</text>
    <text x="0" y="30">• 4 octets (32 bits)</text>
    <text x="0" y="60">• Valeurs de 0 à 255 par octet</text>
    <text x="0" y="90">• Séparés par des points</text>
  </g>
</svg>

```

### Structure d'une adresse IPv4

Une adresse IPv4 se compose de 32 bits, organisés en quatre octets (groupes de 8 bits). Pour faciliter la lecture, nous utilisons une notation décimale pointée, où chaque octet est converti en nombre décimal (de 0 à 255) et séparé par des points.

Prenons l'exemple de l'adresse 192.168.1.10 :
1. Premier octet : 192 (11000000 en binaire)
2. Deuxième octet : 168 (10101000 en binaire)
3. Troisième octet : 1 (00000001 en binaire)
4. Quatrième octet : 10 (00001010 en binaire)

### La correspondance IP/MAC

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- PC1 -->
  <rect x="50" y="100" width="120" height="60" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="110" y="130" text-anchor="middle">PC1</text>
  <text x="110" y="145" text-anchor="middle" font-size="10">192.168.1.10</text>
  
  <!-- Switch -->
  <rect x="300" y="200" width="200" height="100" fill="#4CAF50" stroke="#333"/>
  <text x="400" y="250" text-anchor="middle">Switch</text>
  
  <!-- PC2 -->
  <rect x="630" y="100" width="120" height="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="690" y="130" text-anchor="middle">PC2</text>
  <text x="690" y="145" text-anchor="middle" font-size="10">192.168.1.20</text>
  
  <!-- ARP Request -->
  <path d="M170,130 C250,130 300,200 400,250" stroke="#2196F3" stroke-width="2" stroke-dasharray="5,5" marker-end="url(#arrowhead)">
    <animate attributeName="stroke-dashoffset" from="10" to="0" dur="2s" repeatCount="indefinite"/>
  </path>
  <text x="250" y="160" fill="#2196F3">1. Qui a 192.168.1.20 ?</text>
  
  <!-- ARP Reply -->
  <path d="M630,130 C550,130 500,200 400,250" stroke="#F44336" stroke-width="2" marker-end="url(#arrowhead)">
    <animate attributeName="stroke-dashoffset" from="10" to="0" dur="2s" repeatCount="indefinite"/>
  </path>
  <text x="550" y="160" fill="#F44336">2. C'est moi !</text>
  
  <!-- Table ARP -->
  <rect x="50" y="350" width="300" height="100" fill="#f5f5f5" stroke="#ddd"/>
  <text x="60" y="370" font-weight="bold">Table ARP de PC1</text>
  <text x="60" y="390" font-family="monospace" font-size="12">IP              MAC</text>
  <text x="60" y="410" font-family="monospace" font-size="12">192.168.1.20    00:1A:2B:3C:4D:5E</text>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333"/>
    </marker>
  </defs>
</svg>

```

Le protocole ARP (Address Resolution Protocol) permet d'établir la correspondance entre les adresses IP (couche 3) et les adresses MAC (couche 2). Ce processus est essentiel car les communications physiques sur un réseau local se font toujours via les adresses MAC.

Le processus se déroule en deux temps :
1. Une machine envoie une requête ARP en broadcast : "Qui possède cette adresse IP ?"
2. La machine concernée répond avec son adresse MAC
3. Les informations sont stockées dans une table ARP temporaire