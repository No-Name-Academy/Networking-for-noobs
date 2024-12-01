# L'adressage IPv4

## 03. Les réseaux

### Publique / Privé, WAN / LAN

L'Internet est un immense réseau de réseaux, où coexistent des réseaux publics et privés. Comprendre leur distinction est fondamental pour la sécurité et l'organisation des infrastructures.

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Internet (WAN) -->
  <circle cx="400" cy="100" r="80" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="400" y="100" text-anchor="middle">Internet (WAN)</text>
  <text x="400" y="120" text-anchor="middle" font-size="12">IPs Publiques</text>
  
  <!-- Routeur -->
  <rect x="350" y="200" width="100" height="60" fill="#4CAF50" stroke="#333"/>
  <text x="400" y="235" text-anchor="middle">Routeur</text>
  
  <!-- LANs -->
  <g transform="translate(100,300)">
    <rect width="200" height="100" fill="#FFE6E6" stroke="#F44336"/>
    <text x="100" y="40" text-anchor="middle">LAN Privé 1</text>
    <text x="100" y="60" text-anchor="middle" font-size="12">192.168.1.0/24</text>
  </g>
  
  <g transform="translate(500,300)">
    <rect width="200" height="100" fill="#FFE6E6" stroke="#F44336"/>
    <text x="100" y="40" text-anchor="middle">LAN Privé 2</text>
    <text x="100" y="60" text-anchor="middle" font-size="12">10.0.0.0/8</text>
  </g>
  
  <!-- Connexions -->
  <line x1="400" y1="180" x2="400" y2="200" stroke="#333" stroke-width="2"/>
  <path d="M400,260 L200,300" stroke="#333" stroke-width="2"/>
  <path d="M400,260 L600,300" stroke="#333" stroke-width="2"/>
</svg>

```

#### Le WAN (Wide Area Network)
L'Internet public fonctionne exclusivement avec des adresses IP publiques, uniques à l'échelle mondiale. Ces adresses sont :
- Attribuées par les registres Internet régionaux (RIR)
- Routables sur Internet
- Généralement payantes et en nombre limité
- Directement accessibles depuis n'importe où sur Internet

#### Le LAN (Local Area Network)
Les réseaux locaux utilisent des plages d'adresses privées réservées :

1. **Classe A**
   - Plage : 10.0.0.0 à 10.255.255.255
   - Masque : /8
   - Capacité : Plus de 16 millions d'adresses

2. **Classe B**
   - Plage : 172.16.0.0 à 172.31.255.255
   - Masque : /12
   - Capacité : Environ 1 million d'adresses

3. **Classe C**
   - Plage : 192.168.0.0 à 192.168.255.255
   - Masque : /16
   - Capacité : 65 534 adresses

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Échelle des adresses -->
  <rect x="50" y="50" width="700" height="200" fill="#f5f5f5" stroke="#ddd"/>
  
  <!-- Classe A -->
  <rect x="100" y="100" width="200" height="40" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="200" y="125" text-anchor="middle">Classe A (10.0.0.0/8)</text>
  
  <!-- Classe B -->
  <rect x="320" y="100" width="150" height="40" fill="#E6FFE6" stroke="#4CAF50"/>
  <text x="395" y="125" text-anchor="middle">Classe B</text>
  <text x="395" y="140" text-anchor="middle" font-size="10">172.16.0.0/12</text>
  
  <!-- Classe C -->
  <rect x="490" y="100" width="100" height="40" fill="#FFE6E6" stroke="#F44336"/>
  <text x="540" y="125" text-anchor="middle">Classe C</text>
  <text x="540" y="140" text-anchor="middle" font-size="10">192.168.0.0/16</text>
</svg>

```
