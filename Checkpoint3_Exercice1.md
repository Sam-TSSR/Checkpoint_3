# CHECKPOINT 3

## Exercice 1 : Manipulations pratiques sur VM Windows

### Partie 1 : Gestion des utilisateurs

#### Q.1.1.1 Créer l'utilisateur Lionel Lemarchand avec les même attribut de société que Kelly Rhameur.
1. J'ai ouvert la console Active Directory Users and Computers.
2. J'ai sélectionné "New User" et créé un nouvel utilisateur nommé Lionel Lemarchand.
3. J'ai rempli les champs nécessaires (nom, e-mail, société) en utilisant les mêmes informations que **Kelly Rhameur** pour la société.
4. J'ai appliqué une politique qui oblige l'utilisateur à changer son mot de passe lors de la première connexion.

![SS]()
![SS]()

#### Q.1.1.2 Créer une OU DeactivatedUsers et déplace le compte désactivé de Kelly Rhameur dedans.
1. J'ai bien désactivé l'utilisateur et je l'ai déplacé dans l'OU DeactivatedUsers.

![SS]()

#### Q.1.1.3 Modifier le groupe de l'OU dans laquelle était Kelly Rhameur en conséquence.

1. J'ai supprimé **Kelly Rhameur** des groupes non nécessaires.
2. J'ai créé un groupe comme DeactivatedUsersGroup sous l'OU DeactivatedUsers. 
3. J'ai ajouté **Kelly Rhameur** au groupe destiné aux utilisateurs désactivés comme DeactivatedUsersGroup.

![SS]()
![SS]()

#### Q.1.1.4 Créer le dossier Individuel du nouvel utilisateur et archive celui de Kelly Rhameur en le suffixant par -ARCHIVE.

1. J'ai créé le dossier Individuel du nouvel utilisateur **Lionel Lemarchand**.
2. J'ai localisé **Kelly Rhameur** et J'ai renommé son dossier en ajoutant le suffixe **-ARCHIVE**

![SS]()

### Partie 2 : Restriction utilisateurs

#### Q.1.2.1 Faire en sorte que l'utilisateur Gabriel Ghul ne puisse se connecter que du lundi au vendredi, de 7h à 17h.

1. J'ai configuré les heures de connexion pour l'utilisateur Gabriel Ghul dans Active Directory afin qu'il puisse se connecter uniquement du lundi au vendredi, de 7h à 17h.

![SS]()

#### Q.1.2.2 De même, bloquer sa connexion au seul ordinateur CLIENT01.

1. J'ai configuré l'utilisateur Gabriel Ghul pour qu'il ne puisse se connecter qu'à l'ordinateur CLIENT01.

![SS]()

#### Q.1.2.3 Mettre en place une stratégie de mot de passe pour durcir les comptes des utilisateurs de l'OU LabUsers.

1. J'ai créé une Policy dans l'OU LabUsers.
2. J'ai configuré une stratégie de mot de passe pour renforcer la sécurité des comptes des utilisateurs dans l'OU LabUsers.

![SS]()

### Partie 3 : Lecteurs réseaux

#### Q.1.3.1 Créer une GPO Drive-Mount qui monte les lecteurs E: et F: sur les clients.

1. J'ai créé une GPO appelée Drive-Mount qui permet de monter automatiquement les lecteurs E: et F: sur les clients.

![SS]()
![SS]()