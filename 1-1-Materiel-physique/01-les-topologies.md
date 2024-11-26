# Matériel physique

## 01. Les topologies réseau

Avant de plonger dans les aspects techniques des réseaux informatiques, il est essentiel de comprendre comment nous en sommes arrivés là. Cette histoire commence dans les années 1960, une période marquée par la guerre froide et une course technologique sans précédent.

### 01.1 Introduction aux topologies de base

La topologie réseau est l'arrangement physique ou logique des différents éléments d'un réseau informatique. C'est en quelque sorte la "carte routière" qui définit comment les différents équipements communiquent entre eux. Comprendre les topologies est fondamental car elles influencent directement les performances, la fiabilité et le coût d'un réseau.

Définition d'une topologie réseau :

Une topologie réseau définit :

- L'organisation des connexions entre les équipements, ce qui comprend non seulement l'agencement des liens de communication comme les câbles, fibres et ondes, mais aussi tous les points de connexion et les équipements d'interconnexion tels que les switches, routeurs et hubs qui forment l'infrastructure du réseau.
- Les chemins que peuvent emprunter les données en établissant clairement les routes principales et alternatives, les possibilités de redondance, ainsi que les points de passage obligatoires qui structurent la circulation de l'information.
- La manière dont les équipements communiquent en définissant précisément les méthodes d'accès au support, la gestion des priorités de transmission et les mécanismes de gestion des collisions qui garantissent une communication efficace.
- La structure physique qui englobe la disposition réelle des équipements, toutes les contraintes liées à l'installation et le respect des distances maximales imposées par les technologies utilisées.
- La structure logique qui détermine les flux de données, la sélection des protocoles et la manière dont le réseau est segmenté pour optimiser les communications.

L'importance du choix d'une topologie :
Le choix d'une topologie impacte :

- La facilité d'installation et de maintenance qui englobe tous les aspects pratiques du réseau, depuis l'accessibilité des équipements pour la maintenance quotidienne jusqu'à la simplicité du câblage et de son organisation, en passant par la capacité à diagnostiquer rapidement les pannes et à intervenir sur le réseau sans nécessiter une interruption complète du service.
- La capacité d'évolution du réseau qui détermine la flexibilité de l'infrastructure, permettant d'ajouter de nouveaux équipements, de modifier la structure sans nécessiter une refonte complète, de s'adapter aux nouvelles technologies émergentes et de gérer efficacement l'augmentation progressive du trafic réseau.
- La résistance aux pannes qui constitue un élément crucial, englobant l'identification des points uniques de défaillance potentiels, la capacité du réseau à continuer de fonctionner en mode dégradé, l'estimation du temps nécessaire pour rétablir le service après un incident et l'évaluation de l'impact d'une panne sur l'ensemble de l'infrastructure.
- Les performances globales qui regroupent tous les aspects quantitatifs du réseau, notamment les débits maximaux possibles, la latence inhérente à l'architecture choisie, les mécanismes de gestion de la congestion et les possibilités de mise en place d'une qualité de service adaptée aux besoins.

Les critères de sélection doivent prendre en compte :

- Le budget disponible qui ne se limite pas au simple coût des équipements actifs, mais englobe l'ensemble des dépenses liées au câblage, à l'installation, à la maintenance régulière et aux évolutions futures prévisibles de l'infrastructure.
- Le niveau de fiabilité requis qui doit être évalué en fonction du taux de disponibilité attendu, de la tolérance aux pannes nécessaire pour les applications critiques, et des besoins spécifiques en matière de redondance pour garantir la continuité de service.
- La facilité de maintenance qui doit être analysée en tenant compte des compétences techniques disponibles en interne, des outils de supervision nécessaires pour assurer un suivi efficace, de la fréquence prévue des interventions et des besoins en documentation et formation pour l'équipe technique.

Types de topologies :

- La topologie physique est celle qui se matérialise dans le monde réel. Elle représente l'agencement concret des équipements en prenant en compte la disposition des postes de travail, l'emplacement stratégique des équipements d'interconnexion et l'organisation des locaux techniques, tout en respectant les contraintes architecturales du bâtiment. Elle définit également le parcours physique des signaux à travers les chemins de câbles et les points de concentration, en tenant compte des distances maximales autorisées et des types de média utilisés. Cette topologie détermine enfin les contraintes matérielles incluant le respect des normes de câblage, la prise en compte des contraintes environnementales, la mise en place de la sécurité physique et la gestion des alimentations électriques.
- La topologie logique, quant à elle, décrit la manière dont les données circulent réellement dans le réseau, indépendamment de la disposition physique des équipements. Elle englobe la gestion des flux applicatifs, la définition des routes principales et alternatives, ainsi que la segmentation du trafic et les mécanismes de contrôle. Elle influence directement le choix des protocoles en définissant les méthodes d'accès au média, la gestion des collisions, les priorités de trafic et les mécanismes de routage. Cette topologie définit également l'organisation logique du réseau à travers la mise en place des VLANs, la délimitation des domaines de diffusion, l'établissement des zones de sécurité et l'implémentation des politiques de routage.

