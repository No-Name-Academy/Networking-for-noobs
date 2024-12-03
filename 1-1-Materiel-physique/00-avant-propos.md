# Matériel physique

## 00. Avant propos sur le réseau

Avant de plonger dans les aspects techniques des réseaux informatiques, il est essentiel de comprendre comment nous en sommes arrivés là. Cette histoire commence dans les années 1960, une période marquée par la guerre froide et une course technologique sans précédent.

### 00.1 Les débuts d'ARPANET (1960-1970)

Dans le contexte tendu de la guerre froide, les États-Unis cherchaient à maintenir leur supériorité technologique. En 1957, l'URSS avait lancé Spoutnik, créant une onde de choc dans le monde occidental. En réponse, le département de la Défense américain créa l'ARPA (Advanced Research Projects Agency) en 1958, qui allait devenir le berceau d'ARPANET.
Le projet DARPA (rebaptisé ainsi plus tard) avait un objectif crucial : créer un réseau de communication qui pourrait survivre à une attaque nucléaire. La solution ? Un réseau décentralisé, où l'information pourrait emprunter différents chemins pour atteindre sa destination. Cette approche révolutionnaire s'opposait aux architectures centralisées traditionnelles de l'époque.
Les premiers pas concrets d'ARPANET furent réalisés en 1969, lorsque quatre nœuds furent interconnectés :

L'université de Californie à Los Angeles (UCLA)
Le Stanford Research Institute (SRI)
L'université de Californie à Santa Barbara (UCSB)
L'université de l'Utah

Ces connexions utilisaient des lignes téléphoniques louées et des ordinateurs spécialisés appelés IMPs (Interface Message Processors), les ancêtres de nos routeurs actuels. Le débit était de 50 kbit/s, une vitesse qui nous paraît dérisoire aujourd'hui mais qui était révolutionnaire à l'époque.
Le protocole initial utilisé était le NCP (Network Control Protocol), qui permettait la communication entre les différents nœuds. Bien que rudimentaire comparé aux standards actuels, ce protocole a posé les bases de ce qui allait devenir le TCP/IP quelques années plus tard.
Les premiers services développés sur ARPANET étaient :

Le transfert de fichiers
L'accès à distance aux ordinateurs
La messagerie électronique (qui fut inventée en 1971)

À la fin de l'année 1970, ARPANET comptait déjà 13 nœuds, démontrant le succès et le potentiel de cette nouvelle approche réseau.

### 00.2 L'évolution vers Internet (1970-1990)

La transformation d'ARPANET en ce que nous connaissons aujourd'hui comme Internet s'est faite progressivement, marquée par plusieurs innovations majeures.
La naissance de TCP/IP :
Au début des années 1970, les chercheurs Vinton Cerf et Bob Kahn ont identifié les limites du protocole NCP, notamment son incapacité à gérer les communications entre différents types de réseaux. En 1974, ils proposent alors le protocole TCP (Transmission Control Protocol), rapidement suivi de l'IP (Internet Protocol). Cette suite protocolaire TCP/IP apportait deux avancées majeures :

La possibilité d'interconnecter des réseaux hétérogènes
Une meilleure fiabilité dans la transmission des données grâce au découpage en paquets

- La transition d'ARPANET vers Internet :
Le 1er janvier 1983, marqua un tournant historique : ce fut le "flag day", jour où tous les ordinateurs d'ARPANET durent basculer de NCP vers TCP/IP. Cette migration obligatoire permit :

- L'interconnexion avec d'autres réseaux émergents
La création des premiers backbones (épines dorsales du réseau)
L'apparition des premiers fournisseurs d'accès commerciaux

- L'apparition du DNS :
Face à la croissance du réseau, la mémorisation des adresses IP devint rapidement problématique. En 1984, Paul Mockapetris proposa le DNS (Domain Name System) qui :

* Permettait d'associer des noms plus facilement mémorisables aux adresses IP
* Introduisait une hiérarchie dans les noms de domaine (.com, .edu, .org, etc.)
* Décentralisait la gestion des noms de domaine

- La démocratisation des réseaux locaux :
Les années 1980 virent l'émergence des réseaux locaux (LAN) avec :

* L'apparition d'Ethernet (standardisé en 1983)
* Le développement des premiers hubs et switches
* L'adoption massive du câblage en paire torsadée
* La standardisation du protocole 802.3 par l'IEEE

Cette période fut également marquée par l'apparition des premiers ordinateurs personnels et la création des premières entreprises de networking comme Cisco (1984). Les réseaux sortaient progressivement du domaine militaire et universitaire pour entrer dans les entreprises et bientôt, les foyers.
À la fin des années 1980, Internet comptait déjà plus de 100 000 ordinateurs connectés, préfigurant l'explosion qui allait suivre dans les années 1990 avec l'apparition du World Wide Web.

- L'aube du World Wide Web :
La fin des années 1980 et le début des années 1990 marquent un tournant décisif avec l'invention du World Wide Web par Tim Berners-Lee au CERN. En 1989, il propose un système révolutionnaire basé sur :

* Le protocole HTTP (HyperText Transfer Protocol) pour le transfert des données
* Le langage HTML pour la création de pages web
* Le concept d'URL pour localiser les ressources

