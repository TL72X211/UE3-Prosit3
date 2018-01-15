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
  [Site explication démarrage Cisco](https://fr.scribd.com/doc/28590448/DEMARRAGE-ROUTEUR)
  Séquence de démarrage d'un routeur Cisco :
   * Autotest POST - ROM - Test Matériel
   * Recherche de l'IOS - Boot en NVRAM
    * Dans la FLASH et la charge dans la RAM, sinon
    * Sur un serveur TFTP et la charge dans la RAM, sinon
    * En ROMn démarre en mode RXBoot
   * Recherche du fichier de configuration
    * Si l'IOS est trouvé en FLASH ou sur un TFTP, recherche le fichier startup-config dans la NVRAM et le charge en RAM en tant que running-config
    * Sinon, cherche sur un serveur TFTP un fichier de Startup-config et le charge en RAM en tant que running-config
    * Sinon, démarre en mode de configuration. Une interface qui pose des questions à l'usager pour créer une config de base

- Principe de routage
  

- Routage inter VLAN

	Routage statique : (https://www.cisco.com/c/en/us/td/docs/switches/datacenter/sw/5_x/nx-os/unicast/configuration/guide/l3_cli_nxos/l3_route.html)

## Réalisations
- Reconfigurer le routeur
- Utiliser du routage static
