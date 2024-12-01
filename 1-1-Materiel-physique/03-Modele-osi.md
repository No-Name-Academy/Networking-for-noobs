# 3. La couche physique du modèle OSI

## 3.1 Introduction au modèle OSI et la couche physique

La couche physique constitue le fondement même de toute communication réseau. C'est la première couche du modèle OSI, qui en compte sept au total. Pour bien comprendre son rôle, visualisons d'abord sa place dans l'architecture globale.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-1-Materiel-physique/Sources/03-001.svg">

La couche physique est responsable de la transmission brute des bits sur un canal de communication. Elle définit :

- Les caractéristiques physiques des interfaces et des supports de transmission
- La représentation des bits (codage des données)
- La vitesse de transmission des données
- La synchronisation des bits
- La topologie physique du réseau
- Le mode de transmission (simplex, half-duplex, full-duplex)

### Principes fondamentaux

La couche physique transforme les données numériques en signaux qui peuvent être transmis sur le support physique. Cette transformation peut prendre plusieurs formes :

1. **Signaux électriques** sur câbles en cuivre :
   - Variations de tension
   - Modulation d'amplitude
   - Modulation de fréquence

2. **Signaux lumineux** dans la fibre optique :
   - Présence ou absence de lumière
   - Variations d'intensité lumineuse
   - Différentes longueurs d'onde

3. **Ondes électromagnétiques** pour les communications sans fil :
   - Différentes fréquences radio
   - Différentes techniques de modulation
   - Diverses puissances d'émission

## 3.2 Les supports de transmission physiques

L'évolution des supports de transmission physiques reflète l'histoire des réseaux informatiques. Chaque type de support répond à des besoins spécifiques et présente ses propres caractéristiques en termes de performance, de coût et de facilité d'installation.

### Le câble coaxial : l'ancêtre robuste

Le câble coaxial, bien qu'aujourd'hui moins utilisé dans les réseaux informatiques, reste un exemple parfait pour comprendre la structure d'un support de transmission.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-1-Materiel-physique/Sources/03-002.svg">

Le câble coaxial doit son nom à sa structure concentrique qui comprend plusieurs couches :

- Le conducteur central transporte le signal électrique
- Le diélectrique isole le conducteur central
- Le blindage métallique protège des interférences électromagnétiques
- La gaine externe assure la protection mécanique

Cette structure ingénieuse permet de :

1. Minimiser les interférences électromagnétiques
2. Transporter des signaux sur de plus longues distances
3. Maintenir une meilleure qualité de signal

### La paire torsadée : le standard actuel

La paire torsadée a révolutionné le câblage réseau en offrant un excellent compromis entre performance, coût et facilité d'installation. Son principe repose sur l'entrelacement de paires de fils qui permet de réduire naturellement les interférences.

Les catégories de paires torsadées se sont succédé, chacune apportant des améliorations :

1. **Catégorie 5 (Cat5)**
   - Premier standard largement adopté
   - Supporte le Fast Ethernet (100 Mbps)
   - Distance maximale de 100 mètres
   - Largement remplacée aujourd'hui

2. **Catégorie 5e (Cat5e)**
   - Amélioration de la Cat5
   - Supporte le Gigabit Ethernet
   - Meilleure immunité aux interférences
   - Encore très utilisée aujourd'hui

3. **Catégorie 6 (Cat6)**
   - Performances accrues
   - Bande passante de 250 MHz
   - Meilleur blindage
   - Standard actuel pour les nouvelles installations

4. **Catégorie 6a et supérieures**
   - Support du 10 Gigabit Ethernet
   - Bande passante de 500 MHz
   - Blindage renforcé
   - Utilisée dans les centres de données


## 3.3 La fibre optique : la révolution optique

La fibre optique représente une avancée majeure dans les technologies de transmission, utilisant la lumière plutôt que l'électricité pour transporter les données. Cette technologie a révolutionné les télécommunications en permettant des débits jusqu'alors inimaginables sur de très longues distances.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-1-Materiel-physique/Sources/03-004.svg">

### Structure et principes de fonctionnement

La fibre optique est constituée de plusieurs couches concentriques, chacune ayant un rôle spécifique :

1. **Le cœur** (Core)
   - Filament de verre ou de plastique très pur
   - Diamètre de 8 à 62,5 microns selon le type
   - Support de propagation de la lumière
   - Indice de réfraction précisément contrôlé

2. **La gaine optique** (Cladding)
   - Entoure le cœur
   - Indice de réfraction légèrement inférieur
   - Permet la réflexion totale interne
   - Confine la lumière dans le cœur

3. **La gaine externe** (Coating)
   - Protection mécanique
   - Isolation contre l'humidité
   - Identification par code couleur
   - Facilite la manipulation

### Types de fibres optiques

