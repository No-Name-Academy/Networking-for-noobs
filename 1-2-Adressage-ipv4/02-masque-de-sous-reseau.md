# L'adressage IPv4

## 02. Le masque de sous réseau

### Pourquoi limiter ?

Le masque de sous-réseau est un concept fondamental qui permet d'organiser et de segmenter efficacement les réseaux. Sans cette segmentation, nous aurions un immense réseau difficile à gérer et à sécuriser.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/02-001.svg">

### Le principe du masque

Le masque de sous-réseau fonctionne comme un pochoir qui vient se superposer à l'adresse IP pour distinguer deux parties :
- La partie réseau (identifiant du réseau)
- La partie hôte (identifiant de la machine dans ce réseau)

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/02-002.svg">

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