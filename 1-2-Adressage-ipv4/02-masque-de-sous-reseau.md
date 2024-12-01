# L'adressage IPv4

## 02. Le masque de sous réseau

### Pourquoi limiter ?

Le masque de sous-réseau est un concept fondamental qui permet d'organiser et de segmenter efficacement les réseaux. Sans cette segmentation, nous aurions un immense réseau difficile à gérer et à sécuriser.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Grand réseau non segmenté -->
  <g transform="translate(50,50)">
    <rect x="0" y="0" width="300" height="150" fill="#FFE6E6" stroke="#F44336"/>
    <text x="150" y="30" text-anchor="middle">Sans masque</text>
    <circle cx="50" cy="80" r="10" fill="#333"/>
    <circle cx="100" cy="100" r="10" fill="#333"/>
    <circle cx="150" cy="60" r="10" fill="#333"/>
    <circle cx="200" cy="120" r="10" fill="#333"/>
    <circle cx="250" cy="70" r="10" fill="#333"/>
    <text x="150" y="170" text-anchor="middle" font-size="12">Broadcast à tous</text>
  </g>
  
  <!-- Réseaux segmentés -->
  <g transform="translate(400,50)">
    <rect x="0" y="0" width="140" height="150" fill="#E6F3FF" stroke="#2196F3"/>
    <text x="70" y="30" text-anchor="middle">Sous-réseau 1</text>
    <circle cx="40" cy="80" r="10" fill="#333"/>
    <circle cx="100" cy="100" r="10" fill="#333"/>
    
    <rect x="160" y="0" width="140" height="150" fill="#E6FFE6" stroke="#4CAF50"/>
    <text x="230" y="30" text-anchor="middle">Sous-réseau 2</text>
    <circle cx="200" cy="80" r="10" fill="#333"/>
    <circle cx="260" cy="100" r="10" fill="#333"/>
    
    <text x="150" y="170" text-anchor="middle" font-size="12">Broadcast limité par sous-réseau</text>
  </g>
</svg>

```

### Le principe du masque

Le masque de sous-réseau fonctionne comme un pochoir qui vient se superposer à l'adresse IP pour distinguer deux parties :
- La partie réseau (identifiant du réseau)
- La partie hôte (identifiant de la machine dans ce réseau)

```svg
<svg viewBox="0 0 800 300" xmlns="http://www.w3.org/2000/svg">
  <!-- IP -->
  <g transform="translate(50,50)">
    <text x="0" y="0">IP: 192.168.1.10</text>
    <text x="0" y="30" font-family="monospace">11000000.10101000.00000001.00001010</text>
    
    <!-- Masque -->
    <text x="0" y="70">Masque: 255.255.255.0 (/24)</text>
    <text x="0" y="100" font-family="monospace">11111111.11111111.11111111.00000000</text>
    
    <!-- Résultat -->
    <line x1="0" y1="120" x2="400" y2="120" stroke="#333"/>
    <text x="0" y="150">Partie réseau: 192.168.1.0</text>
    <text x="0" y="180">Partie hôte: 0.0.0.10</text>
    
    <!-- Indication visuelle -->
    <rect x="0" y="85" width="300" height="20" fill="#E6F3FF" opacity="0.3"/>
    <rect x="300" y="85" width="100" height="20" fill="#FFE6E6" opacity="0.3"/>
  </g>
</svg>

```

### En pratique

Le masque peut être noté de deux façons :
1. Notation décimale pointée : 255.255.255.0
2. Notation CIDR : /24 (indique le nombre de bits à 1 dans le masque)

Prenons un exemple avec le réseau 192.168.1.0/24 :
- Les trois premiers octets (24 bits) définissent le réseau
- Le dernier octet (8 bits) définit les hôtes possibles
- Cela permet d'avoir 254 adresses utilisables (de .1 à .254)
- .0 est réservé pour l'adresse du réseau
- .255 est réservé pour le broadcast

Cette segmentation permet :
- Une meilleure organisation du réseau
- Une réduction du trafic de broadcast
- Une amélioration de la sécurité
- Une simplification de la gestion