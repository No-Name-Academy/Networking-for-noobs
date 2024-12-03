# L'adressage IPv4

## 05. Le broadcast - Un mécanisme fondamental

### L'importance du broadcast dans les réseaux

Le broadcast est un mécanisme essentiel qui permet à une machine de communiquer avec toutes les autres machines d'un même réseau en une seule transmission. C'est comme si vous utilisiez un mégaphone dans une salle : tout le monde vous entend en même temps.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/05-001.svg">

### Le processus de découverte

Quand une machine veut communiquer avec une autre dont elle ne connaît que l'adresse IP, voici ce qui se passe :

1. **Phase de découverte**
   - La machine A cherche à joindre la machine B
   - Ne connaissant pas son adresse MAC, elle envoie une requête ARP en broadcast
   - Toutes les machines du réseau reçoivent cette requête
   - Seule la machine B répond avec son adresse MAC

2. **Phase de mémorisation**
   - La machine A enregistre la correspondance IP/MAC dans sa table ARP
   - Cette information est conservée pendant une durée limitée
   - Les communications suivantes seront directes

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-2-Adressage-ipv4/Sources/05-002.svg">