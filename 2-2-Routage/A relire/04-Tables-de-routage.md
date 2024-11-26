# Le routage

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
