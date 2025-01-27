## 2. Les listes de contrôle d'accès (ACL) sur les routeurs

### Définition et types d'ACL (standard et étendu)

Les listes de contrôle d'accès (ACL) sont des ensembles de règles qui permettent de contrôler le trafic entrant et sortant sur un routeur ou un autre dispositif réseau. Elles sont analogues à des gardiens postés à chaque porte d'entrée d'un château, examinant chaque individu et ne laissant passer que ceux qui respectent des critères spécifiques. Les ACL peuvent être utilisées pour autoriser ou refuser l'accès à des réseaux ou des services spécifiques, en fonction de critères tels que les adresses IP source et destination, les numéros de port et les protocoles.

Il existe deux principaux types d'ACL : les ACL standard et les ACL étendues.

- **ACL standard** : Ces ACL filtrent le trafic uniquement en fonction de l'adresse IP source. Elles sont simples à configurer mais offrent une granularité limitée. Par exemple, une ACL standard peut être utilisée pour autoriser ou bloquer tout le trafic provenant d'un ensemble spécifique d'adresses IP.
- **ACL étendues** : Ces ACL offrent un niveau de contrôle plus fin en permettant de filtrer le trafic en fonction de plusieurs critères, tels que l'adresse IP source, l'adresse IP destination, les numéros de port source et destination, et le protocole utilisé (TCP, UDP, ICMP, etc.). Les ACL étendues sont plus complexes à configurer, mais elles offrent une flexibilité et une précision accrues pour le contrôle du trafic.

### Configuration de base des ACL sur un routeur

Configurer une ACL sur un routeur implique plusieurs étapes clés. Voici un guide général pour la configuration de base des ACL standard et étendues sur un routeur Cisco, par exemple :

1. **Accéder au mode de configuration globale** : 
   ```plaintext
   router> enable
   router# configure terminal
   ```

2. **Configurer une ACL standard** :
   - Créer une ACL standard et ajouter une règle pour autoriser le trafic :
     ```plaintext
     router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
     ```

3. **Configurer une ACL étendue** :
   - Créer une ACL étendue et ajouter une règle pour autoriser le trafic HTTP depuis un sous-réseau spécifique :
     ```plaintext
     router(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
     ```

4. **Appliquer l'ACL à une interface** :
   - Associer l'ACL à une interface pour contrôler le trafic entrant ou sortant :
     ```plaintext
     router(config)# interface GigabitEthernet0/0
     router(config-if)# ip access-group 1 in
     ```

Ces étapes illustrent une configuration de base, mais les ACL peuvent être beaucoup plus complexes, en fonction des besoins spécifiques du réseau.

### Cas d'utilisation des ACL dans le filtrage de trafic

Les ACL sont utilisées dans divers scénarios pour contrôler le flux de trafic réseau et renforcer la sécurité. Voici quelques exemples de cas d'utilisation courants :

- **Contrôle de l'accès à un réseau local (LAN)** : Les ACL peuvent être utilisées pour restreindre l'accès à certaines parties d'un réseau interne, en bloquant ou en autorisant le trafic en fonction des adresses IP source.
- **Sécurisation des connexions Internet** : Les ACL peuvent filtrer le trafic entrant et sortant entre un réseau interne et Internet, empêchant les connexions non autorisées à des services sensibles.
- **Gestion de la bande passante** : Les ACL peuvent prioriser ou limiter le trafic pour certains types d'applications ou utilisateurs, aidant ainsi à gérer efficacement la bande passante réseau.
- **Protection contre les attaques** : Les ACL peuvent être configurées pour bloquer le trafic provenant d'adresses IP connues pour être malveillantes ou pour limiter les types de trafic qui pourraient être utilisés dans des attaques par déni de service (DoS).

### Limites des ACL en termes de sécurité

Bien que les ACL soient des outils puissants pour le contrôle du trafic réseau, elles présentent certaines limites en termes de sécurité :

- **Granularité limitée (pour les ACL standard)** : Les ACL standard ne permettent de filtrer le trafic qu'en fonction de l'adresse IP source, ce qui peut être insuffisant pour des politiques de sécurité détaillées.
- **Complexité de configuration (pour les ACL étendues)** : Les ACL étendues offrent une grande flexibilité, mais leur configuration peut être complexe et sujette à des erreurs, surtout dans des environnements réseau de grande envergure.
- **Absence d'inspection approfondie** : Les ACL ne sont pas conçues pour inspecter le contenu des paquets de données. Elles ne peuvent donc pas détecter les attaques basées sur le contenu ou les logiciels malveillants dissimulés.
- **Maintenance et gestion continues** : Les ACL nécessitent une maintenance régulière pour rester efficaces. Les politiques de filtrage doivent être régulièrement mises à jour pour refléter les changements dans le réseau et les nouvelles menaces.

En dépit de ces limites, les ACL restent un élément essentiel de la stratégie de sécurité des réseaux, offrant un contrôle de base mais crucial sur le trafic réseau.
