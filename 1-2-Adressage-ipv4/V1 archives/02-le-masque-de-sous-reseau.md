# L'adressage IPv4

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

