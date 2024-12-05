# L'adressage IPv4

## 05. Découper les réseaux

### Decoupage par masque

Comme évoquée auparavant dans le cours, il est possible de découper les réseaux par une simple application de masque.
Ainsi, un reseau general 192.168.0.0/16 peut contenir une multitude de sous reseau en /24

- 192.168.1.0/24
- 192.168.2.0/24
- 192.168.xxx.0/24

Le decoupage par masque permet egalement dedecouper en plages intermediaires (VLSM).

### Avantages et limites

Un découpage par masque est pratique et simple a mettre en place.
Découper un réseau en plusieurs sous réseau de la bonne taille permet une gestion basique simple des sécurités de bases. On ne donne que les accès nécessaires.
Cependant, il suffit de connaitre le masque pour rentrer sur le réseau.

Egalement, on pourrait masquer au plus large, mais on ne pourra alors plus vérifier simplement les intrusions.

### Réseau Local Virtuel (VLANS)

Afin de découper les réseaux de manière sécurisées, et pour éviter d’avoir à doubler les infrastructures physiques, les VLANS permettent d’avoir une virtualisation de réseau, qui seront logiciellement séparés comme s’ils l’étaient physiquement.

La notion de Vlans vient avec celle de TRUNK (802.1Q) sans laquelles la communication des vlans entre switchs physiques serait impossible.

