# L'adressage IPv4

## 05. Le broadcast - Un mécanisme fondamental

### L'importance du broadcast dans les réseaux

Le broadcast est un mécanisme essentiel qui permet à une machine de communiquer avec toutes les autres machines d'un même réseau en une seule transmission. C'est comme si vous utilisiez un mégaphone dans une salle : tout le monde vous entend en même temps.

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Réseau -->
  <rect x="50" y="50" width="700" height="300" fill="#f5f5f5" stroke="#ddd"/>
  
  <!-- Machine émettrice -->
  <circle cx="400" cy="200" r="30" fill="#4CAF50"/>
  <text x="400" cy="200" text-anchor="middle">PC1</text>
  
  <!-- Machines réceptrices -->
  <circle cx="200" cy="100" r="20" fill="#2196F3"/>
  <circle cx="600" cy="100" r="20" fill="#2196F3"/>
  <circle cx="200" cy="300" r="20" fill="#2196F3"/>
  <circle cx="600" cy="300" r="20" fill="#2196F3"/>
  
  <!-- Ondes de broadcast -->
  <circle cx="400" cy="200" r="50" fill="none" stroke="#4CAF50" opacity="0.3">
    <animate attributeName="r" values="50;200" dur="2s" repeatCount="indefinite"/>
    <animate attributeName="opacity" values="0.3;0" dur="2s" repeatCount="indefinite"/>
  </circle>
  
  <!-- Connexions -->
  <line x1="400" y1="200" x2="200" y2="100" stroke="#333" stroke-width="1" opacity="0.3"/>
  <line x1="400" y1="200" x2="600" y2="100" stroke="#333" stroke-width="1" opacity="0.3"/>
  <line x1="400" y1="200" x2="200" y2="300" stroke="#333" stroke-width="1" opacity="0.3"/>
  <line x1="400" y1="200" x2="600" y2="300" stroke="#333" stroke-width="1" opacity="0.3"/>
</svg>

```

### Le processus de découverte

Quand une machine veut communiquer avec une autre dont elle ne connaît que l'adresse IP, voici ce qui se passe :

1. **Phase de découverte**
   - La machine A cherche à joindre la machine B
   - Ne connaissant pas son adresse MAC, elle envoie une requête ARP en broadcast
   - Toutes les machines du réseau reçoivent cette requête
   - Seule la machine B répond avec son adresse MAC

2. **Phase de mémorisation**
   - La machine A enregistre la correspondance IP/MAC dans sa table ARP
   - Cette information est conservée pendant une durée limitée
   - Les communications suivantes seront directes

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Timeline -->
  <line x1="100" y1="100" x2="700" y2="100" stroke="#333" stroke-width="2"/>
  
  <!-- Machine A -->
  <rect x="50" y="150" width="100" height="60" fill="#4CAF50" stroke="#333"/>
  <text x="100" y="185" text-anchor="middle">Machine A</text>
  
  <!-- Machine B -->
  <rect x="650" y="150" width="100" height="60" fill="#2196F3" stroke="#333"/>
  <text x="700" y="185" text-anchor="middle">Machine B</text>
  
  <!-- Étapes -->
  <g transform="translate(0,250)">
    <rect x="150" y="0" width="500" height="200" fill="#f5f5f5" stroke="#ddd"/>
    
    <text x="170" y="30">1. Broadcast requête ARP</text>
    <text x="170" y="50" font-size="12">Who has 192.168.1.10?</text>
    
    <text x="170" y="90">2. Réponse unicast</text>
    <text x="170" y="110" font-size="12">192.168.1.10 is at 00:11:22:33:44:55</text>
    
    <text x="170" y="150">3. Mise à jour table ARP</text>
    <text x="170" y="170" font-size="12">Cache: 5-10 minutes</text>
  </g>
  
  <!-- Flèches -->
  <path d="M150,180 L650,180" stroke="#F44336" stroke-width="2" stroke-dasharray="5,5">
    <animate attributeName="stroke-dashoffset" from="10" to="0" dur="2s" repeatCount="indefinite"/>
  </path>
  
  <path d="M650,200 L150,200" stroke="#4CAF50" stroke-width="2">
    <animate attributeName="stroke-dashoffset" from="10" to="0" dur="2s" repeatCount="indefinite"/>
  </path>
</svg>

```
