# La transmission de donnée

## 02. Protocole série RS232 - UART

### RS232 Le protocole de base des communications série.

RS-232 est une norme standardisant une voie de communication de type série. Disponible sur presque tous les ordinateur de 1981 jusqu'au milieu des années 2000, les ports RS-232 sont désignés par les noms COM1, COM2, etc. Son remplaçant actuel est le port USB (Universal Serial Bus), et le port RS-232 n'est désormais plus employé que dans des applications professionnelles particulières.

Les liaisons RS-232 sont fréquemment utilisées dans l'industrie pour connecter différents appareils électroniques (automate, appareil de mesure, etc.), c'est également le protocole qui permet d'établir les connexions console sur les materiel réseau.

Au niveau du matériel réseau qui nous concerne aujourd'hui, il a initialement été utilisé avec des connecteurs DB9 et RJ45. Les ports DB9 (communément appelé port COM sur les ordinateurs) ayant disparu avec le temps, au profit des ports USB plus compacts. Aujourd'hui, il est toujours existant, mais le controleur série a été directement intégré dans le materiel réseau, afin de n'avoir besoin que d'un cable usb A / usb Mini, plus standard.

![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/1-3-La-transmission-de-donnees/Ressources-img/599394.png)

Non natif sur les ordinateurs aujourd'hui, le protocole série RS232 nécessite toujours un composant UART, qui permet à l'ordinateur d'avoir une interface "COM" et de communiquer en RS232. Ce composant UART sera soit dans un cable convertisseur USB / DB9, soit dans un cable console USB/RJ45, soit directement intégré dans le materiel réseau (dans un switch derrière le port USB mini).

### Comment ça marche?

Pour établir une communication effective via RS-232, il est nécessaire de définir le protocole utilisé : notamment, le débit de la transmission, le codage utilisé, le découpage en trame, etc. La norme RS-232 laisse ces points libres, mais en pratique on utilise souvent des UART qui découpent le flux en trames d'un caractère ainsi constituées :

- 1 bit de départ ;
- 7 à 8 bits de données ;
- 1 bit de parité optionnel ;
- 1, 1.5 ou 2 bits d'arrêt.
Le bit de départ a un niveau logique "0" tandis que le bit d'arrêt est de niveau logique "1". Le bit de donnée de poids faible est envoyé en premier suivi des autres.

Par exemple, pour générer un signal électrique alternatif carré (rapport cyclique 1:1) sur le port série, il faut imprimer une suite consécutive de U (01010101), ce qui donne dans le temps 0 (départ) 10101010 (U, du LSB au MSB) 1 (arrêt) donc 0101010101 (010101010101010101010101010101 = UI) avec 8 bits de donnée, 1 bit départ, 1 bit arrêt et 0 bit de parité. Les niveaux électriques sont inversés (voir ci-contre).

![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/1-3-La-transmission-de-donnees/Ressources-img/599395.jpg)
Oscillogramme de la transmission du caractère K (01001011), avec un bit de départ et un bit d'arrêt.

### UART l'émetteur-récepteur asynchrone universel

Un UART, pour Universal Asynchronous Receiver Transmitter, est un émetteur-récepteur asynchrone universel. En langage courant, c'est le composant utilisé pour faire la liaison entre l'ordinateur et le port série. L'ordinateur envoie les données en parallèle. Il faut donc transformer ces données pour les faire passer à travers une liaison série qui utilise un seul fil pour faire passer tous les bits de données.

Aujourd'hui, les UART sont généralement intégrés dans des composants comme des microcontrôleurs. Ils ne sont dans ce cas plus un composant à proprement parler, mais une fonction périphérique du composant.
![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/1-3-La-transmission-de-donnees/Ressources-img/599396.png)