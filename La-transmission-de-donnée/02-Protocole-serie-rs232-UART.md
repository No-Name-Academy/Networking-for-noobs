# La transmission de donnée

## 02. Protocole série RS232 - UART

### RS232 Le protocole de base des communications série.

RS-232 est une norme standardisant une voie de communication de type série. Disponible sur presque tous les ordinateur de 1981 jusqu'au milieu des années 2000, les ports RS-232 sont désignés par les noms COM1, COM2, etc. Son remplaçant actuel est le port USB (Universal Serial Bus), et le port RS-232 n'est désormais plus employé que dans des applications professionnelles particulières.

Les liaisons RS-232 sont fréquemment utilisées dans l'industrie pour connecter différents appareils électroniques (automate, appareil de mesure, etc.), c'est également le protocole qui permet d'établir les connexions console sur les materiel réseau.

Au niveau du matériel réseau qui nous concerne aujourd'hui, il a initialement été utilisé avec des connecteurs DB9 et RJ45. Les ports DB9 (communément appelé port COM sur les ordinateurs) ayant disparu avec le temps, au profit des ports USB plus compacts. Aujourd'hui, il est toujours existant, mais le controleur série a été directement intégré dans le materiel réseau, afin de n'avoir besoin que d'un cable usb A / usb Mini, plus standard.

Non natif sur les ordinateurs aujourd'hui, le protocole série RS232 nécessite toujours un composant UART, qui permet à l'ordinateur d'avoir une interface "COM" et de communiquer en RS232. Ce composant UART sera soit dans un cable convertisseur USB / DB9, soit dans un cable console USB/RJ45, soit directement intégré dans le materiel réseau (dans un switch derrière le port USB mini).

### Comment ça marche?

### UART l'émetteur-récepteur asynchrone universel
