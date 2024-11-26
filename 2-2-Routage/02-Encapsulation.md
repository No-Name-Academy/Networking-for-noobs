# Le routage

## 2. L'encapsulation et les en-têtes

### 2.1 Principe de l'encapsulation

Pour comprendre comment les données transitent réellement sur le réseau, nous devons examiner le concept d'encapsulation. Ce mécanisme, directement lié au modèle OSI, est fondamental dans le fonctionnement des réseaux modernes.

Voici une représentation visuelle du processus d'encapsulation :

![Alt text](./02-001.svg)
<img src="./02-001">

L'encapsulation fonctionne comme un système d'enveloppes imbriquées :

- À chaque niveau du modèle OSI, une nouvelle couche d'informations est ajoutée
- Les données d'origine sont progressivement entourées d'en-têtes spécifiques
- Chaque couche traite le contenu des couches supérieures comme de simples données

### 2.2 Structure détaillée d'une trame

Une trame Ethernet est plus complexe qu'elle n'y paraît. Voici sa structure détaillée :

svg 02-002.svg

Dans une trame Ethernet :

1. L'en-tête contient :
   - L'adresse MAC destination (6 octets)
   - L'adresse MAC source (6 octets)
   - Le type de protocole (2 octets)

2. La partie données contient :
   - L'en-tête IP complet
   - L'en-tête de la couche transport
   - Les données applicatives

### 2.3 Observation pratique avec Wireshark

Wireshark est un outil essentiel pour visualiser concrètement l'encapsulation en action. Voyons comment une simple requête web se décompose à travers les différentes couches :

svg 02-003.svg

Lors de l'analyse d'une capture Wireshark, nous pouvons observer :

La décomposition en couches nous permet d'examiner chaque niveau de communication :

- Au niveau de la trame (Frame) : Nous voyons la totalité des données qui transitent sur le réseau, y compris les informations de synchronisation.

- Au niveau Ethernet : Wireshark nous révèle des informations précieuses sur les interfaces réseau :
  - Les adresses MAC source et destination
  - L'identification du fabricant de la carte réseau
  - Le type de protocole encapsulé

- Au niveau IP : Nous observons les détails du routage :
  - Les adresses IP source et destination
  - La taille du paquet
  - Les informations de fragmentation si présentes

### 2.4 La hiérarchie des couches en action

Pour bien comprendre comment ces couches interagissent, observons le parcours d'une requête web :

svg 02-004.svg

Ce processus d'interaction entre les couches se déroule en plusieurs étapes :

1. Au niveau Application :
   - L'utilisateur initie une requête web
   - Les données sont formatées selon le protocole HTTP
   - L'en-tête HTTP est ajouté

2. Au niveau Transport :
   - Les données sont segmentées si nécessaire
   - Un en-tête TCP ou UDP est ajouté
   - Des numéros de séquence sont attribués

3. Au niveau Réseau :
   - L'en-tête IP est ajouté
   - Les adresses source et destination sont définies
   - Le paquet est prêt pour le routage

4. Au niveau Liaison :
   - La trame Ethernet est construite
   - Les adresses MAC sont ajoutées
   - Le CRC est calculé

Cette organisation permet une grande flexibilité : si nous voulons changer la technologie de transmission physique, seule la couche liaison doit être modifiée, les couches supérieures restant inchangées.
