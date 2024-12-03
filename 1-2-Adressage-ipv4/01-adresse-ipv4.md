# L'adressage IPv4

## 01. L'adresse IP

### Pourquoi une adresse IP ?

Une adresse IP joue un rôle similaire à une adresse postale dans le monde réel : elle permet d'identifier de manière unique chaque "habitant" du réseau et de lui acheminer les informations qui lui sont destinées.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/01-001.svg">

### Structure d'une adresse IPv4

Une adresse IPv4 se compose de 32 bits, organisés en quatre octets (groupes de 8 bits). Pour faciliter la lecture, nous utilisons une notation décimale pointée, où chaque octet est converti en nombre décimal (de 0 à 255) et séparé par des points.

Prenons l'exemple de l'adresse 192.168.1.10 :
1. Premier octet : 192 (11000000 en binaire)
2. Deuxième octet : 168 (10101000 en binaire)
3. Troisième octet : 1 (00000001 en binaire)
4. Quatrième octet : 10 (00001010 en binaire)

### La correspondance IP/MAC

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/01-002.svg">

Le protocole ARP (Address Resolution Protocol) permet d'établir la correspondance entre les adresses IP (couche 3) et les adresses MAC (couche 2). Ce processus est essentiel car les communications physiques sur un réseau local se font toujours via les adresses MAC.

Le processus se déroule en deux temps :
1. Une machine envoie une requête ARP en broadcast : "Qui possède cette adresse IP ?"
2. La machine concernée répond avec son adresse MAC
3. Les informations sont stockées dans une table ARP temporaire