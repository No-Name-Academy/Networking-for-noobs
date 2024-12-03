# L'adressage IPv4

## Introduction : Avant de commencer

Le réseau est souvent comparé à un système postal : pour communiquer efficacement, chaque destinataire doit avoir une adresse unique et comprendre comment les messages sont acheminés. L'adressage IPv4, pierre angulaire d'Internet, répond précisément à ce besoin.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/00-001.svg">

### Pourquoi un protocole réseau ?

La communication en réseau pose plusieurs défis fondamentaux. Comment s'assurer que chaque machine peut être identifiée de manière unique ? Comment garantir que les messages arrivent à destination ? Comment organiser efficacement ces échanges ?

Le protocole TCP/IP répond à ces questions en proposant une architecture en couches, où chaque niveau a une responsabilité spécifique. L'IPv4, situé dans la couche Internet, s'occupe spécifiquement de l'adressage et du routage des paquets à travers les réseaux.

### Les bases de TCP/IP

Le protocole TCP/IP fonctionne comme un système postal sophistiqué :

1. **L'adressage** : Chaque machine reçoit une adresse IP unique, comme une adresse postale
2. **Le routage** : Les paquets sont acheminés de routeur en routeur, comme des lettres passant par différents centres de tri
3. **La fiabilité** : TCP s'assure que tous les paquets arrivent et dans le bon ordre
4. **La standardisation** : Des règles communes permettent à des équipements différents de communiquer

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/00-002.svg">

Dans ce module, nous allons nous concentrer sur l'adressage IPv4, élément fondamental pour comprendre comment :
- Les machines s'identifient sur le réseau
- Les réseaux sont organisés et segmentés
- Le routage permet la communication entre différents réseaux
- La sécurité peut être mise en place au niveau réseau