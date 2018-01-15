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
