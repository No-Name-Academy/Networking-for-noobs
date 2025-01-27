# Les pare-feux: du filtrage des routes aux NGFWs

## 2. Fonctionnement des Pare-Feux Basés sur le Routage

Un pare-feu basé sur le routage agit comme un régulateur du trafic réseau, contrôlant la circulation des données entre différents segments du réseau. Imaginez une autoroute urbaine avec plusieurs sorties et entrées. Le pare-feu, dans ce cas, agirait comme les panneaux de signalisation et les barrières, dirigeant le flux de véhicules (paquets de données) vers leur destination finale tout en bloquant les accès interdits.

### 2.1 **Architecture : Un Pont Intégré**

Ces pare-feux sont souvent intégrés directement aux routeurs, les dispositifs responsables d'acheminer les paquets de données entre différents réseaux.  Imaginez un pont qui relie deux îles, permettant le passage des navires (données) tout en contrôlant l'accès aux différentes zones. Le pare-feu, dans cette architecture, est intégré au pont lui-même, surveillant et filtrant le trafic qui traverse son "passage".

### 2.2 **Fonctionnement : Décryptage des Paquets de Données**

Le cœur du fonctionnement d'un pare-feu basé sur le routage réside dans l'analyse des paquets TCP/IP. Chaque paquet contient des informations essentielles sur l'origine, la destination et le contenu de la communication. Le pare-feu examine attentivement ces données pour déterminer si la communication est autorisée. 

Il décrypte les adresses IP, qui identifient les ordinateurs impliqués dans la communication, et les ports utilisés par chaque application. Il analyse également les protocoles, les règles régissant le format et l'échange des données. Imaginez un agent de sécurité examinant un passeport (l'adresse IP), une lettre (le contenu) et un billet d'entrée (le port) avant d'autoriser l'accès à un lieu spécifique.

### 2.3 **ACL : Les Barrières Virtuelles du Réseau**

Les **Access Control Lists (ACL)** sont des ensembles de règles définies par l'administrateur système qui déterminent le trafic autorisé sur le réseau. Imaginez-les comme des barrières virtuelles, contrôlant le passage des données entre différents segments du réseau. 

Chaque règle ACL spécifie les conditions d'accès, comme l'adresse IP source ou destination, le port utilisé, le protocole, etc. Par exemple, une règle pourrait autoriser uniquement les connexions entrantes sur le port 80 (utilisé par les navigateurs web) provenant d'une adresse IP spécifique.

* **Exemples concrets:**
  * Bloquer tout accès à un serveur FTP situé sur l'adresse IP 192.168.1.10 depuis l'extérieur du réseau local.
  * Autoriser uniquement les connexions entrantes sur le port 22 (SSH) provenant d'adresses IP définies comme "confiable".

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-3-Firewalling/Sources/02-001.svg">

### 2.4 **Règles de Filtrage Simples : Un Cadre Basique**

Les pare-feux basés sur le routage utilisent des règles de filtrage simples pour contrôler le trafic réseau. Ces règles peuvent être basées sur:

* **Adresse IP:** Bloquer ou autoriser l'accès à des réseaux spécifiques en fonction de leurs adresses IP.
* **Port:** Définir les ports ouverts et fermés, permettant ou bloquant l'accès aux applications spécifiques.

* **Protocole:** Contrôler le type de protocole utilisé pour la communication (HTTP, FTP, SSH, etc.)

 En résumé, les pare-feux basés sur le routage agissent comme des gardiens vigilants du réseau, contrôlant le flux de données entre différents segments et protégeant votre système contre les menaces potentielles. Ils utilisent des règles précises pour analyser chaque paquet de données et décider s'il est autorisé ou bloqué, assurant ainsi la sécurité et l'intégrité de votre environnement informatique.
