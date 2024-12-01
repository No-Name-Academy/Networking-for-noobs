# L'adressage IPv4

## 06. Les Vlans

### Encapsulation

Lors de la segmentation en vlan, les paquets sont encapsules avec un “tag” correspondant au vlan.
La notion TAG et UNTAGED est très utilisée chez tous les autres fabricants que Cisco.
Chez Cisco, on va plus parler d’encapsulation DOT1Q (en référence a la norme 802.1Q) et de mode Access ou Trunk.
Ainsi seuls les interfaces “Taguées” avec le VLAN correspondant au Tag du paquet peuvent le transmettre.

### Mode Access ou Trunk

Chez Cisco, on utilisera le système Acces vs Trunk afin de décider comment transférer les données entre vlans et Switchs.

Soit une interface est en mode access sur un vlan spécifique pour ne laisser passer que ce Vlan, soit elle est en mode trunk et laissera passer plusieurs Vlans (tous ou certains).
Chez la plupart des autres fabricants, les interfaces seront Taguées avec le ou les id des Vlans qui sont autorises.
Sinon on les laissera en mode Untaged.
