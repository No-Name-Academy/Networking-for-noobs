# L'adressage IPv4

## 03. Les réseaux

### Publique / Privé, WAN / LAN.

#### WAN, L'Internet:

L’'intégralité du net fonctionne sur le protocole IP (v4 ou v6) grâce a des IP publiques

#### Le routeur:

Le routeur assure la passerelle entre les différents réseaux, mais surtout entre le LAN et le WAN.

Il est indispensable pour aller sur internet depuis un réseau local

#### LAN, la maison:


Le réseau local LAN est le réseau privé qui est séparé de la zone publique.

Certaines plages d’IP lui sont réservées et ne se retrouveront jamais en WAN.

#### Les plages réservées:

Il existe 3 classes d’IP privée (obsolètes) A B et C

- (Classe A) 10.0.0.0 à 10.255.255.255 soit /8
- (Classe B) 172.16.0.0 à 172.31.255.255 soit /12
- (Classe C) 192.168.0.0 à 192.168.255.255 soit /16

### Les composantes d'un réseau:

Un réseau comprend obligatoirement les éléments suivants:

- Une adresse IP
- Un masque de sous réseau

Le réseau suivant 192.168.0.0/24 signifie:

- Adresse de réseau : 192.168.0.0
- Adresse de Broadcast : 192.168.1.255
- Masque de sous réseau : 255.255.255.0
- Ip disponibles pour les hôtes de 1 à 254

Images ifconfig Linux et ipconfig windows

### Les autres composantes:

Afin d’interconnecté un réseau, d’autres composantes peuvent être nécessaires.

Les deux plus importantes sont:

- L’adresse de passerelle (c’est elle qui connait les routes vers les autres réseaux).
- L’adresse du serveur DNS (C’est lui qui permettra de transformer un FQDN en IP pour la transmission des paquets.