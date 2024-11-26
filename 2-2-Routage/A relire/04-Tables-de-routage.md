# Le routage

## 4. Les tables de routage

### 4.1 Principes fondamentaux

La table de routage est le cœur du processus de routage. Elle représente une carte routière que le routeur consulte pour savoir où envoyer les paquets qu'il reçoit. Contrairement à ce qu'on pourrait penser, cette table ne contient pas une entrée pour chaque destination possible sur Internet - ce serait impossible à gérer. Elle fonctionne plutôt avec des réseaux de destination.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-2-Routage/Sources/04-001.svg">

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

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-2-Routage/Sources/04-002.svg">

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

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-2-Routage/Sources/04-003.svg">

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

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-2-Routage/Sources/04-004.svg">

La route par défaut se caractérise par :

- Son réseau de destination : 0.0.0.0/0
- Sa capacité à correspondre à toutes les adresses
- Sa priorité la plus basse dans la table de routage

Dans un réseau d'entreprise typique, la route par défaut pointe généralement vers le routeur qui fournit l'accès à Internet. Cette configuration simple permet de garantir que tout le trafic destiné à Internet trouve son chemin vers l'extérieur du réseau local.

### 4.3 Sélection des routes et prise de décision

La sélection de la meilleure route est un processus crucial qui suit des règles précises. Lorsqu'un paquet arrive, le routeur doit prendre une décision rapide et efficace pour déterminer le meilleur chemin à utiliser.

#### Le processus de décision

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-2-Routage/Sources/04-005.svg">

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

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-2-Routage/Sources/04-006.svg">

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

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-2-Routage/Sources/04-007.svg">

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

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-2-Routage/Sources/04-008.svg">

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

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-2-Routage/Sources/04-009.svg">

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
