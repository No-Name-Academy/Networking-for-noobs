# La transmission de donnée

## 00. Avant de commencer

### Avant propos sur les données

La donnée, c'est l'essence même de pourquoi le réseau existe. Sans données à transmettre, pas besoin de réseau entre les machines.
Ainsi les protocoles de transmission de données sont des protocoles extremement anciens (à l'echelle de l'informatique), et comprendre comment ils ont été mis en place et pourquoi, aide à comprendre pourquoi l'informatique fonctionne comme ça aujourd'hui, mais aussi pourquoi il y a certaines limites.

Du binaire à l'octet, du simplex au full duplex, de manière syncrhone ou asyncrhone, les données transitent sur différentes couches, avec différents protocoles, mais doivent toujours respecter des normes.

### Qu'est ce qu'une donnée

Une donnée, c'est l'information que l'on cherche à stocker, envoyer, recevoir, afficher, modifier.

Afin de l'utiliser, il faut trouver un moyen de la stocker, et de la transmettre. Elle peut être analogique, mais aujourd'hui elle est quasi exclusivement numérique (donc binaire). Il existe néanmoins des travaux sur l'utilisation de l'analogique pour sa simplicité et notamment pour certaines opérations logiques.

Toutefois, le bits n'est pas un objet tangible, et nous devons toujours passer par des supports physiques, et donc leur restrictions correspondantes. Il y a donc toujours une base analogique derrière le numérique (une onde porteuse par exemple) et donc des opérations de modulation/démodulation. Sans aller dans le détail électronique de ce qui peut se passer derrière une transmission de donnée, il est bon d'en connaître l'existence pour comprendre les limites des technologies.

### En réseau, ou se situe-t-on?

En réseau pur, la transmission de données se situe sur plusieurs couches, mais 2 sont plus concernées que d'autres.

1. La couche physique
En effet, la transmission de donnée commence par la couche physique, c'est là que l'on va utiliser le support physique (Le medium, ou média, comme une fibre optique, ou un cable cuivre ethernet) et qu'on va transmettre la donnée dans sa plus simple expression numérique, le bit

2. La couche liaison de données
C'est là qu'on va se mettre d'accord sur les protocoles de transmissions utilisés (duplex, syncrhone etc) et structurer les données.

Ce n'est pas pour rien que ces deux couches sont représentées sur le même niveau dans le modèle TCP contrairement au modèle OSI.