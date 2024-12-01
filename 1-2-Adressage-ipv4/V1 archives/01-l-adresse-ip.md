# L'adressage IPv4

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

