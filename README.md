# Active Directory Home Lab

## Présentation

Laboratoire Active Directory réalisé sous Windows Server 2022 afin d'apprendre l'administration d'un domaine Windows, la gestion des utilisateurs, des groupes, des GPO et des partages réseau.

---

## Architecture

```text
DC01 (Windows Server 2022)
│
├── Active Directory
├── DNS
├── SMB
└── GPO

CLIENT01 (Windows 10)
│
└── Poste joint au domaine

CLIENT02 (Windows 10)

Kali Linux 
│
└── BloodHound / PingCastle
```

---

## Domaine

```text
ADLAB.LOCAL
```

Organisation actuelle :

```text
ADLAB.LOCAL
│
├── OU Utilisateurs
│
├── Utilisateurs
│   ├── lm
│   ├── Yaska
│   └── Sisi
│
├── Groupes
│   ├── Accueil
│   │   └── lm
│   │
│   ├── BTP
│   │   ├── lm
│   │   └── Yaska
│   │
│   └── RH
│       └── Sisi
│
└── OU Postes
    └── Clients
        ├── CLIENT01
        └── CLIENT02
```

---

## Fonctionnalités mises en place

* Création du domaine Active Directory
* Gestion des utilisateurs
* Gestion des groupes de sécurité
* Création d'OU
* Déploiement de GPO
* Blocage de l'invite de commande (CMD)
* Création de partages SMB
* Gestion des permissions SMB et NTFS
* Contrôle d'accès par groupes
* Intégration des CLIENT01/02 au domaine
* Mappage automatique d'un lecteur réseau

---

## Progression

### Infrastructure

* [x] Installation de Windows Server 2022
* [x] Déploiement d'Active Directory
* [x] Configuration DNS
* [x] Création du domaine ADLAB.LOCAL

### Administration Active Directory

* [x] Création d'utilisateurs
* [x] Création de groupes
* [x] Création d'OU
* [x] Gestion des permissions

### GPO

* [x] Création de GPO
* [x] Blocage de CMD
* [x] Mappage de lecteur réseau

### Partages SMB

* [x] Création des partages
* [x] Permissions SMB
* [x] Permissions NTFS
* [x] Contrôle d'accès par groupe

### Clients

* [x] Déploiement de CLIENT01
* [x] Déploiement de CLIENT02
* [x] Jointure au domaine
* [x] Tests d'accès aux ressources

### Sécurité

* [x] Installation de Kali Linux
* [x] Analyse BloodHound
* [x] Audit

---

## Compétences travaillées

* Active Directory
* Gestion des utilisateurs et groupes
* Group Policy Objects (GPO)
* SMB
* NTFS
* Contrôle d'accès
* Administration Windows Server
* Gestion d'un domaine Windows

---

## Auteur

**Anass Moumen**
Étudiant ingénieur cybersécurité – EFREI Paris
