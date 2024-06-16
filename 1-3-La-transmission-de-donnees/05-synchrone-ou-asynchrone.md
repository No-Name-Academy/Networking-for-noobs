# La transmission de donnée

## 05. Synchrone ou Asynchrone

### Une problèmatique de la transmission en série:

Lorsque l'on souhaite transmettre des données en série, il existe un soucis de synchronisation entre l'émetteur et le récepteur. Une fois de plus, si le premier et le second ne sont pas "au diapason", ils ne peuvent pas se comprendre.

En effet, le récepteur ne peut à priori pas distinguer les caractères (les séquences de bits), vu que les bits sont envoyés successivement.

Afin de palier à ce problème, il existe deux types de transmission, synchrone et asynchrone.

### La transmission asynchrone

Lors d’une transmission asynchrone, chaque caractère est émis de façon irrégulière dans le temps.

#### Exemple:
Un utilisateur qui envoie en temps réel des caractères saisis au clavier.

On imagine qu'un seul bit est transmis pendant une longue période de silence : le récepteur ne pourrait savoir s'il s'agit de 00010000, ou 10000000 ou encore 00000100.

Afin de remédier à ce problème, chaque caractère :

- est précédé d'une information qui indique le début de la transmission du caractère (l'information de début d'émission est appelée « bit de START ») ;
- et est terminé par l'envoi d'une information de fin de transmission (appelée « bit de STOP »). Il peut y avoir plusieurs bits de STOP.

![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/1-3-La-transmission-de-donnees/Ressources-img/598695.jpg)

Il existe à l'émission et à la réception deux horloges qui doivent fonctionner à la même fréquence. Par contre, ces fréquences peuvent différer de quelques pourcents et, surtout, les horloges n'ont pas besoin d'être synchronisées.

La transmission asynchrone est simple et économique. Elle est utilisée pour transmettre une petite quantité de données.

### La transmission synchrone:

Lors d’une transmission synchrone, l’émetteur et le récepteur sont cadencés sur la même horloge.

Le récepteur reçoit de façon continue (même lorsqu'aucun bit n'est transmis) des informations de l'émetteur. C'est pourquoi il est nécessaire que l’émetteur et le récepteur soient cadencés à la même vitesse. Des informations supplémentaires sont de plus insérées afin de garantir l'absence d'erreurs lors de la transmission.

La transmission synchrone est efficace, fiable et est utilisée pour transférer une grande quantité de données. Elle fournit une communication en temps réel entre les appareils connectés, tels que la vidéoconférence, les conversations téléphoniques, ainsi que les interactions en face à face.

![](https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/main/1-3-La-transmission-de-donnees/Ressources-img/599393.jpg)

### Simplex, duplex et synchrone?

Pour conclure, il est important de retenir pour la partie réseau qui nous concerne, que les technologies d'aujourd'hui utilisent quasi exclusivement des transmissions full duplex synchrone.

Certe plus gourmandes en calcul, les technologies actuelles permettent largement de compenser ce besoin, et le gain en latence et bande passante est suffisamment notable pour le justifier.