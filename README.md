*UE3 Prosit 3 - Principe de routage*

# Mots Clés
- ROMMON = IOS "restreint"
- Boot = Séquence de démarrage d'un équipement
- Inter VLAN
- Routeur = équipement de niveau 3 permettant l'accès à internet et la communication inter réseau
- Routage
- Bootstrap = Va chercher l'IOS et la startup-config

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
- Image peut-être récupérée sur le réseau ou sur un périphérique bootable.

# Plan d'action
## Etudes
- Boot d'un routeur  
  [Site explication démarrage Cisco](https://fr.scribd.com/doc/28590448/DEMARRAGE-ROUTEUR)  
  Séquence de démarrage d'un routeur Cisco :
   * Autotest POST - ROM - Test Matériel
   * Recherche de l'IOS - Boot en NVRAM
      * Dans la FLASH et la charge dans la RAM, sinon
      * Sur un serveur TFTP et la charge dans la RAM, sinon
      * En ROM démarre en mode RXBoot
   * Recherche du fichier de configuration
      * Si l'IOS est trouvé en FLASH ou sur un TFTP, recherche le fichier startup-config dans la NVRAM et le charge en RAM en tant que running-config
      * Sinon, cherche sur un serveur TFTP un fichier de Startup-config et le charge en RAM en tant que running-config
      * Sinon, démarre en mode de configuration. Une interface qui pose des questions à l'usager pour créer une config de base

- Principe de routage
  * Commutation de processus, tous les paquets sont analysé et comparé à la table de routage puis transféré. Très lent, pratiquement plus utilisé. (Comparaison : Effectue un calcul à la main, même si le calcul s'est déjà posé)
  * Commutation rapide : Stocke les informations des paquets précédent et si un paquet ayant la même adresse de destination arrive, le traite beaucoup plus rapidement. (Comparaison : Effectue un calcul à la main et se souvient de la solution au cas où le calcul se représenterai)
  * CEF (Cisco Express Forwarding) : Mode de transfert le plus rapide et le plus utilisé, générant une table FIB permettant le transfert très rapide des paquets. (Comparaison : Effectue à l'avance tous les calculs possibles sur le réseau et se souvient de toutes les solutions)

  Le routage permet de définir des routes, cad permettre à un routeur de savoir sur quel port envoyer une information destinée à un sous-réseau ou un poste particulier
  Deux types de routes :
   * Routes Distantes
   * Routes Directes

   Deux types de routage :
   * Routage dynamique :
      * Complexité de configuration indépendante de celle du réseau 
      * S'adapte automatiquement aux changements de topologie
      * Evolutivité idéale pour les topologies simples et complexes
      * Moins sécurisé qu'une route statique
      * Utilise le CPU, la mémoire, la bande passante
      * La route dépend de la topologie en cours

   * Routage statique :
      * Complexité de configuration augmente avec celle du réseau
      * L'adaptation nécessite l'intervention d'un admin
      * Evolutivité idéale pour les topologies simples
      * Plus sécurisé qu'une route dynamique
      * Aucune ressource supplémentaire n'est requise
      * La route menant à la destination est toujours la même

- Routage inter VLAN

 * Routeur On Stick : Qd deux VLAN différents, on se sert d'un routeur et d'une sous-interface (ex : fastEthernet 0/0.10 (dot1Q)) pour les faire communiquer entre eux.  
   CMD : inter giga 0/1.20 --> encapsulation dot1Q *vlan* --> switchport mode trunk --> switchport trunk allowed *vlan,vlan,vlan*

## Réalisations
- Reconfigurer le routeur

- Utiliser du routage static