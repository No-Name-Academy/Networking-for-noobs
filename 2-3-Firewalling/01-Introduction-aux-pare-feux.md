# Les pare-feux: du filtrage des routes aux NGFWs

## 1. Introduction aux Pare-Feux

Imaginez une ville fortifiée, protégée par ses remparts contre les intrusions extérieures. Un pare-feu, dans le monde numérique, joue un rôle similaire à ces remparts, servant de barrière entre votre réseau informatique et l'extérieur,  protégeant vos données sensibles des menaces potentielles. En termes techniques, un **pare-feu** est un système qui contrôle le flux de trafic réseau, autorisant ou bloquant les communications en fonction d'un ensemble de règles prédéfinies. 

### 1.1 **Définition : Un Gardien du Réseau**

Le pare-feu agit comme un gardien vigilant, scrutant chaque paquet de données qui tente d'entrer ou de sortir de votre réseau. Il analyse l'adresse IP, le port et le protocole utilisés par le paquet pour déterminer s'il est autorisé à passer ou non. Imaginez-le comme un agent de sécurité qui vérifie les badges d'entrée et examine les colis avant de les laisser entrer dans la zone sécurisée.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/2-3-Firewalling/Sources/01-001.svg">

### 1.2 **Rôle Essentiel : Sécurité et Contrôle des Données**

Le rôle du pare-feu dans la sécurité informatique est crucial. Il agit comme une première ligne de défense contre diverses menaces, notamment:

* **Attaques malveillantes:** Les pirates informatiques tentent souvent d'infiltrer les réseaux pour voler des données sensibles ou saboter les systèmes. Le pare-feu bloque ces tentatives en filtrant les communications suspectes et les attaques connues.
* **Accès non autorisé:** Un pare-feu empêche les utilisateurs non autorisés d'accéder à vos ressources réseau, protégeant ainsi vos informations confidentielles.
* **Surveillance du trafic réseau:** Le pare-feu permet de surveiller le flux de données entrant et sortant de votre réseau, fournissant des informations précieuses sur l'activité utilisateur et la sécurité globale.

### 1.3 **Types de Pare-Feux : Adaptabilité aux Besoins**

Il existe différents types de pare-feux, chacun adapté à des besoins spécifiques:

* **Pare-feu basés sur le routage (hardware/software):** Ces pare-feux fonctionnent au niveau du réseau, analysant les adresses IP et les protocoles pour filtrer le trafic. Ils sont souvent utilisés dans les environnements d'entreprise ou les réseaux domestiques plus grands.
* **Pare-feu d'application:** Ces pare-feux analysent le contenu des paquets de données, comme les fichiers et les messages, pour identifier les menaces spécifiques. Ils sont particulièrement efficaces pour bloquer les attaques ciblées sur des applications web ou des services spécifiques.


### 1.4 **Le Filtrage : Un Système Règles Rigides**

Le principe du filtrage est au cœur du fonctionnement d'un pare-feu. Chaque pare-feu utilise un ensemble de règles prédefinies, appelées politiques de sécurité, pour déterminer quel trafic est autorisé et lequel est bloqué. Ces règles peuvent être basées sur divers critères, tels que l'adresse IP, le port, le protocole, l'heure ou même le contenu du message. Imaginez ces règles comme des "barrières" virtuelles qui définissent les accès autorisés à chaque zone de votre réseau.



