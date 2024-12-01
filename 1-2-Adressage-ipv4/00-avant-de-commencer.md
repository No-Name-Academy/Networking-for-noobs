# L'adressage IPv4

## Introduction : Avant de commencer

Le réseau est souvent comparé à un système postal : pour communiquer efficacement, chaque destinataire doit avoir une adresse unique et comprendre comment les messages sont acheminés. L'adressage IPv4, pierre angulaire d'Internet, répond précisément à ce besoin.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Pile TCP/IP -->
  <g transform="translate(50,50)">
    <!-- Application -->
    <rect x="200" y="0" width="400" height="60" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="400" y="35" text-anchor="middle">Application (HTTP, FTP, DNS...)</text>
    
    <!-- Transport -->
    <rect x="200" y="80" width="400" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
    <text x="400" y="115" text-anchor="middle">Transport (TCP/UDP)</text>
    
    <!-- Internet -->
    <rect x="200" y="160" width="400" height="60" fill="#FFE6E6" stroke="#F44336"/>
    <text x="400" y="195" text-anchor="middle">Internet (IPv4/IPv6)</text>
    
    <!-- Accès réseau -->
    <rect x="200" y="240" width="400" height="60" fill="#FFF3E0" stroke="#FF9800"/>
    <text x="400" y="275" text-anchor="middle">Accès réseau (Ethernet)</text>

    <!-- Flèches -->
    <line x1="150" y1="30" x2="180" y2="30" stroke="#333" marker-end="url(#arrowhead)"/>
    <text x="120" y="35" font-size="12">Données</text>
    
    <line x1="150" y1="110" x2="180" y2="110" stroke="#333" marker-end="url(#arrowhead)"/>
    <text x="120" y="115" font-size="12">Segments</text>
    
    <line x1="150" y1="190" x2="180" y2="190" stroke="#333" marker-end="url(#arrowhead)"/>
    <text x="120" y="195" font-size="12">Paquets</text>
    
    <line x1="150" y1="270" x2="180" y2="270" stroke="#333" marker-end="url(#arrowhead)"/>
    <text x="120" y="275" font-size="12">Trames</text>
  </g>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333"/>
    </marker>
  </defs>
</svg>

```

### Pourquoi un protocole réseau ?

La communication en réseau pose plusieurs défis fondamentaux. Comment s'assurer que chaque machine peut être identifiée de manière unique ? Comment garantir que les messages arrivent à destination ? Comment organiser efficacement ces échanges ?

Le protocole TCP/IP répond à ces questions en proposant une architecture en couches, où chaque niveau a une responsabilité spécifique. L'IPv4, situé dans la couche Internet, s'occupe spécifiquement de l'adressage et du routage des paquets à travers les réseaux.

### Les bases de TCP/IP

Le protocole TCP/IP fonctionne comme un système postal sophistiqué :

1. **L'adressage** : Chaque machine reçoit une adresse IP unique, comme une adresse postale
2. **Le routage** : Les paquets sont acheminés de routeur en routeur, comme des lettres passant par différents centres de tri
3. **La fiabilité** : TCP s'assure que tous les paquets arrivent et dans le bon ordre
4. **La standardisation** : Des règles communes permettent à des équipements différents de communiquer

```svg
<svg viewBox="0 0 800 300" xmlns="http://www.w3.org/2000/svg">
  <!-- Source -->
  <rect x="50" y="100" width="100" height="60" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="100" y="135" text-anchor="middle">Source</text>
  
  <!-- Routeurs -->
  <rect x="250" y="100" width="80" height="60" fill="#4CAF50" stroke="#333"/>
  <text x="290" y="135" text-anchor="middle">R1</text>
  
  <rect x="450" y="100" width="80" height="60" fill="#4CAF50" stroke="#333"/>
  <text x="490" y="135" text-anchor="middle">R2</text>
  
  <!-- Destination -->
  <rect x="650" y="100" width="100" height="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="700" y="135" text-anchor="middle">Destination</text>
  
  <!-- Paquet en mouvement -->
  <circle cx="175" cy="130" r="10" fill="#FFD700">
    <animate attributeName="cx" values="175;375;575;700" dur="3s" repeatCount="indefinite"/>
  </circle>
  
  <!-- Connexions -->
  <line x1="150" y1="130" x2="250" y2="130" stroke="#333" stroke-width="2"/>
  <line x1="330" y1="130" x2="450" y2="130" stroke="#333" stroke-width="2"/>
  <line x1="530" y1="130" x2="650" y2="130" stroke="#333" stroke-width="2"/>
</svg>

```

Dans ce module, nous allons nous concentrer sur l'adressage IPv4, élément fondamental pour comprendre comment :
- Les machines s'identifient sur le réseau
- Les réseaux sont organisés et segmentés
- Le routage permet la communication entre différents réseaux
- La sécurité peut être mise en place au niveau réseau