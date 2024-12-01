# 4. La transmission de l'information

## 4.1 Principes de base de la transmission des signaux

La transmission des données numériques n'est pas aussi simple qu'il n'y paraît. Ce que nous percevons comme des "0" et des "1" doit être converti en signaux physiques qui peuvent voyager à travers différents supports de transmission.

### Les bases de la transmission numérique

Une donnée numérique peut être transmise de plusieurs façons :

- **La transmission en tension**
  * Une tension haute représente un "1"
  * Une tension basse représente un "0"
  * Simple à mettre en œuvre sur courte distance
  * Sensible aux perturbations électromagnétiques
  * Utilisée principalement en Ethernet sur cuivre

- **La transmission optique**
  * La présence de lumière représente un "1"
  * L'absence de lumière représente un "0"
  * Immunisée aux perturbations électromagnétiques
  * Nécessite une conversion électrique/optique
  * Utilisée dans les fibres optiques

- **La transmission radio**
  * Utilise des ondes électromagnétiques
  * Nécessite une modulation du signal
  * Peut utiliser différentes fréquences
  * Sensible aux interférences et aux obstacles
  * Base des technologies sans fil

### Les techniques de codage

Il existe plusieurs façons de coder l'information pour la rendre plus fiable lors de sa transmission :

1. **Le codage NRZ (Non Return to Zero)**
   - Le plus simple et le plus direct
   - Niveau haut maintenu pour un "1"
   - Niveau bas maintenu pour un "0"
   - Avantages :
     * Facile à implémenter
     * Utilisation efficace de la bande passante
   - Inconvénients :
     * Perte de synchronisation sur longues séquences identiques
     * Présence d'une composante continue
     * Sensible aux distorsions

2. **Le codage Manchester**
   - Transition au milieu de chaque bit
   - Montante pour un "1"
   - Descendante pour un "0"
   - Avantages :
     * Auto-synchronisation (clock recovery)
     * Pas de composante continue
     * Détection d'erreurs facilitée
   - Inconvénients :
     * Double bande passante nécessaire
     * Plus complexe à implémenter
     * Utilisé notamment dans l'ancien Ethernet 10BASE-T

3. **Le codage MLT-3 (Multi-Level Transmission)**
   - Utilise trois niveaux de signal
   - Change de niveau à chaque "1"
   - Maintient le niveau pour les "0"
   - Avantages :
     * Réduction des interférences électromagnétiques
     * Meilleure utilisation de la bande passante
     * Utilisé dans le Fast Ethernet
   - Inconvénients :
     * Plus complexe à décoder
     * Nécessite une électronique plus sophistiquée

## 4.2 Gestion des perturbations et qualité du signal

La transmission de données numériques dans le monde réel fait face à de nombreux défis. Même le meilleur système de transmission doit composer avec différentes formes de perturbations qui peuvent dégrader la qualité du signal.

### Les types de perturbations fondamentales

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-1-Materiel-physique/Sources/04-001.svg">

L'atténuation représente le premier défi majeur de toute transmission. Imaginez une voix qui s'affaiblit avec la distance : c'est exactement ce qui se passe avec nos signaux numériques. Plus le signal parcourt une longue distance, plus il perd en puissance. Cette perte n'est pas uniforme : certaines fréquences sont plus affectées que d'autres, ce qui peut déformer le signal d'origine. Pour combattre ce phénomène, les réseaux modernes utilisent des amplificateurs ou des répéteurs, stratégiquement placés le long du parcours de transmission. Ces dispositifs permettent de régénérer le signal avant qu'il ne devienne trop faible pour être correctement interprété.

La diaphonie constitue un second défi majeur, particulièrement présent dans les câbles multiconducteurs comme les câbles réseau. Ce phénomène se produit lorsqu'un signal transitant dans un conducteur interfère avec les signaux des conducteurs voisins. C'est un peu comme si vous entendiez la conversation de votre voisin sur votre propre ligne téléphonique. Dans les réseaux informatiques, la diaphonie peut sérieusement perturber la transmission de données. C'est pourquoi les câbles réseau modernes utilisent des paires torsadées : l'entrelacement des fils aide à annuler les interférences mutuelles. De plus, le blindage des câbles offre une protection supplémentaire contre ces perturbations.

