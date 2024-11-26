# Le routage

## 1. Introduction au routage et protocole IP

### 1.1 Fondamentaux du routage

Le routage est le mécanisme fondamental qui permet aux informations de transiter d'un réseau à un autre. Pour bien comprendre ce concept, nous devons explorer :

- Comment les données sont organisées au niveau de la couche 3
- Quel matériel permet la communication entre réseaux
- Comment les machines communiquent entre différents réseaux

> 💡 Note importante : Bien que nous mentionnions la commande `ifconfig` dans ce cours, celle-ci n'est plus installée par défaut sur les distributions récentes Debian/RedHat. Vous pouvez l'installer via :
>
> - Debian/Ubuntu : `sudo apt-get install net-tools`
> - RedHat/CentOS : `sudo yum install net-tools`
>
> A noter que sur windows, toutes les informations seront obtenues avec les simples commandes ipconfig et netstat disponibles sur toutes les éditions.

### 1.2 Le protocole IP

#### Qu'est-ce qu'un protocole IP ?

Un protocole est essentiellement un langage permettant aux machines de communiquer entre elles. Pour la couche 3 du modèle OSI, nous utilisons le protocole IP (Internet Protocol), qui définit la manière dont les données sont formatées et transmises.

#### Les composants essentiels

Dans sa forme la plus basique, le protocole IP nécessite deux informations fondamentales :

- L'adresse IP source (émetteur)
- L'adresse IP destination (récepteur)

#### Le masque réseau dans IP

Une question importante se pose : devons-nous inclure le masque réseau dans l'en-tête IP ? Pour répondre à cette question, analysons un exemple concret :

Imaginons une machine A (192.168.0.1/24) souhaitant communiquer avec une machine B (192.168.1.1/24) :

- La machine A doit d'abord déterminer si B est sur son réseau
- Pour cela, A examine sa propre plage d'adresses (192.168.0.0 à 192.168.0.255)
- A constate que 192.168.1.1 n'appartient pas à sa plage
- A en déduit que B est sur un autre réseau et qu'il faut utiliser la couche 3

Cette analyse révèle que le masque de B n'est pas nécessaire pour la communication - seule l'adresse IP suffit.

### 1.3 Format du datagramme IP

Le datagramme (ou paquet) IP organise les informations de manière structurée. Sa structure de base est la suivante :

```plaintext
+----------------+----------------+------------------+
|  En-tête IP    | Adresse IP    | Adresse IP      |
|  (autres info) |    Source     |   Destination   |
+----------------+----------------+------------------+
```

#### Particularité de l'en-tête IP

Une caractéristique intéressante de l'en-tête IP est la position de l'adresse de destination, qui n'est pas au début de l'en-tête. Ceci peut sembler contre-intuitif, surtout si l'on compare avec la couche 2 où l'adresse MAC de destination est placée en premier.

Cette différence s'explique par le processus de traitement des paquets :

- À la couche 2, la machine doit rapidement déterminer si la trame lui est destinée
- À la couche 3, ce contrôle a déjà été effectué par la couche 2
- La position de l'adresse IP de destination est donc moins critique
- Ce positionnement permet d'optimiser le traitement avec les informations de la couche 4