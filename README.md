
*UE3 Prosit 3 - Principe de routage*

# Mots Clés
- ROMMON : ROM ON
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

https://www.it-connect.fr/wp-content-itc/uploads/2013/11/RoutageInterVLAN01-550x271.png

**Mise en place**

**1** - Créer deux VLAN et les nommer mais aussi un VLAN natif (99)

**2** - On configure les interface des switchs entre eux :

*Switch(config)#interface fa0/1*

*Switch(config-if)#switchport mode trunk *

*Switch(config-if)#switchport trunk allowed vlan 20,30,99*

*Switch(config-if)# switchport trunk native vlan 99*

*Switch(config-if)#no shutdown*

**3** - On atribue aux interfaces du switch qui sont reliés aux postes le bon VLAN :

*Switch(config)#interface fa0/10*

*Switch(config-if)#switchport access vlan 20*

*Switch(config-if)#no shutdown*

**4** - Tester la connectivité

**Si on veut faire communiquer deux LAN isolés par des VLAN, il est nécessaire de faire intervenir un routeur**

Celà s'appelle : Router-on-stick.

## 3- Router-on-stick :

Pour  communiquer d'un VLAN à un autre, il est obligatoire de passer par un routeur.

**1** - Active l'interface du routeur Fa0/0

**2** - Créer l'interface Fa0/0.1 et Fa0/0.2

Nous admettons que  Fa0/0.1 sera la passerelle vers VLAN20 et 0.2 sur VLAN30 :

*Router(config)#interface fa0/0.1*

*Router(config-subif)#encapsulation dot1Q 20*

*Router(config-subif)#ip address 192.168.20.254 255.255.255.0*

*Router(config-subif)#no shutdown*
et

*Router(config)#interface fa0/0.2*

*Router(config-subif)#encapsulation dot1Q 30*

*Router(config-subif)#ip address 192.168.30.254 255.255.255.0* 

*Router(config-subif)#no shutdown*

**expliquons la dot1q**

La norme 802.1q indique que une trame est étiquetée pour contenir un numéro de VLAN auquel elle est attribuée. La commande permet donc d'encapsuler une trame pour transiter sur le numéro du VLAN qu'on lui indique directement sur le routeur.

**3** - Passer le switch connecté au routeur :

*Switch(config)#interface fa0/24*

*Switch(config-if)#switchport mode trunk*

*Switch(config-if)#switchport trunk allowed vlan 20,30,99*

*Switch(config-if)# switchport trunk native vlan 99*

*Switch(config-if)#no shutdown*

**4**-  On met la Gateway sur les postes, qui correspond à l'interface que l'on a crée précédemment.

**Inconvénients** :
Celà crée un goulot d'étranglement, tout le trafic inter-VLAN allant d'un port à un autre SWITCH devra remonter par ce lien :

https://www.networklab.fr/wp-content/uploads/2014/01/83.jpg

**nb** : Il également une autre configuration (moins conseillée si trop de VLAN pour éviter ce goulot, cette configuration peut-être réalisée à l'aide d'un switch L3.

https://www.networklab.fr/wp-content/uploads/2014/01/102.jpg

Il faut créer des SVI sur chaque SWITCH, activer la fonction de rouage sur les SWITCH, configurer un Switch comme un routeur en utilisant EIGRP, un protocole de routage CISCO...

Configuration : https://www.networklab.fr/routage-inter-vlan-et-switch-l3/

## 4- Boot d'un routeur :

- POST (power on selft test), il teste le matériel
- CONFREG : Lire la valeur du registre
	- Si  0x???0 -> Démarer en mode ROM
	- Si 0x???1 -> Démarrer en mode ROMMON
	- 0x???2 -> 0x???F -> Lire la flash
- Chercher l'IOS
	- Trouve => Charge
	- Trouve pas, va check si disponible sur un FTP
	- Trouve pas ou Invalide => mode ROMMON
- Vérifier le 7eme bit du registre, si il est ) 1, il passe le fichier de conf qui se trouve dans la NVRAM
	- Si il est à 0, il charge le fichier startup.conf dans la NVRAM
- Si il trouve le fichier de config, il le charge
	- Sinon, il entre dans le setup conf.mod
	