### 01.2 Les topologies fondamentales et leurs caractéristiques

Avant de détailler chaque topologie, il est important de comprendre que chacune répond à des besoins spécifiques et présente ses propres particularités. Nous allons examiner les quatre topologies fondamentales qui ont façonné l'histoire des réseaux et continuent d'influencer les architectures modernes.

#### La topologie en bus

Cette topologie, une des plus anciennes, repose sur un principe simple mais efficace :

- Le principe de fonctionnement s'articule autour d'un câble principal (le bus) auquel tous les équipements sont connectés en série. Les données émises par un équipement se propagent sur toute la longueur du bus et sont "vues" par tous les autres équipements, mais seul le destinataire désigné par l'adresse les traite effectivement.

- La méthode d'accès au média utilise généralement le protocole CSMA/CD (Carrier Sense Multiple Access with Collision Detection), où chaque équipement doit écouter le bus avant d'émettre et être capable de détecter les collisions quand deux équipements émettent simultanément.

- Les avantages incluent la simplicité de mise en œuvre, le faible coût d'installation initial et la facilité à comprendre le fonctionnement du réseau, particulièrement utile pour la formation et le dépannage basique.

- Les inconvénients sont nombreux et expliquent son abandon progressif : une rupture du câble principal paralyse l'ensemble du réseau, les performances se dégradent rapidement avec l'augmentation du nombre d'équipements, et l'extensibilité est limitée par la longueur maximale du bus et le nombre de connexions possibles.

#### La topologie en étoile

Cette topologie, devenue aujourd'hui un standard, se caractérise par sa structure centralisée :

- La structure repose sur un équipement central (switch ou hub) auquel chaque équipement du réseau est directement connecté via son propre câble. Cette organisation permet une gestion individualisée de chaque connexion et simplifie considérablement la détection des problèmes.

- L'équipement central joue un rôle crucial car il gère l'ensemble du trafic réseau. Dans le cas d'un switch moderne, il analyse les adresses de destination et ne transmet les données qu'aux ports concernés, optimisant ainsi l'utilisation de la bande passante et réduisant les risques de collision.

- Les forces de cette topologie sont nombreuses : la facilité d'ajout ou de retrait d'équipements sans perturber le reste du réseau, l'isolation des problèmes à une seule branche de l'étoile, et la possibilité d'utiliser des technologies différentes sur chaque branche.

- Les faiblesses se concentrent principalement sur la dépendance à l'équipement central, dont la panne peut paralyser l'ensemble du réseau, et sur le coût plus élevé en câblage puisque chaque équipement nécessite son propre câble jusqu'au point central.

#### La topologie en anneau

Cette topologie, particulièrement utilisée dans les technologies comme Token Ring, présente une approche unique :

- La circulation des données suit un chemin circulaire unidirectionnel, chaque équipement recevant les données de son prédécesseur et les transmettant à son successeur. Ce système forme une boucle fermée où l'information circule de manière séquentielle.

- Les mécanismes de redondance peuvent être mis en place grâce à un double anneau fonctionnant en sens inverse, permettant de maintenir la connectivité même en cas de rupture d'un des anneaux.

- Les points forts incluent une gestion déterministe de l'accès au média (particulièrement avec le système du jeton), une égalité d'accès pour tous les équipements, et des performances prévisibles même en charge.

- Les limitations concernent principalement la complexité d'administration, le risque de paralysie en cas de défaillance d'un équipement si la redondance n'est pas implémentée, et la latence qui augmente avec le nombre d'équipements puisque chaque nœud doit relayer les données.

#### La topologie maillée

Cette topologie, la plus complexe mais aussi la plus robuste, se caractérise par sa redondance maximale :

- Les connexions multiples permettent à chaque équipement d'être relié directement à plusieurs autres nœuds du réseau, créant ainsi de nombreux chemins possibles pour les données.

- La redondance et la résilience sont maximales car la perte d'une connexion n'isole pas les équipements, le trafic pouvant emprunter des chemins alternatifs.

- Les coûts sont généralement élevés en raison du nombre important de connexions nécessaires et de la complexité des équipements devant gérer plusieurs liens simultanés.

- La complexité de gestion nécessite des protocoles de routage sophistiqués pour déterminer le meilleur chemin à utiliser et gérer la redondance de manière efficace.

### 01.3 Les topologies hybrides modernes

Les réseaux modernes utilisent rarement une topologie pure, préférant combiner les avantages de différentes approches pour répondre aux besoins complexes des organisations actuelles.

#### L'étoile étendue (Extended Star)

