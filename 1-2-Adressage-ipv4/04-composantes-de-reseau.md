# L'adressage IPv4

## 04. Les composantes d'un réseau

Pour qu'un réseau soit fonctionnel et permette une communication efficace entre ses différents éléments, plusieurs composantes essentielles doivent être définies et configurées correctement. Chacune joue un rôle spécifique dans le bon fonctionnement de l'ensemble.

### Les éléments fondamentaux

Tout réseau IP nécessite au minimum deux éléments de base :

1. **L'adresse du réseau**
   Cette adresse identifie de manière unique le réseau lui-même. Elle correspond à la première adresse de la plage disponible et ne peut jamais être attribuée à un hôte.

2. **Le masque de sous-réseau**
   Comme nous l'avons vu précédemment, il définit la taille du réseau et permet de distinguer la partie réseau de la partie hôte dans une adresse IP.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/04-001.svg">

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

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/04-002.svg">

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

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/04-003.svg">

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
