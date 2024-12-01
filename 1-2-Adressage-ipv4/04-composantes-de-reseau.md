# L'adressage IPv4

## 04. Les composantes d'un réseau

Pour qu'un réseau soit fonctionnel et permette une communication efficace entre ses différents éléments, plusieurs composantes essentielles doivent être définies et configurées correctement. Chacune joue un rôle spécifique dans le bon fonctionnement de l'ensemble.

### Les éléments fondamentaux

Tout réseau IP nécessite au minimum deux éléments de base :

1. **L'adresse du réseau**
   Cette adresse identifie de manière unique le réseau lui-même. Elle correspond à la première adresse de la plage disponible et ne peut jamais être attribuée à un hôte.

2. **Le masque de sous-réseau**
   Comme nous l'avons vu précédemment, il définit la taille du réseau et permet de distinguer la partie réseau de la partie hôte dans une adresse IP.

```svg
<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Réseau principal -->
  <rect x="50" y="50" width="700" height="300" fill="#f5f5f5" stroke="#ddd"/>
  <text x="400" y="80" text-anchor="middle" font-weight="bold">Réseau 192.168.1.0/24</text>
  
  <!-- Adresse réseau -->
  <rect x="100" y="100" width="200" height="80" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="200" y="130" text-anchor="middle">Adresse réseau</text>
  <text x="200" y="150" text-anchor="middle" font-size="12">192.168.1.0</text>
  
  <!-- Broadcast -->
  <rect x="500" y="100" width="200" height="80" fill="#FFE6E6" stroke="#F44336"/>
  <text x="600" y="130" text-anchor="middle">Broadcast</text>
  <text x="600" y="150" text-anchor="middle" font-size="12">192.168.1.255</text>
  
  <!-- Plage d'hôtes -->
  <rect x="100" y="220" width="600" height="80" fill="#E6FFE6" stroke="#4CAF50"/>
  <text x="400" y="250" text-anchor="middle">Plage d'adresses utilisables</text>
  <text x="400" y="270" text-anchor="middle" font-size="12">192.168.1.1 - 192.168.1.254</text>
  
  <!-- Flèches -->
  <path d="M200,180 L200,220" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M600,180 L600,220" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
</svg>

```

Prenons l'exemple concret du réseau 192.168.1.0/24 :

- **Adresse du réseau** : 192.168.1.0
  - Première adresse de la plage
  - Identifie uniquement le réseau
  - Ne peut être attribuée à aucun équipement

- **Adresse de broadcast** : 192.168.1.255
  - Dernière adresse de la plage
  - Utilisée pour la diffusion à tous les hôtes
  - Messages reçus par tous les équipements du réseau

- **Plage d'adresses utilisables** : 192.168.1.1 à 192.168.1.254
  - 254 adresses disponibles pour les équipements
  - Chaque adresse doit être unique dans le réseau
  - Configuration possible en statique ou en dynamique (DHCP)

### Les composantes additionnelles essentielles

Pour permettre une connectivité complète, notamment vers l'extérieur du réseau, d'autres éléments sont nécessaires :

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Gateway -->
  <rect x="50" y="50" width="300" height="100" fill="#FFF3E0" stroke="#FF9800"/>
  <text x="200" y="85" text-anchor="middle">Passerelle (Gateway)</text>
  <text x="200" y="105" text-anchor="middle" font-size="12">192.168.1.254</text>
  <text x="200" y="125" text-anchor="middle" font-size="10">Route vers les autres réseaux</text>
  
  <!-- DNS -->
  <rect x="450" y="50" width="300" height="100" fill="#E1BEE7" stroke="#9C27B0"/>
  <text x="600" y="85" text-anchor="middle">Serveur DNS</text>
  <text x="600" y="105" text-anchor="middle" font-size="12">192.168.1.253</text>
  <text x="600" y="125" text-anchor="middle" font-size="10">Résolution des noms de domaine</text>
  
  <!-- Internet -->
  <circle cx="400" cy="250" r="50" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="400" y="255" text-anchor="middle">Internet</text>
  
  <!-- Connexions -->
  <path d="M200,150 L400,200" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M600,150 L400,200" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
</svg>

```


### Les composantes additionnelles en détail

#### La passerelle (Gateway)
La passerelle est un élément crucial qui joue le rôle de "porte de sortie" du réseau local. Son fonctionnement est plus complexe qu'il n'y paraît :

- **Rôle principal** :
  * Fait le lien entre le réseau local et les réseaux externes
  * Connaît les routes vers les autres réseaux
  * Traduit les adresses privées en adresses publiques (NAT)
  * Applique les règles de sécurité (firewall)

- **Configuration type** :
  La passerelle prend généralement la première ou la dernière adresse utilisable du réseau pour être facilement mémorisable. Par exemple :
  * Dans un réseau 192.168.1.0/24 : 192.168.1.254
  * Dans un réseau 10.0.0.0/24 : 10.0.0.1

#### Le serveur DNS (Domain Name System)

Le DNS est souvent considéré comme "l'annuaire d'Internet". Son rôle est essentiel pour la navigation web et les services réseau :

```svg
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Client -->
  <rect x="50" y="150" width="120" height="60" fill="#E6F3FF" stroke="#2196F3"/>
  <text x="110" y="185" text-anchor="middle">Client</text>
  
  <!-- DNS Local -->
  <rect x="300" y="150" width="120" height="60" fill="#E6FFE6" stroke="#4CAF50"/>
  <text x="360" y="185" text-anchor="middle">DNS Local</text>
  
  <!-- DNS Internet -->
  <rect x="550" y="150" width="120" height="60" fill="#FFE6E6" stroke="#F44336"/>
  <text x="610" y="185" text-anchor="middle">DNS Internet</text>
  
  <!-- Requêtes -->
  <path d="M170,180 L300,180" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <text x="235" y="160" font-size="12">1. www.example.com ?</text>
  
  <path d="M420,180 L550,180" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <text x="485" y="160" font-size="12">2. Recherche</text>
  
  <path d="M550,200 L420,200" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <text x="485" y="220" font-size="12">3. 93.184.216.34</text>
  
  <path d="M300,200 L170,200" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)"/>
  <text x="235" y="220" font-size="12">4. 93.184.216.34</text>
  
  <!-- Marqueur de flèche -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333"/>
    </marker>
  </defs>
</svg>

```

- **Fonctions principales** :
  * Traduit les noms de domaine en adresses IP
  * Maintient un cache des résolutions fréquentes
  * Peut filtrer l'accès à certains sites
  * Optimise la navigation en réduisant les temps de résolution

- **Configuration courante** :
  * DNS primaire : souvent l'adresse du routeur ou d'un serveur local
  * DNS secondaire : généralement fourni par le FAI ou un service public (8.8.8.8 pour Google)

#### Les services additionnels courants

D'autres services peuvent être nécessaires selon les besoins :

1. **Serveur DHCP**
   - Attribue automatiquement les configurations IP
   - Évite les conflits d'adresses
   - Simplifie l'administration du réseau
   - Gère les baux d'adresses IP

2. **Serveur de temps (NTP)**
   - Synchronise les horloges des équipements
   - Important pour la journalisation
   - Crucial pour certains protocoles de sécurité
