# SecureSync Réseau - Bloc 2

Projet d'étude en réseaux de niveau Bloc 2, centré sur la gestion d'une architecture distribuée composée d'un client, de conteneurs et d'un dispatcher. L'objectif est de simuler un système de partage de fichiers et de communication réseau avec gestion de sécurité, authentification et supervision des nœuds.

## Présentation

Ce projet met en place une infrastructure de type client/serveur répartie :

- un client graphique Python pour interagir avec le système,
- un ou plusieurs conteneurs Java qui exposent des services réseau,
- un dispatcher Java qui centralise les échanges, contrôle les conteneurs et gère les demandes des utilisateurs.

Le système permet de réaliser des opérations réseau de base comme l'enregistrement d'un utilisateur, l'authentification, l'upload, le téléchargement et la suppression de fichiers, en plus d'une surveillance des conteneurs via des messages de vie (heartbeat/ALIVE).

## Architecture du projet

```text
SecureSync Réseau
├── b2-reseau-client/        # Client Python + GUI + 2FA
├── b2-reseau-container/     # Conteneurs réseau Java
├── b2-reseau-dispatcher/    # Serveur central Java
└── README.md                # Documentation globale du projet
```

## Fonctionnalités principales

- Communication entre client, dispatcher et conteneurs
- Gestion des demandes de fichiers (GET, POST, DELETE)
- Vérification de disponibilité des conteneurs par messages ALIVE
- Authentification utilisateur avec 2FA côté client
- Système de sécurité et de configuration TLS/SSL
- Gestion d'un protocole réseau personnalisé entre les services
- Suivi de logs et supervision du fonctionnement du système

## Stack technique

- Python 3 pour le client
- Java 21 + Gradle pour les services dispatcher et conteneurs
- Protocoles réseau TCP / multicast
- Stockage JSON pour les utilisateurs et les données de configuration
- QR Code + otp pour la partie authentification à deux facteurs

## Composants du projet

### 1. Client
Le client est développé en Python et fournit une interface utilisateur pour :

- se connecter au dispatcher,
- créer un compte,
- récupérer un code 2FA via Google Authenticator,
- envoyer et récupérer des fichiers,
- interagir avec le système via le protocole réseau défini.

Dossier : `b2-reseau-client`

### 2. Conteneur
Le conteneur est une application Java qui accueille les services réseau et traite les requêtes transmises par le dispatcher. Il assure en particulier :

- l'écoute des demandes,
- le traitement des messages de protocole,
- la gestion des fichiers locaux,
- l'envoi de signaux de présence au dispatcher.

Dossier : `b2-reseau-container`

### 3. Dispatcher
Le dispatcher est le point central du système. Il coordonne les interactions, surveille les conteneurs actifs et oriente les requêtes des clients vers les ressources disponibles. Il peut être vu comme le cœur de l'architecture réseau du projet.

Dossier : `b2-reseau-dispatcher`

## Déploiement et exécution

### Prérequis

- Java 21
- Python 3
- Gradle (ou wrapper fourni par les projets Java)
- Un environnement réseau fonctionnel (IP, port, interface réseau)
- Application Google Authenticator pour la gestion 2FA côté client

### 1. Lancer le dispatcher

```bash
cd b2-reseau-dispatcher
./gradlew run --args="<SWIN_KEYSTORE_PASSWORD> <NETWORK_INTERFACE>"
```

### 2. Lancer le conteneur

```bash
cd b2-reseau-container
./gradlew run --args="<MY_IP_ADDRESS> <PORT> <ID_CONTAINER> <NETWORK_INTERFACE>"
```

### 3. Lancer le client

```bash
cd b2-reseau-client
python .\main.py <IP_DISPATCHER> <PORT> <USER> <PASSWORD> <tls|empty>
```

Si nécessaire, vérifiez la configuration de l'hôte réseau et la présence du certificat `ca.crt`.

## Structure du dépôt

```text
b2-reseau-client/
├── controller/
├── gui/
├── model/
├── net/
├── services/
├── main.py
├── README.md
├── users.json
└── ca.crt

b2-reseau-container/
├── App/
├── Domains/
├── Infrastructures/
├── README.md
├── build.gradle
├── settings.gradle
└── gradlew

b2-reseau-dispatcher/
├── App/
├── Domains/
├── Infrastructure/
├── README.md
├── build.gradle
├── settings.gradle
└── gradlew
```

## Objectif pédagogique

Ce projet a été conçu dans un contexte d'apprentissage des réseaux informatiques et de l'architecture logicielle distribuée. Il permet de mettre en pratique :

- les principes de communication réseau,
- la gestion des sockets et des protocoles,
- la supervision d'un système réparti,
- la sécurisation des échanges,
- la structuration logicielle par modules et couches.

## Conclusion

SecureSync Réseau est un projet d'étude de réseaux en Bloc 2 qui illustre une architecture modulaire de système distribué avec client, conteneurs et dispatcher. Il s'inscrit dans une logique pédagogique de découverte des mécanismes réseau, de la coordination de services et de la sécurisation des échanges.

---

Projet réalisé dans le cadre du Bloc 2 - Réseaux.
