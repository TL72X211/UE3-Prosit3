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

Etudes

## Boot d'un routeur

![boot_img](/ress/boot.png)

## Principe de routage statique :

Routage statique :

Commande pour configurer une route statique : « ip route AddRéseau AddSousRéseau ViaAddRouteur Interface TTL »
Commande pour supprimer une route statique : « no ip route AddRéseau AddSousRéseau ViaAddRouteur»
Afficher table de routage : « show ip route »

Algorithme de configurations de route statique : 
- Step 1 : configuration de l’interface
- Step 2 : création des routes statiques (ip route)

![conf](/ress/conf1.png)

Les avantages du routage statique sont :
- L’économie de la bande passante 
- La sécurité
- La connaissance du chemin à l’avance

Les désavantages du routage statique sont :
- La complexité de la configuration 
- La maintenant pour chaque modification de la topologie du réseau


## Routage inter VLAN

![routage](/ress/routage.png)

Réalisations:

## Reconfigurer le routeur
## Utiliser du routage static