Cette évolution de la topologie en étoile est aujourd'hui la plus répandue dans les entreprises :

- Le principe repose sur une hiérarchie d'équipements centraux, chaque switch de niveau inférieur étant connecté à un ou plusieurs switches de niveau supérieur
- Cette organisation permet une segmentation logique du réseau, facilitant la gestion du trafic et la sécurisation des données
- Les avantages incluent une excellente scalabilité, une maintenance simplifiée et la possibilité d'implémenter facilement des technologies comme les VLANs
- La redondance peut être mise en place à chaque niveau, offrant une excellente tolérance aux pannes

#### L'arbre hiérarchique (Hierarchical Tree)

Dérivée de l'étoile étendue, cette topologie structure le réseau en niveaux distincts :

- Le niveau cœur (Core) assure le routage rapide entre les différentes parties du réseau
- Le niveau distribution (Distribution) gère les politiques de routage et de sécurité
- Le niveau accès (Access) connecte les équipements finaux
- Cette séparation claire des fonctions permet une meilleure gestion des performances et de la sécurité

#### L'anneau en étoile (Star-Ring)

Cette topologie hybride combine les avantages de l'anneau et de l'étoile :

- Les équipements sont connectés en étoile à des concentrateurs.
- Les concentrateurs sont eux-mêmes reliés en anneau.
- Cette configuration offre une bonne redondance tout en simplifiant la gestion des connexions utilisateurs.
- Elle est particulièrement adaptée aux réseaux métropolitains (MAN) et aux réseaux de campus.

#### Le maillage partiel (Partial Mesh)

Version optimisée de la topologie maillée :

- Seuls les nœuds stratégiques sont interconnectés de manière redondante.
- Les autres équipements utilisent des connexions simples vers les nœuds principaux.
- Cette approche offre un bon compromis entre coût et résilience.
- Elle est couramment utilisée dans les réseaux WAN et les backbones d'entreprise.

#### Les solutions mixtes

Les réseaux d'entreprise modernes combinent souvent plusieurs approches :

- Le cœur du réseau peut être maillé pour la résilience.
- La distribution utilise souvent une topologie en étoile étendue.
- L'accès peut être organisé en arbre ou en étoile simple.
- Les connexions redondantes sont établies aux points critiques uniquement.

##### Considérations pour le choix d'une topologie hybride :

- Les besoins en performance :
  - Débits nécessaires à chaque niveau
  - Latence acceptable
  - Points de congestion potentiels

- La tolérance aux pannes :
  - Identification des équipements critiques
  - Niveau de redondance nécessaire
  - Temps de basculement acceptable

- La facilité de gestion :
  - Complexité de la configuration
  - Outils de supervision nécessaires
  - Compétences requises

- L'évolutivité :
  - Capacité d'extension future
  - Compatibilité avec les nouvelles technologies
  - Facilité de mise à niveau

### 01.4 Cas d'usage et choix selon les besoins

Le choix d'une topologie réseau est une décision stratégique qui impacte durablement le fonctionnement de l'organisation. Il n'existe pas de solution universelle, mais une analyse méthodique des besoins permet d'identifier la meilleure approche.

#### Critères de choix fondamentaux

La sélection d'une topologie réseau doit résulter d'une analyse approfondie des besoins organisationnels et techniques :

- Les contraintes budgétaires constituent souvent le premier facteur limitant. Au-delà du coût d'acquisition des équipements, il faut considérer l'ensemble du cycle de vie : installation, formation du personnel, maintenance préventive et corrective, mises à niveau futures, et coûts énergétiques. Une topologie apparemment économique à l'achat peut s'avérer coûteuse sur le long terme si elle nécessite une maintenance intensive

