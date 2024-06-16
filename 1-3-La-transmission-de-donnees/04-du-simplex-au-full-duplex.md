# La transmission de donnée

## 04. Du simplex au full-duplex

### Mode de transmissions en fonction du sens des échanges.

Selon le besoin, il existe plusieurs sens d'échanges, qui vont induire un des 3 modes de transmissions suivants:

- Simplex
- Half duplex
- Full Duplex

En réseau, ces trois notions sont extremements importantes, car bien que les technologies actuelles rendent leur utilisations automatiques, il y a bien négociation sur le mode employé, autrement les échanges ne peuvent aboutir.

### Le mode Simplex (J'envoi simplement dans un sens)

![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/1-3-La-transmission-de-donnees/Ressources-img/598692.jpg)

La transmission simplex est la plus simple, lorsque l'on a juste besoin d'envoyer des informations dans un sens (de l'ordinateur vers son écran par exemple).
Elle est communément utilisée pour les transmissions vers ou depuis des périphériques.

### Le mode Half duplex (J'envoi puis je reçois)

![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/1-3-La-transmission-de-donnees/Ressources-img/598693.jpg)

La transmission half duplex, se caractérise par une transmission dans les deux sens. On commence ici à avoir une notion de réseau, car elle est totalement adaptée pour la communication entre deux hôtes.

La communication se fait donc dans les deux sens, mais chacun son tour.

### Le mode full duplex (J'envoi et je reçois)

![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/1-3-La-transmission-de-donnees/Ressources-img/598694.jpg)

Le full duplex, est le mode de transmission le plus complet, il permet, contrairement au half duplex, de communiquer dans les deux sens en simultané, permettant du coup une plus faible latence.

C'est le mode le plus courant aujourd'hui sur le materiel récent.

### La négociation du mode de transmission en réseau.

Comme précisé avant, le mode de transmission est négocié en amont de la communication entre deux hôtes.

Il s'agit de se mettre d'accord (principalement entre le half et le full duplex, bien que des usages précis de simplex puisse exister) avant de commencer à communiquer.

La non négociation (sur du materiel plus ancien), peut être une cause de panne basique mais complexe à diagnostiquer, il convient donc de ne pas oublier ces notions de base, qui interviennent sur les couches basses du modèle OSI.

Une fois de plus, il s'agit d'une négociation avant d'initier la communication, comme dans la plupart des protocols réseaux, parler de la même chose, de la même manière, dans la même langue, permet de communiquer. L'informatique ne permet pas la couche d'interpretation humaine que nous sommes capable d'avoir dans une conversation, et est donc extremement précise une fois de plus.