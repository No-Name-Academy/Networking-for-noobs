# L'adressage IPv4

## 04. Le broadcast

### Une adresse importante.

Historiquement, les anciennes topologies et technologies physiques faisaient transiter les informations a transmettre a travers TOUT les hotes. Peu pratique avec le grandissement des infrastructures et le volume de data grandissant, le systeme broadcast / Table ARP est devenu essentiel dans les reseau actuels.

Il permet d’eviter de saturer l’integralite du reseau et de ses hotes de paquets indesirable, et d’eviter des colisions de paquets, et donc de la latence.

### Comment communique-t-on?

Initier une communication dans un reseau est simple.

On ne connait rien ni personnes, et c’est la que le broadcast sert. L’adresse de broadcast est “ecoutée” par tous les hotes. Ainsi, une fois que le destinataire du paquet le recoit et le confirme (Aknowledge) il en profite pour enregistrer une entree ARP correspondant a l’expediteur.

Une fois la confirmation recue par l’expediteur, lui meme enregistre une entree ARP corresondant au destinataire.

Ainsi, lors du prochain echange, le broadcast ne sera plus necessaire.