Le premier site web est mis en ligne en 1991, et le CERN rend la technologie publique en 1993. Cette décision historique, couplée à l'apparition des premiers navigateurs graphiques comme Mosaic, puis Netscape Navigator, marque le début de la démocratisation d'Internet tel que nous le connaissons aujourd'hui.

### 00.3 Les grandes évolutions des réseaux physiques

L'évolution des supports physiques a été déterminante dans le développement des réseaux, chaque innovation apportant son lot d'améliorations en termes de performances et de fiabilité.

Du câble coaxial à la paire torsadée :

Les premiers réseaux utilisaient le câble coaxial, hérité des technologies de télécommunication :

- Le câble coaxial épais (10BASE5) ou "thick ethernet"
- Le câble coaxial fin (10BASE2) ou "thin ethernet"

Ces câbles présentaient plusieurs inconvénients :

- Installation complexe et peu flexible
- Vulnérabilité aux ruptures physiques
- Coût élevé

---

L'arrivée de la paire torsadée dans les années 1990 a révolutionné le câblage réseau :

- Catégorie 3 (10 Mbps)
- Catégorie 5 (100 Mbps)
- Catégorie 5e, 6, 6a (1 Gbps et plus)

Avantages majeurs :

- Installation plus simple et moins coûteuse
- Meilleure résistance aux interférences électromagnétiques
- Facilité de maintenance et de dépannage

---

L'avènement de la fibre optique :

La fibre optique représente une révolution technologique majeure :

- Transmission par impulsions lumineuses
- Débits considérablement plus élevés (plusieurs Tbps possibles)
- Immunité aux interférences électromagnétiques
- Portée beaucoup plus importante (plusieurs dizaines de kilomètres)

Elle existe en deux variantes principales :

- Monomode (longue distance, télécommunications)
- Multimode (courte distance, réseaux locaux)

---

L'émergence des technologies sans fil :

Parallèlement, les technologies sans fil ont connu un développement fulgurant :

- Wi-Fi : De 802.11b (11 Mbps) au Wi-Fi 6 (plusieurs Gbps)
- Bluetooth : Communication courte portée
- Réseaux cellulaires : Du 1G au 5G
- Technologies émergentes : Li-Fi, WiGig, etc.

---

L'augmentation des débits au fil du temps montre une progression exponentielle :

- 1980 : 10 Mbps (Ethernet)
- 1995 : 100 Mbps (Fast Ethernet)
- 1999 : 1 Gbps (Gigabit Ethernet)
- 2002 : 10 Gbps
- 2010 : 100 Gbps
- Aujourd'hui : Plusieurs Tbps sur fibre optique

Cette évolution continue des supports physiques a permis :

- L'explosion des services en ligne
- Le développement du cloud computing
- L'émergence de nouvelles applications (streaming HD, réalité virtuelle, etc.)
- La démocratisation de l'accès à Internet haut débit

### 00.4 L'importance des réseaux aujourd'hui

L'interconnexion mondiale :
En quelques décennies, les réseaux sont devenus l'épine dorsale de notre société moderne :

- Plus de 5 milliards d'utilisateurs Internet dans le monde
- Des millions de kilomètres de câbles sous-marins reliant les continents
- Des infrastructures critiques reposant sur les réseaux (banques, hôpitaux, énergie)
- Une connectivité omniprésente (mobiles, objets connectés, véhicules)

L'impact sur l'économie et la société :
La transformation numérique a bouleversé tous les secteurs :

- Le e-commerce représente une part croissante des échanges commerciaux
- Le télétravail est devenu une réalité pour des millions de personnes
- L'éducation en ligne permet l'accès au savoir à l'échelle mondiale
- Les réseaux sociaux ont transformé nos modes de communication
- L'économie numérique génère des billions de dollars chaque année

Les nouveaux enjeux :
L'évolution des réseaux fait face à de nouveaux défis :

- L'Internet des Objets (IoT) : Des milliards d'objets connectés à gérer
- Le Edge Computing : Rapprocher le traitement des données des utilisateurs
- La 5G et bientôt la 6G : Permettre de nouvelles applications (véhicules autonomes, chirurgie à distance)
- Le Green IT : Réduire l'impact environnemental des infrastructures réseau
- La cybersécurité : Protéger les données et les infrastructures

Les défis futurs :
Les réseaux devront répondre à des exigences toujours plus importantes :

- La souveraineté numérique des nations
- La fracture numérique à combler
- La gestion de la confidentialité et de la vie privée
- L'explosion du volume de données (Big Data)
- L'adaptation aux nouvelles technologies (Quantum Computing, IA)

Cette révolution numérique continue de s'accélérer, rendant la compréhension des réseaux fondamentale pour :

- Les professionnels de l'IT
- Les décideurs et entrepreneurs
- Les citoyens qui doivent comprendre cet environnement numérique
- Les futures générations qui devront faire face à ces défis

Nous avons ainsi terminé notre voyage historique, des premiers pas d'ARPANET à l'omniprésence actuelle des réseaux. Cette base historique nous permet de mieux comprendre les enjeux techniques que nous allons aborder dans les prochaines parties du cours.