- Le niveau de disponibilité requis doit être évalué service par service. Certaines applications critiques peuvent nécessiter une disponibilité de 99,999% (soit moins de 5 minutes d'interruption par an), impliquant une redondance complète, tandis que d'autres services peuvent tolérer des interruptions occasionnelles. Cette analyse influencera directement le degré de redondance nécessaire dans la topologie

- Les compétences techniques disponibles en interne sont cruciales pour la maintenance quotidienne. Une topologie complexe avec de nombreuses redondances et des protocoles sophistiqués nécessite une équipe qualifiée et disponible. À l'inverse, une petite structure pourrait privilégier une topologie plus simple, quitte à sacrifier certaines fonctionnalités avancées

- Les perspectives d'évolution doivent être anticipées sur 5 à 10 ans : croissance prévisible de l'organisation, nouveaux usages, technologies émergentes. La topologie choisie doit pouvoir s'adapter sans nécessiter une refonte complète

#### Exemples d'implémentation

##### Réseaux d'entreprise

La topologie en étoile étendue s'est imposée comme un standard pour les entreprises de taille moyenne à grande. Son organisation hiérarchique répond parfaitement aux besoins de segmentation et de sécurité :

- Le cœur de réseau redondant assure une disponibilité maximale des services critiques. Généralement constitué de deux switches de niveau 3 en redondance active/active ou active/passive, il garantit la continuité de service même en cas de panne d'un équipement

- La distribution par étage ou par service permet une gestion granulaire du trafic. Chaque switch de distribution peut appliquer des politiques de QoS spécifiques, des règles de routage distinctes et des mécanismes de sécurité adaptés aux besoins du service qu'il dessert

- Les connexions utilisateurs en étoile simple offrent un excellent rapport coût/performance. La panne d'un port ou d'un câble n'affecte qu'un seul utilisateur, simplifiant le dépannage

##### Réseaux domestiques

Les réseaux domestiques privilégient la simplicité et la facilité d'utilisation :

- La box Internet joue le rôle de point central, concentrant les fonctions de routeur, switch et point d'accès Wi-Fi. Cette approche tout-en-un simplifie la gestion mais crée un point unique de défaillance

- Les connexions filaires directes sont réservées aux équipements nécessitant une bande passante importante ou une latence minimale : Smart TV, console de jeux, NAS

- Le réseau Wi-Fi assure la mobilité mais doit être correctement dimensionné pour couvrir l'ensemble de l'habitat. L'utilisation de répéteurs ou de systèmes mesh peut être nécessaire dans les grandes surfaces

##### Réseaux industriels

Les environnements industriels imposent des contraintes particulières qui influencent fortement le choix de la topologie :

- La topologie en anneau ou maillée pour les équipements critiques est privilégiée car la continuité de production est vitale. Un arrêt non planifié peut coûter des milliers d'euros par minute. Cette approche garantit qu'une panne unique ne pourra pas interrompre la communication :
  - La redondance des liens critiques permet un basculement automatique en quelques millisecondes
  - La séparation physique des réseaux de contrôle isole le trafic industriel sensible du trafic bureautique
  - Les chemins multiples assurent que les données critiques peuvent toujours atteindre leur destination

- Les équipements non-critiques (terminaux de supervision, stations de programmation) utilisent une topologie en étoile plus économique. La perte momentanée de ces équipements n'impacte pas directement la production

- La segmentation du trafic est cruciale dans l'industrie. Les communications temps réel des automates ne doivent jamais être perturbées par d'autres types de trafic. Cette séparation peut être physique ou logique (VLAN) selon le niveau de criticité

##### Réseaux de campus

Les campus universitaires ou d'entreprise présentent des défis spécifiques liés à leur étendue géographique et à la diversité des usages :

- L'architecture hiérarchique à trois niveaux s'est imposée comme la solution la plus adaptée :
  - Le cœur maillé interconnecte les différents bâtiments via des liens haut débit, généralement en fibre optique. La redondance complète à ce niveau est standard
  - La distribution assure la connexion entre le cœur et les équipements d'accès de chaque bâtiment. Elle gère également les services réseau comme le routage inter-VLAN
  - L'accès connecte les utilisateurs finaux avec des technologies adaptées à leurs besoins : Wi-Fi pour la mobilité, Ethernet pour les postes fixes

#### Évolution des topologies

L'évolution des technologies et des usages transforme progressivement les topologies traditionnelles :

- Le SDN (Software Defined Networking) révolutionne l'approche du réseau :
  - La séparation du plan de contrôle et du plan de données permet une gestion centralisée plus intelligente
  - Les modifications de configuration peuvent être déployées instantanément sur l'ensemble du réseau
  - L'automatisation réduit les erreurs humaines et accélère les déploiements

- Le cloud computing impose de nouvelles contraintes :
  - Les topologies doivent s'adapter à l'hybridation croissante des infrastructures
  - La connectivité multi-sites devient la norme plutôt que l'exception
  - La bande passante doit pouvoir évoluer dynamiquement selon les besoins

#### Considérations de maintenance

La maintenance n'est plus une activité périphérique mais doit être intégrée dès la conception :

- La documentation devient un élément critique :
  - Les schémas des topologies physiques et logiques doivent être maintenus à jour en temps réel
  - L'inventaire des équipements doit inclure les informations de garantie et de support
  - Les procédures d'intervention doivent être détaillées et testées régulièrement

- La supervision proactive remplace la réaction aux incidents :
  - Les outils de monitoring permettent de détecter les problèmes avant qu'ils n'impactent les utilisateurs
  - Les alertes automatisées doivent être correctement calibrées pour éviter la fatigue d'alerte
  - Les tableaux de bord donnent une vision synthétique de l'état du réseau

- Le plan de reprise d'activité n'est plus une option :
  - Les procédures de basculement doivent être documentées et testées régulièrement
  - Le personnel doit être formé aux situations d'urgence
  - Les sauvegardes des configurations doivent être accessibles rapidement en cas de besoin