Il existe deux grandes catégories de fibres optiques, chacune adaptée à des usages spécifiques :

#### La fibre monomode (SMF - Single Mode Fiber)

- Cœur très fin (8 à 10 microns)
- Un seul rayon lumineux
- Très longues distances (plusieurs dizaines de kilomètres)
- Utilisée pour :
  * Liaisons intercontinentales
  * Réseaux métropolitains
  * Connexions entre datacenters

#### La fibre multimode (MMF - Multi Mode Fiber)

- Cœur plus large (50 ou 62,5 microns)
- Plusieurs rayons lumineux simultanés
- Distances plus courtes (jusqu'à 2 km)
- Applications :
  * Câblage vertical des bâtiments
  * Réseaux locaux haute performance
  * Connexions courte distance en datacenter

### Avantages et limitations

La fibre optique présente de nombreux avantages :

- Très haut débit (plusieurs Térabits/seconde)
- Immunité aux interférences électromagnétiques
- Atténuation très faible sur de longues distances
- Sécurité accrue (difficile à intercepter)
- Légèreté et faible encombrement

Mais elle comporte aussi quelques contraintes :

- Coût plus élevé que le cuivre
- Installation nécessitant une expertise spécifique
- Fragilité relative nécessitant des précautions
- Équipements actifs plus onéreux

## 3.4 Les technologies sans fil : la mobilité avant tout

Les technologies sans fil ont révolutionné notre façon d'utiliser les réseaux en supprimant la contrainte des câbles. Elles utilisent les ondes électromagnétiques pour transporter l'information à travers l'air.

### Les fondamentaux de la transmission sans fil

La transmission sans fil repose sur plusieurs concepts essentiels :

1. **Les ondes radio**
   - Porteuses du signal
   - Différentes fréquences selon les usages
   - Comportement variable selon l'environnement
   - Puissance d'émission réglementée

2. **La modulation du signal**
   La modulation est la technique permettant d'encoder l'information numérique sur l'onde porteuse. Il existe plusieurs types :
   - Modulation d'amplitude (AM)
   - Modulation de fréquence (FM)
   - Modulation de phase (PM)
   - Modulations complexes combinant plusieurs techniques

### Les principales technologies

#### Le Wi-Fi (IEEE 802.11)

Le Wi-Fi est devenu incontournable dans nos vies quotidiennes. Son évolution montre l'amélioration constante des performances :

- **802.11b** (1999)
  * Fréquence 2,4 GHz
  * Débit théorique 11 Mbps
  * Première version largement adoptée

- **802.11g** (2003)
  * Fréquence 2,4 GHz
  * Débit théorique 54 Mbps
  * Compatibilité ascendante avec 802.11b

- **802.11n** (2009)
  * Double bande 2,4/5 GHz
  * MIMO (Multiple Input Multiple Output)
  * Débits jusqu'à 600 Mbps

- **802.11ac** (2013)
  * Bande 5 GHz privilégiée
  * MU-MIMO
  * Débits supérieurs à 1 Gbps

- **802.11ax (Wi-Fi 6)** (2019)
  * Efficacité spectrale améliorée
  * OFDMA (Orthogonal Frequency Division Multiple Access)
  * Optimisé pour les environnements denses

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-1-Materiel-physique/Sources/03-006.svg">

### Les autres technologies sans fil

#### Le Bluetooth

Conçu pour les communications à courte portée, le Bluetooth est devenu essentiel pour les périphériques :

- Portée typique de 10 à 100 mètres selon la classe
- Faible consommation d'énergie
- Idéal pour les périphériques mobiles
- Bande de fréquence 2,4 GHz

#### La 5G et les réseaux mobiles

L'évolution des réseaux mobiles illustre les progrès constants des technologies sans fil :

- Débits considérablement augmentés (jusqu'à 10 Gbps)
- Latence réduite (< 1ms)
- Support d'un grand nombre d'appareils connectés
- Utilisation de différentes bandes de fréquences selon les besoins

## 3.5 Les connecteurs et leurs caractéristiques

Les connecteurs sont souvent négligés mais sont pourtant cruciaux pour la qualité de la transmission.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-1-Materiel-physique/Sources/03-007.svg">

### Principaux types de connecteurs

1. **Connecteurs cuivre**
   - RJ45 : le plus commun pour l'Ethernet
   - RJ11 : utilisé pour la téléphonie
   - BNC : ancien connecteur coaxial

2. **Connecteurs fibre optique**
   - LC (Lucent Connector) : compact, très utilisé
   - SC (Subscriber Connector) : robuste et fiable
   - ST (Straight Tip) : ancien mais encore présent

3. **Caractéristiques importantes**
   - Qualité des contacts
   - Facilité d'installation
   - Résistance mécanique
   - Protection contre les interférences
