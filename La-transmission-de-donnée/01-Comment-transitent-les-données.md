# La transmission de donnée

## 01. Comment transitent les données

### Bien comprendre comment se structurent les données pour comprendre comment elles transitent

Afin de simplifier le concept, nous allons rester sur une transmission de donnée simple, basé sur des octets, donc codé sur 8 bits, de la droite vers la gauche (sens normal de la transmission)

![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/La-transmission-de-donn%C3%A9e/Ressources-img/598688.png)

Dans cet exemple basique, on lit l'octet de la droite vers la gauche, grace au même tableau de conversion que pour le calcul des adresses IPv4.

En effet, c'est le sens naturel des données, car le bit de droit (bit de poids faible, 1 en décimal) est le premier qui arrive dans le temps, à l'inverse de celui de gauche (bit de poids fort, 128 en décimal).

### Le masque, un notion utile aussi en transmission de donnée

De la même manière que l'on utilise des masques en réseau, les masques binaires sont aussi existant pour la transmission de données, ainsi, une donnée binaire passé dans un masque en resortira masqué d'une partie.

![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/La-transmission-de-donn%C3%A9e/Ressources-img/598689.png)

Cela peut permettre d'effectuer des opérations basiques sur des octets, sans même passer par un calcul logiciel, comme cela se produit sur les IPv4 justement.

### Transmission parallèle ou série, comment faire transiter les informations?

Partant des informations vu précedemment, il existe deux manière de faire transiter les informations.
En effet, un processeur (unité de traitement de l’information) ne traite jamais un seul bit à la fois, il permet généralement d’en traiter plusieurs (8 bits, soit un octet, mais aussi 16 bits, 32 bits ou 64 bits), c’est la raison pour laquelle la transmission de base sur un ordinateur est une transmission parallèle.

1. Transmission parallèle: on désigne par transmission parallèle la transmission simultanée de N bits. Ces bits sont envoyés simultanément sur N voies différentes (une voie étant par exemple un fil). Les câbles parallèles sont composés de plusieurs fils en nappe.
![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/La-transmission-de-donn%C3%A9e/Ressources-img/598690.png)
Ce type de communication n’est utilisé que sur de courtes distances (à l'intérieur d'un ordinateur)

2. Transmission en série: dans une transmission série, les données sont envoyées bit par bit sur la voie de transmission.
![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/La-transmission-de-donn%C3%A9e/Ressources-img/598691.png)
C’est le type de communication utilisé pour relier des appareils en utilisant, par exemple, les ports USB (Universal Serial Bus) de l’ordinateur.

On retrouve ainsi les deux modes de transmission, mixés en fonction du besoin, avec la transmission parallèle pour la transmission de données ultra rapide au sein des unités de calcul, ou au sein d'un ordinateur (la raison même de l'existence et de la rapidité des bus PCI et PCIe). Pratique sur de courte distance, car extremement gourmant en "fils" de transmissions (8 ou un octet, ou 16, 32, et même 64 pour les processeurs 64 bits actuels).
Pour les transmissions de données qui nous concernent, en réseau, c'est un bus série qui est utilisé, de plus en plus complexe avec l'évolution des technologies, le principe de base reste le même, et le binaire est toujours la forme initiale de l'information, codée sur un octet dans la plupart des cas.
