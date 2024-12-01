# L'adressage IPv4

## 00. Avant de commencer

### Comment communiquer sur un réseau?

Comment se reconnaître, et comment s'organiser?

Comprendre les bases du protocole TCP/IP, pourquoi il a été mis en place et comment le sécuriser simplement.

Le coeur du réseau est ici, sur la couche IP, et dans ce qui nous concernera en IPv4.

Adressage ip, masquage, broadcast, et surtout réseau IPv4, comment est-ce que tous ces éléments s'articulent, commment sont-ils codifiés, et enfin, comment le utiliser et les calculer. Une fois ce module passé, vous aurez tout les éléments en main pour devenir un expert de l'adressage IPv4.

## 01. L'adresse IP

### Pourquoi une IP?

#### Communiquer = adresser

Sans adresse, impossible de savoir ou envoyer les informations ni même les recevoir en retour.

#### Standardisation

Avec les réseaux, sont nés les protocoles. La nature même du réseau intercommuniquant entre machines / pays différents, oblige à une standardisation mondiale.

#### Le protocole TCP/IP

Assure la bonne transmission des informations en les découpant en paquets. Vérifie que les paquets sont complets et intègres.

### Comment se compose une ip

Une adresse IP est composée d’une série de bit, regroupés en Octets (8 bits), chaque octet représentant une notation décimale, et séparé du suivant par un point.

Ainsi dans un paquet IPV4, on trouvera l’adresse ip de destination et source du message (le protocole TCP exigeant un retour, la source est indispensable).

Images octet
image adresse ipv4 notation décimale à point

### La correspondance IP / MAC

Quand une machine initie une communication, elle utilise l’'adresse de Broadcast pour "demander” qui est là.

Une fois que la réponse sera reçu, elle pourra ainsi stocker un cache de la réponse, afin de retenir (un certain temps) l’adresse machine correspondante.

Ces informations sont stockées dans la table ARP.

Une table ARP peut être sur un hôte (Ordinateur) mais également sur un switch, notamment L3.

Image Table ARP Linux et windows

## 02. Le masque de sous réseau

### Pourquoi limiter?

#### Diviser pour mieux reigner.

Afin d’'organiser, il est vite apparu qu’il fallait ségréguer les réseaux, afin de mieux segmenter l’'information, et de ne pas saturer le broadcast.

#### La solution du masque.

Le masque, permet de “couvrir” une partie de l’adresse IP afin de la définir comme base.

Dans 192.168.1.0/24 on autorise toutes les ip entre 192.168.1.1 et 192.168.1.254.

#### Tout est dans le bit.

Le masque prend tout son sens lorsqu'il est exprimé en binaire et non en décimal.

Ainsi, savoir lire une ip binaire a son importance ici.

#### Comment fonctionne un masque.

Images masque binaire.

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

## 04. Le broadcast

### Une adresse importante.

Historiquement, les anciennes topologies et technologies physiques faisaient transiter les informations a transmettre a travers TOUT les hotes. Peu pratique avec le grandissement des infrastructures et le volume de data grandissant, le systeme broadcast / Table ARP est devenu essentiel dans les reseau actuels.

Il permet d’eviter de saturer l’integralite du reseau et de ses hotes de paquets indesirable, et d’eviter des colisions de paquets, et donc de la latence.

### Comment communique-t-on?

Initier une communication dans un reseau est simple.

On ne connait rien ni personnes, et c’est la que le broadcast sert. L’adresse de broadcast est “ecoutée” par tous les hotes. Ainsi, une fois que le destinataire du paquet le recoit et le confirme (Aknowledge) il en profite pour enregistrer une entree ARP correspondant a l’expediteur.

Une fois la confirmation recue par l’expediteur, lui meme enregistre une entree ARP corresondant au destinataire.

Ainsi, lors du prochain echange, le broadcast ne sera plus necessaire.

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