Le bruit électromagnétique représente une troisième source majeure de perturbations. Notre environnement moderne est saturé d'ondes électromagnétiques provenant de diverses sources : téléphones mobiles, moteurs électriques, éclairages, et bien d'autres équipements. Ces perturbations peuvent s'introduire dans nos systèmes de transmission et altérer les signaux. Les effets peuvent aller d'une légère distorsion à une corruption complète des données. La lutte contre le bruit électromagnétique passe par plusieurs stratégies : le blindage des câbles, bien sûr, mais aussi une mise à la terre soignée des équipements et un routage intelligent des câbles pour éviter les sources de perturbations les plus importantes.

### Les solutions modernes

Face à ces défis, les technologies modernes de transmission ont développé des stratégies sophistiquées. L'égalisation du signal, par exemple, permet de compenser les distorsions en appliquant des corrections qui restaurent la forme originale du signal. C'est un peu comme un égaliseur audio qui ajusterait automatiquement différentes fréquences pour obtenir le meilleur son possible.

Le contrôle d'erreur joue également un rôle crucial. Les systèmes modernes ne se contentent pas de détecter les erreurs : ils peuvent souvent les corriger à la volée. Quand une correction directe n'est pas possible, des mécanismes de retransmission automatique entrent en jeu, garantissant l'intégrité des données même dans des conditions difficiles.

# 4.3 Performances et mesure de la qualité de transmission

En matière de réseaux, la performance n'est pas qu'une question de vitesse. Elle englobe plusieurs aspects qui, ensemble, déterminent la qualité globale d'une transmission.

<img src="https://raw.githubusercontent.com/No-Name-Academy/Networking-for-noobs/refs/heads/main/1-1-Materiel-physique/Sources/04-002.svg">

Le débit représente la quantité d'informations pouvant transiter par unité de temps. C'est comme le débit d'eau dans un tuyau : plus le tuyau est large, plus le débit potentiel est important. Cependant, le débit réel dépend de nombreux facteurs : la qualité du support de transmission, les équipements utilisés, et même la distance à parcourir. Un câble en fibre optique moderne peut atteindre des débits de plusieurs térabits par seconde, tandis qu'une connexion sans fil sera généralement plus modeste.

La latence, quant à elle, représente le temps nécessaire à un paquet pour aller d'un point à un autre du réseau. Elle est cruciale pour les applications en temps réel comme la visioconférence ou les jeux en ligne. Une latence élevée se traduit par des délais perceptibles qui peuvent rendre ces applications inutilisables. La latence dépend non seulement de la distance physique à parcourir, mais aussi du nombre d'équipements traversés et de leur charge de travail.

La gigue, souvent négligée mais tout aussi importante, représente la variation de la latence dans le temps. Imaginez une conversation où les mots arrivent tantôt rapidement, tantôt avec retard : c'est exactement ce que provoque une gigue élevée sur une communication réseau. Les équipements modernes utilisent des tampons (buffers) pour compenser la gigue, mais cela se fait au prix d'un délai supplémentaire.

# Conclusion du module

Au terme de ce module sur le matériel physique, nous avons exploré les fondements mêmes des réseaux informatiques, depuis les premiers câbles coaxiaux jusqu'aux technologies sans fil modernes. Cette compréhension des aspects physiques est essentielle car elle constitue la base sur laquelle reposent toutes les couches supérieures du modèle OSI. 

La diversité des supports de transmission et des techniques utilisées reflète la complexité des besoins en communication moderne : haute performance pour les centres de données, fiabilité pour les infrastructures critiques, mobilité pour les utilisateurs finaux. Chaque technologie présente ses avantages et ses contraintes, et le choix de la solution appropriée dépend toujours du contexte spécifique de déploiement. Cette connaissance des fondamentaux permet non seulement de mieux concevoir les réseaux, mais aussi d'en assurer efficacement la maintenance et l'évolution.