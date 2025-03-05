# Tutoriel : Configuration d'un Serveur VPS

## Table des Matières

- [Tutoriel : Configuration d'un Serveur VPS](#tutoriel--configuration-dun-serveur-vps)
  - [Table des Matières](#table-des-matières)
  - [Introduction](#introduction)
  - [Prérequis](#prérequis)
  - [Étape 1 : Mise à Jour du Système](#étape-1--mise-à-jour-du-système)
  - [Étape 2 : Installation des Outils Nécessaires (selon mon utilisation)](#étape-2--installation-des-outils-nécessaires-selon-mon-utilisation)
    - [1. Installation des Outils](#1-installation-des-outils)
    - [2. Configuration pour Tous les Utilisateurs](#2-configuration-pour-tous-les-utilisateurs)
      - [2.1. les Chemins d'Accès](#21-les-chemins-daccès)
      - [2.2. Configurer Poetry](#22-configurer-poetry)
      - [2.3. Configurer pipx](#23-configurer-pipx)
    - [3. Vérification](#3-vérification)
  - [Étape 3 : Configuration des Groupes d'Utilisateurs](#étape-3--configuration-des-groupes-dutilisateurs)
    - [1. Création des Groupes](#1-création-des-groupes)
    - [2. Création des Répertoires Partagés](#2-création-des-répertoires-partagés)
    - [3. Attribution des Répertoires aux Groupes](#3-attribution-des-répertoires-aux-groupes)
    - [4. Configuration des Permissions](#4-configuration-des-permissions)
    - [5. Ajoutez des utilisateurs aux groupes :](#5-ajoutez-des-utilisateurs-aux-groupes-)
    - [6. Création de Nouveaux Utilisateurs (si nécessaire)](#6-création-de-nouveaux-utilisateurs-si-nécessaire)
    - [7. Vérification](#7-vérification)
  - [Étape 4 : Sécurisation du Serveur](#étape-4--sécurisation-du-serveur)
    - [1. Changer le Port SSH](#1-changer-le-port-ssh)
      - [1.1 Modifiez le fichier de configuration SSH](#11-modifiez-le-fichier-de-configuration-ssh)
      - [1.2 Trouver la Ligne du Port](#12-trouver-la-ligne-du-port)
      - [1.3 Enregistrer et Quitter](#13-enregistrer-et-quitter)
      - [1.4 Redémarrer le Service SSH : Pour appliquer les modifications, redémarrez le service SSH](#14-redémarrer-le-service-ssh--pour-appliquer-les-modifications-redémarrez-le-service-ssh)
    - [2. Désactiver l'Authentification par Mot de Passe](#2-désactiver-lauthentification-par-mot-de-passe)
    - [3. Configurer l'Authentification par Clé SSH](#3-configurer-lauthentification-par-clé-ssh)
      - [3.1 Générer une Paire de Clés SSH sur le Client](#31-générer-une-paire-de-clés-ssh-sur-le-client)
      - [3.2 Copier la Clé Publique sur le Serveur :](#32-copier-la-clé-publique-sur-le-serveur-)
      - [3.3 Configurer le Serveur SSH](#33-configurer-le-serveur-ssh)
      - [3.4 Redémarrer le Service SSH](#34-redémarrer-le-service-ssh)
      - [3.5 Se Connecter au Serveur](#35-se-connecter-au-serveur)
  - [Étapes 5 : Renforcement de la Sécurité](#étapes-5--renforcement-de-la-sécurité) -
    [1. Configurer un Pare-feu avec ufw](#1-configurer-un-pare-feu-avec-ufw) -
    [2. Autoriser les Connexions sur des Ports Spécifiques :](#2-autoriser-les-connexions-sur-des-ports-spécifiques-) -
    [2.1 Autoriser d'Autres Ports (si nécessaire) :](#21-autoriser-dautres-ports-si-nécessaire-) -
    [2.2 Vérifier l'État de ufw](#22-vérifier-létat-de-ufw) -
    [3. Installer et Configurer Fail2Ban](#3-installer-et-configurer-fail2ban) -
    [4. Redémarrer Fail2Ban :](#4-redémarrer-fail2ban-) -
    [5. Vérifier l'État de Fail2Ban](#5-vérifier-létat-de-fail2ban)
  - [Étape 6 : (Optionnel) Scripts de Monitoring](#étape-6--optionnel-scripts-de-monitoring)
    - [1. Script de Monitoring Simple](#1-script-de-monitoring-simple)
    - [1. Automatisation avec Cron](#1-automatisation-avec-cron)
    - [2. Script de Monitoring Interactif](#2-script-de-monitoring-interactif)
  - [Étape 6 : Script de Backup](#étape-6--script-de-backup)
    - [1. Automatisation avec Cron](#1-automatisation-avec-cron-1)
  - [Conclusion](#conclusion)
  - [Remarques](#remarques)

## Introduction

Ce tutoriel vous guide à travers les étapes de configuration d'un serveur VPS simple, y compris
l'installation de certains outils nécessaires, la sécurisation du serveur, et la mise en place de
scripts de monitoring et de backup.

## Prérequis

- Un VPS avec un système d'exploitation Linux (Ubuntu, Debian, etc.)
- Accès root ou utilisateur avec privilèges sudo

## Étape 1 : Mise à Jour du Système

Avant de commencer, assurez-vous que votre système est à jour. Si la configuration s'étale sur
plusieurs jours, n'hésitez pas à mettre à jour le système avant de reprendre.

```bash
sudo apt update && sudo apt upgrade -y
```

## Étape 2 : Installation des Outils Nécessaires (selon mon utilisation)

### 1. Installation des Outils

Installez les outils et logiciels suivants si nécessaire :

- **Python3** : Un langage de programmation populaire, utilisé pour le développement de scripts et
  d'applications.
- **pip** : Le gestionnaire de paquets pour Python, utilisé pour installer et gérer les
  bibliothèques Python.
- **x11-apps** : Un ensemble d'applications graphiques pour les systèmes Linux, utilisé pour les
  interfaces graphiques.
- **poetry** : Un outil de gestion des dépendances et des environnements pour les projets Python.
- **pipx** : Un installateur de paquets Python qui permet d'installer et d'exécuter des applications
  Python dans des environnements isolés.
- **ensurepath** : Une commande utilisée pour s'assurer que les chemins nécessaires sont ajoutés à
  votre variable d'environnement PATH.

```markdown
# Installer Python3 et pip

sudo apt update sudo apt install python3 python3-pip -y

# Installer x11-apps pour les applications graphiques

sudo apt install x11-apps -y

# Installer Poetry pour la gestion des dépendances Python

sudo curl -sSL https://install.python-poetry.org | sudo python3 -

# Installer pipx pour les applications Python isolées

sudo apt install pipx -y sudo pipx ensurepath
```

### 2. Configuration pour Tous les Utilisateurs

Pour rendre ces outils disponibles pour tous les utilisateurs, y compris ceux à venir, nous devons
ajuster les chemins d'accès et configurer les environnements.

#### 2.1. les Chemins d'Accès

Ajoutez les chemins d'accès aux exécutables dans un fichier global comme `/etc/profile` ou créez un
script dans `/etc/profile.d/`.

**- Créer un Script dans /etc/profile.d/ :**

```bash
echo 'export PATH=$PATH:/usr/local/bin:/home/linuxbrew/.linuxbrew/bin' | sudo tee /etc/profile.d/custom_paths.sh
sudo chmod +x /etc/profile.d/custom_paths.sh
```

#### 2.2. Configurer Poetry

Pour rendre Poetry accessible à tous les utilisateurs, assurez-vous que le chemin d'installation est
ajouté au fichier de configuration global.

**- Ajouter Poetry au PATH :**

```bash
sudo sh -c 'echo "export PATH=\$PATH:/root/.local/bin" >> /etc/profile'
```

#### 2.3. Configurer pipx

pipx installe les applications Python dans un répertoire utilisateur par défaut. Pour rendre ces
applications accessibles à tous les utilisateurs, vous pouvez configurer pipx pour utiliser un
répertoire partagé.

**- Configurer pipx pour un Répertoire Partagé :**

```bash
sudo mkdir -p /opt/pipx
sudo pipx install --system-site-packages
```

**- Ajouter le Répertoire Partagé au PATH :**

```bash
echo 'export PATH=$PATH:/opt/pipx/bin' | sudo tee -a /etc/profile.d/custom_paths.sh
```

### 3. Vérification

Pour vérifier que les outils sont correctement installés et accessibles, vous pouvez vous connecter
avec un utilisateur existant ou créer un nouvel utilisateur et exécuter les commandes suivantes :

```bash
python3 --version
pip --version
poetry --version
pipx --version
```

## Étape 3 : Configuration des Groupes d'Utilisateurs

Créez des groupes pour organiser les utilisateurs et attribuer des permissions spécifiques.
J'utilise trois groupes en plus de `sudo`, chacun ayant ses propres droits :

- **developer** : Accès complet aux répertoires de développement et aux outils nécessaires. Les
  membres de ce groupe peuvent lire, écrire et exécuter des fichiers dans les répertoires de
  développement.
- **user** : Accès en lecture seule aux répertoires de développement et accès complet à leurs
  répertoires personnels. Les utilisateurs peuvent lire les fichiers dans les répertoires partagés
  mais ne peuvent pas les modifier.
- **invite** : Accès limité, généralement en lecture seule aux répertoires spécifiques. Les invités
  peuvent accéder à certains fichiers ou répertoires partagés sans pouvoir les modifier.

### 1. Création des Groupes

```bash
sudo groupadd developer
sudo groupadd user
sudo groupadd invite
```

### 2. Création des Répertoires Partagés

Créez des répertoires partagés pour chaque groupe. Ces répertoires seront utilisés pour stocker les
fichiers et dossiers auxquels chaque groupe aura accès.

```bash
sudo mkdir -p /home/developer_shared
sudo mkdir -p /home/user_shared
sudo mkdir -p /home/invite_shared
```

### 3. Attribution des Répertoires aux Groupes

Changez le groupe propriétaire des répertoires pour correspondre aux groupes respectifs.

```bash
sudo chown -R :developer /home/developer_shared
sudo chown -R :user /home/user_shared
sudo chown -R :invite /home/invite_shared
```

### 4. Configuration des Permissions

Définissez les permissions pour chaque répertoire afin de contrôler l'accès des utilisateurs.

**- Pour le groupe `developer` :** Les membres du groupe developer doivent avoir un accès complet
(lecture, écriture, exécution) aux répertoires de développement.

```bash
sudo chmod -R 770 /home/developer_shared
```

**- Pour le groupe `user` :** Les membres du groupe user doivent avoir un accès en lecture seule aux
répertoires de développement et un accès complet à leurs répertoires personnels.

```bash
sudo chmod -R 750 /home/user_shared
```

**- Pour le groupe `invite` :** Les membres du groupe invite doivent avoir un accès en lecture seule
aux répertoires spécifiques.

```bash
sudo chmod -R 740 /home/invite_shared
```

### 5. Ajoutez des utilisateurs aux groupes :

```bash
sudo usermod -aG developer nom_utilisateur
sudo usermod -aG user nom_utilisateur
sudo usermod -aG invite nom_utilisateur
```

### 6. Création de Nouveaux Utilisateurs (si nécessaire)

Si vous avez besoin de créer de nouveaux utilisateurs, utilisez la commande suivante :

```bash
sudo adduser nouveau_utilisateur
```

Remplacez **nouveau_utilisateur** par le nom d'utilisateur souhaité. Cette commande créera un nouvel
utilisateur avec un répertoire personnel et les permissions par défaut. Vous pouvez ensuite ajouter
cet utilisateur aux groupes appropriés en utilisant les commandes usermod mentionnées ci-dessus.

### 7. Vérification

Pour vérifier que les permissions sont correctement configurées, vous pouvez utiliser la commande
`ls -l` pour lister les répertoires et voir les permissions attribuées.

```bash
ls -l /home
```

## Étape 4 : Sécurisation du Serveur

`Avertissement :` Avant de procéder aux modifications, assurez-vous de créer une snapshot de votre
serveur. Cela vous permettra de restaurer l'état actuel en cas de problème.

La sécurisation de votre serveur est une étape cruciale pour protéger vos données et prévenir les
accès non autorisés. Voici un guide détaillé pour sécuriser votre serveur SSH.

### 1. Changer le Port SSH

Par défaut, SSH fonctionne sur le port 22. Changer ce port peut aider à réduire les tentatives de
connexion automatisées.

#### 1.1 Modifiez le fichier de configuration SSH

Ouvrez le fichier de configuration SSH avec un éditeur de texte :

```bash
sudo nano /etc/ssh/sshd_config
```

#### 1.2 Trouver la Ligne du Port

Cherchez la ligne #Port 22 et remplacez-la par un autre numéro de port, par exemple Port 8456.

#### 1.3 Enregistrer et Quitter

Enregistrez les modifications et quittez l'éditeur (Ctrl+X, puis Y, puis Entrée).

#### 1.4 Redémarrer le Service SSH : Pour appliquer les modifications, redémarrez le service SSH

```bash
sudo systemctl restart ssh
```

**Explication :** Cette commande redémarre le service SSH pour prendre en compte le nouveau port.

### 2. Désactiver l'Authentification par Mot de Passe

Pour améliorer la sécurité, il est recommandé de désactiver l'authentification par mot de passe et
d'utiliser des clés SSH. **- Modifier le Fichier de Configuration SSH :** Assurez-vous que les
lignes suivantes sont présentes et décommentées dans `/etc/ssh/sshd_config` :

```bash
PasswordAuthentication no
PermitRootLogin no
```

**- Explication :** Ces lignes désactivent l'authentification par mot de passe et empêchent la
connexion root directe.

### 3. Configurer l'Authentification par Clé SSH

L'authentification par clé SSH est plus sécurisée que l'utilisation de mots de passe. Voici comment
la configurer :

#### 3.1 Générer une Paire de Clés SSH sur le Client

Sur votre machine locale (client), générez une paire de clés SSH si vous n'en avez pas déjà une :

```bash
ssh-keygen -t rsa -b 4096 -C "votre_email@example.com"
```

**- Explication :** Cette commande génère une paire de clés RSA avec une longueur de 4096 bits. Vous
serez invité à spécifier un emplacement pour sauvegarder la clé (par défaut `~/.ssh/id_rsa`) et à
entrer une phrase de passe (optionnelle).

#### 3.2 Copier la Clé Publique sur le Serveur :

Utilisez la commande `ssh-copy-id` pour copier votre clé publique sur le serveur :

```bash
ssh-copy-id user@votre_serveur
```

**- Explication** : Remplacez user par votre nom d'utilisateur sur le serveur et votre_serveur par
l'adresse IP ou le nom de domaine de votre serveur. Cette commande ajoute votre clé publique au
fichier `~/.ssh/authorized_keys` sur le serveur.

#### 3.3 Configurer le Serveur SSH

Assurez-vous que le fichier `/etc/ssh/sshd_config` contient les lignes suivantes pour permettre
l'authentification par clé :

```bash
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```

**- Explication :** Ces lignes permettent l'authentification par clé publique et spécifient
l'emplacement du fichier contenant les clés autorisées.

#### 3.4 Redémarrer le Service SSH

Après avoir modifié le fichier de configuration, redémarrez le service SSH pour appliquer les
changements :

```bash
sudo systemctl restart ssh
```

#### 3.5 Se Connecter au Serveur

Vous pouvez maintenant vous connecter à votre serveur sans mot de passe en utilisant votre clé SSH :

```bash
ssh user@votre_serveur
```

**- Explication :** Remplacez user par votre nom d'utilisateur et votre_serveur par l'adresse IP ou
le nom de domaine de votre serveur. Remarques

## Étapes 5 : Renforcement de la Sécurité

Dans cette étape, nous allons configurer un pare-feu pour contrôler le trafic réseau et installer
Fail2Ban pour protéger votre serveur contre les tentatives de connexion répétées. (brutforce)

#### 1. Configurer un Pare-feu avec ufw

ufw (Uncomplicated Firewall) est un outil simple et efficace pour gérer les règles de pare-feu sur
votre serveur.

**- Installer ufw :**

```bash
sudo apt install ufw -y
```

#### 2. Autoriser les Connexions sur des Ports Spécifiques :

Vous devez autoriser les connexions sur les ports nécessaires, comme le port SSH que vous avez
configuré précédemment. Remplacez <Port> par le numéro de port que vous utilisez pour SSH (par
exemple, 8456).

```bash
sudo ufw allow <Port>/tcp
```

**- Explication :** Cette commande autorise les connexions TCP entrantes sur le port choisi.

##### 2.1 Autoriser d'Autres Ports (si nécessaire) :

Si vous avez d'autres services (comme un serveur web) qui nécessitent des ports spécifiques,
autorisez-les également. Par exemple, pour un serveur web :

```bash
sudo ufw allow 80/tcp   # Pour HTTP
sudo ufw allow 443/tcp  # Pour HTTPS
```

**- Activer ufw :** sudo ufw enable Activez le pare-feu pour appliquer les règles :

```bash
sudo ufw enable
```

**- Explication :** Cette commande active ufw et applique les règles configurées.

##### 2.2 Vérifier l'État de ufw

Vous pouvez vérifier l'état et les règles actives de ufw avec la commande suivante :

```bash
sudo ufw status verbose
```

#### 3. Installer et Configurer Fail2Ban

Fail2Ban est un outil qui protège votre serveur contre les tentatives de connexion répétées en
bannissant les adresses IP qui échouent à plusieurs reprises.

**- Installer Fail2Ban :** Installez Fail2Ban en utilisant la commande suivante :

```bash
sudo apt install fail2ban -y
```

**- Configurer Fail2Ban :** Copiez le fichier de configuration par défaut pour créer un fichier de
configuration local que vous pouvez modifier sans risque :

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

**- Explication :** Cela crée une copie locale du fichier de configuration que vous pouvez modifier
sans affecter les mises à jour futures du paquet.

**- Modifier le Fichier de Configuration :** Ouvrez le fichier de configuration local pour ajuster
les paramètres selon vos besoins :

```bash
sudo nano /etc/fail2ban/jail.local
```

**- Paramètres à Vérifier :**

- _bantime :_ Durée pendant laquelle une adresse IP est bannie (par exemple, bantime = 10m pour 10
  minutes).
- _maxretry :_ Nombre de tentatives échouées avant qu'une adresse IP ne soit bannie (par exemple,
  maxretry = 5).
- _destemail :_ Adresse e-mail pour recevoir les notifications (facultatif).
- _action :_ Assurez-vous que l'action par défaut est définie pour utiliser ufw (par exemple, action
  = %(action_mwl)s).

#### 4. Redémarrer Fail2Ban :

Après avoir modifié le fichier de configuration, redémarrez Fail2Ban pour appliquer les changements
:

```bash
sudo systemctl restart fail2ban
```

#### 5. Vérifier l'État de Fail2Ban

Vous pouvez vérifier l'état de Fail2Ban avec la commande suivante :

```bash
sudo fail2ban-client status
```

## Étape 6 : (Optionnel) Scripts de Monitoring

### 1. Script de Monitoring Simple

Créez un script pour envoyer des données de monitoring aux utilisateurs.

```bash
sudo nano /usr/local/bin/monitoring.sh
```

**Contenu du script :**

```bash
#!/bin/bash

DATE=\$(date '+%Y-%m-%d %H:%M:%S')
USERS="raphael root"
LOG_FILE="/var/log/simple_monitor.log"

send_to_user_terminals() {
    local user=\$1
    local message=$2
    user_ttys=$(who | grep "^\$user" | awk '{print \$2}')
    if [ -z "\$user_ttys" ]; then
        return
    fi
    for tty in \$user_ttys; do
        echo "\$message" | write "\$user" "\$tty"
    done
}

log_message() {
    local message=\$1
    echo "[\$DATE] \$message" >> "\$LOG_FILE"
}

monitoring_data=\$(
    echo "--- Monitoring Data: \$DATE ---"
    echo -e "\n--- Espace disque (df -h) ---"
    df -h
    echo -e "\n--- Mémoire disponible (free -m) ---"
    free -m
    echo -e "\n--- Utilisation CPU et top processus (top 5) ---"
    top -b -n 1 | head -15
    echo -e "\n--- Charge système (uptime) ---"
    uptime
    echo -e "\n--- Nombre de connexions actives (who) ---"
    who
    echo -e "\n--- Nombre de processus actifs ---"
    ps -e --no-headers | wc -l
)

for user in \$USERS; do
    send_to_user_terminals "\$user" "\$monitoring_data"
done

log_message "\$monitoring_data"
```

**Rendre le script exécutable :**

```bash
sudo chmod +x /usr/local/bin/monitoring.sh
```

### 1. Automatisation avec Cron

Configurez une tâche cron pour exécuter le script de backup tous les heures.

```bash
sudo crontab -e
```

Ajoutez la ligne suivante :

```bash
0 * * * * /usr/local/bin/monitoring.sh
```

### 2. Script de Monitoring Interactif

Créez un script pour un monitoring interactif.

```bash
sudo nano /usr/local/bin/interactive_monitor.sh
```

Contenu du script :

```bash
#!/bin/bash

clear
echo "Appuyez sur 'q' pour quitter."

while true; do
    echo "---- Monitoring Data: \$(date) ----"
    echo -e "\n--- Espace disque (df -h) ---"
    df -h
    echo -e "\n--- Mémoire disponible (free -m) ---"
    free -m
    echo -e "\n--- Utilisation CPU et top processus (top 5) ---"
    top -b -n 1 | head -15
    echo -e "\n--- Charge système (uptime) ---"
    uptime
    echo -e "\n--- Nombre de connexions actives (who) ---"
    who
    echo -e "\n--- Nombre de processus actifs ---"
    ps -e --no-headers | wc -l
    echo -e "\nAppuyez sur 'q' pour quitter ou attendez la prochaine mise à jour..."

    read -t 5 -n 1 key
    if [[ \$key == "q" ]]; then
        echo -e "\nSortie du script. Au revoir !"
        break
    fi

    clear
done
```

Rendre le script exécutable :

```bash
sudo chmod +x /usr/local/bin/interactive_monitor.sh
```

## Étape 6 : Script de Backup

Créez un script pour automatiser les sauvegardes.

```bash
sudo nano /usr/local/bin/backup.sh
```

Contenu du script :

```bash
#!/bin/bash

BACKUP_DIR="/var/backups"
SOURCE_DIR="/etc/source"
MAX_BACKUPS=20
MIN_DISK_SPACE=5000000
BACKUP_PREFIX="backup_"
LOG_FILE="/var/log/custom_backup.log"

log_message() {
    local type="$1"
    shift
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] [$type] $*" >> "\$LOG_FILE"
    echo "[$type] $*"
}

usage() {
    echo "Usage: \$0 [-s source_dir] [-d backup_dir] [-m max_backups] [-f free_space]"
    echo "  -s : Répertoire source (défaut: \$SOURCE_DIR)"
    echo "  -d : Répertoire de destination des backups (défaut: \$BACKUP_DIR)"
    echo "  -m : Nombre maximum de backups à conserver (défaut: \$MAX_BACKUPS)"
    echo "  -f : Espace minimum requis en KB (défaut: \$MIN_DISK_SPACE)"
    exit 1
}

while getopts "s\:d\:m\:f\:h" opt; do
    case \$opt in
        s) SOURCE_DIR="\$OPTARG" ;;
        d) BACKUP_DIR="\$OPTARG" ;;
        m) MAX_BACKUPS="\$OPTARG" ;;
        f) MIN_DISK_SPACE="\$OPTARG" ;;
        h) usage ;;
        ?) usage ;;
    esac
done

mkdir -p "\$BACKUP_DIR"

check_disk_space() {
    local available_space
    available_space=\$(df -k "\$BACKUP_DIR" | tail -1 | awk '{print \$4}')
    if [ "\$available_space" -lt "$MIN_DISK_SPACE" ]; then
        log_message "ERROR" "Espace disque insuffisant. Disponible: ${available_space}KB"
        return 1
    fi
    return 0
}

rotate_backups() {
    local count
    count=\$(ls -1 "\$BACKUP_DIR"/"\$BACKUP_PREFIX"* 2>/dev/null | wc -l)

    while [ "\$count" -ge "$MAX_BACKUPS" ]; do
        oldest_backup=$(ls -1t "\$BACKUP_DIR"/"\$BACKUP_PREFIX"* | tail -1)
        log_message "INFO" "Suppression de l'ancien backup: \$oldest_backup"
        rm -rf "$oldest_backup"
        count=$((count - 1))
    done
}

create_backup() {
    local backup_date
    backup_date=$(date '+%Y%m%d_%H%M%S')
    local backup_name="${BACKUP_PREFIX}\${backup_date}"
    local backup_path="\$BACKUP_DIR/\$backup_name"

    log_message "INFO" "Début du backup: \$backup_name"

    if rsync -azh --stats --delete "\$SOURCE_DIR/" "\$backup_path/"; then
        log_message "INFO" "Backup terminé avec succès: $backup_name"
        echo "Backup créé le: $(date)" > "\$backup_path/backup_info.txt"
        echo "Source: \$SOURCE_DIR" >> "$backup_path/backup_info.txt"
        echo "Taille: $(du -sh "\$backup_path" | cut -f1)" >> "\$backup_path/backup_info.txt"
        return 0
    else
        log_message "ERROR" "Échec du backup: \$backup_name"
        rm -rf "\$backup_path"
        return 1
    fi
}

main() {
    if [ ! -d "\$SOURCE_DIR" ]; then
        log_message "ERROR" "Répertoire source non trouvé: \$SOURCE_DIR"
        exit 1
    fi

    if ! check_disk_space; then
        exit 1
    fi

    rotate_backups

    if create_backup; then
        log_message "INFO" "Processus de backup terminé avec succès"
    else
        log_message "ERROR" "Le processus de backup a échoué"
        exit 1
    fi
}

main
```

Rendre le script exécutable :

```bash
sudo chmod +x /usr/local/bin/backup.sh
```

### 1. Automatisation avec Cron

Configurez une tâche cron pour exécuter le script de backup tous les jours à minuit.

```bash
sudo crontab -e
```

Ajoutez la ligne suivante :

```bash
0 0 * * * /usr/local/bin/backup.sh
```

## Conclusion

Vous avez maintenant un serveur VPS configuré avec des outils de monitoring et de backup.
Assurez-vous de tester régulièrement vos scripts et de mettre à jour votre système pour maintenir la
sécurité. Ce tutoriel couvre les étapes de base pour configurer un serveur VPS. Vous pouvez
l'adapter en fonction de vos besoins spécifiques. Si vous avez des questions ou besoin d'aide
supplémentaire, n'hésitez pas à demander !

## Remarques

- **Personnalisation :** Adaptez les chemins, les noms d'utilisateurs, et les configurations en
  fonction de votre environnement.
- **Tests :** Assurez-vous de tester chaque étape dans un environnement de test avant de déployer en
  production.

Ce tutoriel devrait fournir une base solide pour configurer un serveur VPS. Si vous avez besoin
d'aide supplémentaire, faites-le moi savoir !
