### 3. Les pare-feux classiques

#### Définition et fonctionnement des pare-feux classiques

Les pare-feux classiques, également appelés pare-feux de première génération, sont des dispositifs de sécurité qui surveillent et contrôlent le trafic réseau en fonction de règles prédéfinies. Pensez à eux comme à des gardiens postés aux portes d'une ville, vérifiant chaque véhicule et chaque piéton avant de leur permettre d'entrer ou de sortir. Ces pare-feux prennent leurs décisions en se basant sur les informations contenues dans les en-têtes des paquets de données, telles que les adresses IP source et destination, ainsi que les numéros de port.

Les pare-feux classiques opèrent principalement au niveau de la couche réseau (couche 3) et de la couche transport (couche 4) du modèle OSI. Ils filtrent le trafic en autorisant ou en bloquant les paquets en fonction de critères simples, tels que les adresses IP, les ports et les protocoles.

#### Filtrage par paquet

Le filtrage par paquet est la méthode de base utilisée par les pare-feux classiques pour contrôler le trafic réseau. Chaque paquet de données entrant ou sortant est comparé à un ensemble de règles définies par l'administrateur réseau. Voici comment cela fonctionne en pratique :

1. **Examen des en-têtes** : Lorsque le pare-feu reçoit un paquet de données, il examine les informations contenues dans les en-têtes du paquet, telles que l'adresse IP source, l'adresse IP destination, les numéros de port source et destination, et le protocole utilisé.
2. **Comparaison avec les règles** : Le pare-feu compare ces informations avec les règles de filtrage configurées. Chaque règle spécifie si le trafic correspondant doit être autorisé ou bloqué.
3. **Décision de filtrage** : Si le paquet de données correspond à une règle autorisant le trafic, il est transmis à sa destination. Sinon, il est bloqué et supprimé.

Par exemple, une règle de filtrage peut spécifier que tout le trafic provenant de l'adresse IP 192.168.1.10 et destiné au port 80 (HTTP) doit être autorisé, tandis que tout le trafic provenant de l'adresse IP 192.168.1.20 doit être bloqué.

#### Configuration de base d'un pare-feu classique

Configurer un pare-feu classique implique plusieurs étapes pour définir et appliquer les règles de filtrage. Voici un guide général pour la configuration de base d'un pare-feu classique sur un routeur ou un dispositif de sécurité dédié :

1. **Accéder à l'interface de configuration** :
   - Connectez-vous au dispositif de pare-feu via une interface web ou une ligne de commande.

2. **Définir les règles de filtrage** :
   - Créez des règles de filtrage en spécifiant les critères de trafic à autoriser ou à bloquer. Par exemple :
     ```plaintext
     permit tcp 192.168.1.0/24 any eq 80
     deny tcp 192.168.1.20 any
     ```

3. **Appliquer les règles à une interface** :
   - Associez les règles de filtrage à l'interface réseau appropriée pour contrôler le trafic entrant ou sortant. Par exemple :
     ```plaintext
     interface GigabitEthernet0/0
     ip access-group 100 in
     ```

4. **Vérifier la configuration** :
   - Examinez et testez la configuration pour vous assurer que les règles de filtrage fonctionnent comme prévu.

#### Cas d'utilisation et exemples de pare-feux classiques

Les pare-feux classiques sont utilisés dans divers scénarios pour protéger les réseaux et contrôler le trafic. Voici quelques exemples de cas d'utilisation courants :

- **Sécurisation des périmètres réseau** : Les pare-feux classiques sont souvent déployés à la frontière entre un réseau interne et Internet pour bloquer les accès non autorisés et surveiller le trafic entrant et sortant.
- **Filtrage du trafic interne** : Ils peuvent également être utilisés pour segmenter les réseaux internes, en contrôlant le trafic entre différents segments de réseau pour prévenir les mouvements latéraux des menaces.
- **Protection des serveurs publics** : Les pare-feux classiques peuvent être configurés pour protéger les serveurs exposés à Internet, en autorisant uniquement le trafic légitime et en bloquant les tentatives d'accès non autorisées.

#### Avantages et inconvénients des pare-feux classiques

**Avantages** :
- **Simplicité** : Les pare-feux classiques sont relativement simples à configurer et à gérer, ce qui en fait une solution accessible pour de nombreux environnements.
- **Efficacité pour le filtrage de base** : Ils sont efficaces pour contrôler le trafic en fonction de critères simples comme les adresses IP et les ports.
- **Coût** : Les pare-feux classiques sont souvent moins coûteux que les solutions de pare-feux de nouvelle génération.

**Inconvénients** :
- **Limitation de la granularité** : Les pare-feux classiques ne peuvent pas inspecter le contenu des paquets de données, ce qui limite leur capacité à détecter et à bloquer des menaces plus sophistiquées.
- **Incapacité à gérer le trafic chiffré** : Ils ne peuvent pas analyser le trafic chiffré, ce qui laisse une faille potentielle pour les attaques dissimulées.
- **Gestion et mise à jour manuelle** : Les règles de filtrage doivent être mises à jour manuellement, ce qui peut devenir fastidieux dans des environnements réseau complexes et en évolution rapide.

En dépit de leurs limites, les pare-feux classiques restent un élément important de la stratégie de sécurité des réseaux, en offrant une première ligne de défense contre les menaces.