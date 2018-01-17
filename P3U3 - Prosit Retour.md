*UE3 Prosit 3 - Principe de routage*

# Mots Clés
- ROMMON : IOS restrain
- Boot
- Inter VLAN
- Routeur
- Routage
- Bootstrap : Protocol d'amorcage, permet de démarrer le startup-config.

# Plan d'action
## Etudes
### Boot d'un routeur

1- POST
2- Bootstrap
    - Recuperation de l'OS
        - Flash
        - Server TFTP
        - ROM -> Limité

### Principe de routage
2 types de config
- Static
    Pros :
        - Economie de bande passante
        - Sécurité : pas de diffusion
        - Connaissance du chemin a l'avance
    Cons :
        - Pour un grand réseau : difficile de configurer
        - Problèmes d'évolution
- Dynamique

Routeur :
On-stick | Couche 3
---------|---------
+ prix | - prix
- perf | + perf

Principe de routage : permet la communication entre plusieurs réseaux locals. Couche 3 (Modele OSI)

Commandes (config d'un roueur)
```
R1(config)# ip route [dest] [mask] [adresse routeur distant / port / distance administrative (1 par défaut)]
R1(config)# show ip route       // Affiche la table de routage
```

### Routage inter VLAN
Permet de faire communiquer 2 VLAN avec un routeur (ou switch l3)
- Interfaces en mode trunk entre routeur et switch
Cons
- Peut réduire le débit
```
R1(config-if)# switchport mode trunk
R1(config-if)# switchport mode allowed [vlan], [...]
```

## Réalisations
- Reconfigurer le routeur
