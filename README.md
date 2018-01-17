
*UE3 Prosit 3 - Principe de routage*

# Mots Clés
- ROMMON
- Boot
- Inter VLAN
- Routeur
- Routage
- Bootstrap

# Besoins
**Quoi** : Comprendre le problème du routeur.
**Comment** : En reconfigurant le routeur.
**Pourquoi** : Routeur ne boot pas.

# Contraintes
- Prendre en compte le routage inter VLAN
- Utilisation d'un routeur Cisco 1841

# Problématique
- Comment configurer un routeur en prenant en compte le routage inter VLAN ?
- Comment boot un routeur ?

# Généralisation
- MCO

# Hypothèse
- Il n'y a pas de bouton reboot sur un routeur / switch.
- Un routeur inter VLAN permet faire communiquer plusieurs VLAN entre eux.
- L'image IOS n'est pas présente dans le routeur.
- Affichage d'une image IOS présent dans la ROM.
- image peut-être récupérée sur le réseau ou sur un périphérique bootable.

# Plan d'action
## Etudes
- Boot d'un routeur 
- Principe de routage
- Routage inter VLAN

## Réalisations
- Reconfigurer le routeur
- Utiliser du routage static

## 1- Configurer son routeur Cisco :

Il existe deux types de configurations :
****
- Statique :
Les administrateur vont config les routeurs un à un au sein du réseau afin d'y saisir les routes (par un socket). 
	- Économie de bande passante (aucune info ne transite entre les routeurs pour qu'ils se tiennent à jour).
	- Sécurité : Vu qu'il n'y a pas de routage dynamique, il ne diffuse pas d'informations sur le réseau. Les informations sont définitives et ne peuvent pas être altérer.
	- Connaissance du chemin à l'avance : L'admin ayant configuré l'ensemble de la topologie saura exactement par où passent les paquets pour aller d'un réseau à un autre, pouvant par exemple permettre une compréhension plus simpliste en cas d'incident).

**Désavantages :**
- La configuration de réseau de taille importante peut devenir assez longue & complexe. Il faut en effet connaître l'intégralité de la topologie pour saisir les informations de manière exhaustive (ordonnée) et correcte pour garantir le fonctionnement. Quand le réseau est assez grand, ça peut devenir une source d'erreur.
- A chaque fois que le réseau évolue il faut que chaque routeur soit au courant de l'évolution faite sur un autre routeur.

Le routage statique est donc intéressant pour les petits réseaux de quelques routeurs, ayant peu d'évolutions.
****
* Dynamique : 
MAJ auto, c'est le protocole de routage qui permet au routeur de se comprendre et d'échanger des informations de manière périodique ou événementielle.

https://www.it-connect.fr/wp-content-itc/uploads/2013/12/StatDyna04-550x232.png

Pour chaque nouveau réseau auquel il se connecte, le message est transmis de routeur en routeur, mettant à jour leurs tables.
Même principe si un routeur tombe en panne, les tables de routages vont se modifier pour utiliser une autre route.

Ils échangent régulièrement des "hello" indiquant que l'hôte est toujours joignable.
Des calculs de routes sont fait en fonction de la vitesse des lies les liants, du nombre de routeur à traverser... Tout dépend du protocole de routage.

**Avantages :** 

- Maintenance réduite
- Modularité et flexibilité accrue, l'évolution du réseau est plus facile à mettre en place
- Performance et mise en place ne dépendent pas de la taille du réseau

**DESAVANTAGES**

- Peut-être plus compliqué à mettre en place lors de son initialisation
- Consomme la bande passante (messages envoyés périodiquement)
- Diffusion automatique sur le réseau = danger car un attaquant peut obtenir des infos sur la topologie du réseau, voir même créer et se faire passer pour un membre du réseau.
- Le traitement des messages réseau et le calcul des meilleurs routes => consommation CPU et RAM supplémentaire qui peut "encombre" certains éléments du réseau peu puissant.


## 2- Le routage Inter-VLAN :


