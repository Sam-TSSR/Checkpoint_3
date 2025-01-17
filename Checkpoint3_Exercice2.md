# CHECKPOINT 3

## Exercice 2 : Manipulations pratiques sur VM Linux

### Partie 1 : Gestion des utilisateurs

#### Q.2.1.1 Sur le serveur, créer un compte pour ton usage personnel.

1. J'ai créé un compte pour mon usage personnel sur le serveur en utilisant la commande `adduser`.
![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.1.1.png)

#### Q.2.1.2 Quelles préconisations proposes-tu concernant ce compte ?

1. Utiliser des mots de passe complexes.
2. Ne pas accorder les privilèges **sudo** à l'utilisateur.

### Partie 2 : Configuration de SSH

#### Q.2.2.1 Désactiver complètement l'accès à distance de l'utilisateur root.

1. J'ai désactivé l'accès à distance de l'utilisateur root en modifiant le fichier de configuration sshd_config. Et j'ai modifié la ligne `PermitRootLogin no`.


    ```bash
    nano /etc/ssh/sshd_config

![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.2.1.png)

#### Q.2.2.2 Autoriser l'accès à distance à ton compte personnel uniquement.

1. J'ai autorisé l'accès à distance uniquement à mon compte personnel en modifiant le fichier de configuration sshd_config. J'ai ajouté la ligne suivante pour spécifier l'utilisateur autorisé :

    ```bash
    AllowUsers wilder123

![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.2.2.png)

#### Q.2.2.3 Mettre en place une authentification par clé valide et désactiver l'authentification par mot de passe

1. J'ai généré une paire de clés SSH avec la commande suivante sur ma machine locale :

    ```bash
    ssh-keygen

2. J'ai copié la clé publique sur le serveur avec la commande :

    ```bash
    ssh-copy-id wilder123@192.168.1.200

3. J'ai désactivé l'authentification par mot de passe en ajoutent le fichier sshd_config :

    ```bash
    PasswordAuthentication no

4. Enfin, j'ai redémarré le service SSH avec la commande :

    ```bash
    systemctl restart sshd


![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.2.3%20-1.png)
![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.2.3%20-2.png)

### Partie 3 : Analyse du stockage

#### Q.2.3.1 Quels sont les systèmes de fichiers actuellement montés ?

1. Pour lister les systèmes de fichiers actuellement montés, j'ai utilisé la commande suivante :

    ```bash
    findmnt

![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.3.1.png)

#### Q.2.3.2 Quel type de système de stockage ils utilisent ?

1. Pour savoir quel type de système de stockage est utilisé par chaque système de fichiers, j'ai utilisé la commande suivante :

    ```bash
    lsblk

2. J'ai trouvé le système de stockage ils utilisent ces type : **disk (sda)**, **part (sda1)**, **raid1 (md0)**, **part (md0p1)**, **part (md0p2)**, **part (md0p2)**, **lvm (cp3--vg-root)**, **lvm (cp3--vg-swap_1)**.

![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.3.2.png)

#### Q.2.3.3 Ajouter un nouveau disque de 8,00 Gio au serveur et réparer le volume RAID

1. Ajout du disque : J'ai ajouté un nouveau disque de 8,00 Gio à la machine virtuelle, qui est apparu sous le nom /dev/sdb.

2. Partitionnement du disque : J'ai utilisé la commande fdisk pour partitionner le disque :

    ```bash
    fdisk /dev/sdb

- Création d'une nouvelle partition avec `n`.
- Définition du type de partition comme RAID avec `t (type fd)`.
- Sauvegarde avec `w`.

3. Ajout du disque au RAID : J'ai utilisé mdadm pour ajouter la nouvelle partition au volume RAID existant :

    ```bash
    mdadm --add /dev/md0 /dev/sdb1

4. Vérification de l'état du RAID : J'ai utilisé la commande suivante :

    ```bash
    lsblk


![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.3.3%20-1.png)
![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.3.3%20-2.png)

#### Q.2.3.4 Ajouter un nouveau volume logique LVM de 2 Gio qui servira à héberger des sauvegardes. Ce volume doit être monté automatiquement à chaque démarrage dans l'emplacement par défaut : /var/lib/bareos/storage.

1. Préparation du disque physique : J'ai créé un volume physique sur le disque /dev/sdc avec la commande suivante :

    ```bash
    pvcreate /dev/sdc

2. Création du Volume Group : J'ai créé un Volume Group appelé **vgwilder** sur le disque /dev/sdc :

    ```bash
    vgcreate vgwilder /dev/sdc

3. Création du Logical Volume : J'ai créé un Logical Volume de 512 MiB, nommé backup_volume, dans le Volume Group **backupvolume**

    ```bash
    lvcreate -L 512M -n backupvolume vgwilder

4. Formatage du Logical Volume : J'ai formaté le Logical Volume avec le système de fichiers ext4 :

    ```bash
    mkfs.ext4 /dev/vgwilder/backupvolume

5. Montage du volume logique : J'ai monté le Logical Volume dans le répertoire /var/lib/bareos/storage :

    ```bash
    mount /dev/vgwilder/backupvolume /var/lib/bareos/storage

6. Mise à jour de /etc/fstab : J'ai ajouté le volume logique à /etc/fstab pour qu'il soit monté automatiquement au démarrage du système :

    ```bash
    /dev/vgwilder/backupvolume  /var/lib/bareos/storage  ext4  defaults  0  2

7. Vérification : J'ai vérifié le montage avec la commande `lsblk`.


![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.3.4%20-1.png)
![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.3.4%20-2.png)

#### Q.2.3.5 Combien d'espace disponible reste-t-il dans le groupe de volume ?

1. Il reste 383 MiB d'espace disponible dans le groupe de volume vgwilder.

![SS](https://github.com/Sam-TSSR/Checkpoint_3/blob/8d007b8cee83ea9b3b6e308f4673f26c7f728770/Captures%20d'%C3%A9cran/Debian/Q.2.3.5.png)

### Partie 4 : Sauvegardes

#### Q.2.4.1 Expliquer succinctement les rôles respectifs des 3 composants bareos installés sur la VM.

- bareos-dir : Il contrôle tout et gère. Il est responsable de la configuration, de la définition et du lancement des sauvegardes.
- bareos-sd : Il permet de réaliser des sauvegardes sur différents types de stockage.
- bareos-fd : Il est responsable de la gestion des fichiers et de la collecte des informations à sauvegarder.

