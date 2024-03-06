# Workflow GitHub Actions for Docker Image Build and Deployment

Ce fichier représente un workflow GitHub Actions qui se déclenche automatiquement lorsqu'un push est effectué sur la branche principale (main). Le workflow réalise plusieurs étapes liées à la construction et au déploiement d'une image Docker à l'aide de Docker Compose.

## Étapes du Workflow

### 1. Checkout du Code

Utilisation de l'action checkout pour récupérer le code source depuis le référentiel.

### 2. Construction des Images Docker

Exécution de la commande `docker compose build` pour construire les images Docker en utilisant Docker Compose.

### 3. Connexion à Docker Hub

Utilisation des secrets GitHub (DOCKER_USER et DOCKER_TOKEN) pour se connecter à Docker Hub à l'aide de la commande `docker login`.

### 4. Étiquetage de l'Image Docker

Étiquetage de l'image Docker construite avec la version 1.0 du tag `dockervolume-backend` pour la version 2.0 du tag `lamineoziljr/dockervolume-backend`.

### 5. Pousser l'Image Docker vers Docker Hub

Utilisation de la commande `docker push` pour pousser l'image Docker étiquetée (`lamineoziljr/dockervolume-backend:2.0`) vers Docker Hub.

## Configuration du Service Docker Compose

Nous avons configuré un service pour Docker Compose, un outil de définition et de gestion d'applications. Voici un aperçu de la configuration :

```yaml
app-dockervolume-backend:
  image: dockervolume-backend:1.0
  container_name: container-dockervolume-backend
  ports:
    - "8080:8080"
  restart: unless-stopped
  build:
    context: .
    dockerfile: Dockerfile
  environment:
    directoryDatas: /app/data/
  volumes:
    - ./datas:/app/data
```

## Définition des Étapes (Stages):
Les différentes étapes du pipeline sont définies dans la section stages. Chaque étape représente une phase spécifique du processus d'intégration et de déploiement.

## Étape de Build:

Cette étape est destinée à la construction du projet. La commande bat est utilisée pour exécuter des scripts batch sous Windows. 
La commande Maven spécifiée (C:/apache-maven-3.9.5/bin/mvn clean package) nettoie le projet et construit les artefacts de l'application.

## Étape de Test:

Cette étape est destinée à l'exécution des tests du projet. 
La commande Maven spécifiée (C:/apache-maven-3.9.5/bin/mvn test) exécute les tests unitaires définis dans le projet.

## Étape de Déploiement:

Cette étape est destinée au déploiement de l'application à l'aide de Docker Compose. 
La commande bat spécifiée (C:/Docker/resources/bin/docker-compose up -d --build) construit et lance les conteneurs Docker définis dans le fichier docker-compose.yml.

## Étape de Push vers Docker Hub:

Cette étape est destinée à la publication de l'image Docker sur Docker Hub. 
Les identifiants Docker Hub (nom d'utilisateur et mot de passe) sont récupérés de manière sécurisée depuis les credentials stockées dans Jenkins. 
La commande bat est utilisée pour exécuter des scripts Docker, notamment pour se connecter à Docker Hub (docker login), tagger l'image (docker tag), et 
pousser l'image taguée vers Docker Hub (docker push). Le tag de l'image est basé sur la version du build (v$BUILD_NUMBER), assurant une traçabilité de l'image publiée.

##
## CI/CD Pipeline for Spring Boot Project

Notre fichier décrit le pipeline d'intégration continue et de déploiement continu (CI/CD) pour notre projet Spring Boot, notamment 
les étapes de packaging, de construction d'une image Docker, et de déploiement manuel de cette image.

## Stages

Définit deux étapes du pipeline : "packaging" et "build_docker_image".

## Default Image

Spécifie l'image Docker par défaut utilisée dans le pipeline. Ici c'est une image Maven avec Java 17.

```yaml
default:
  image: maven:3.8.4-openjdk-17
```

L'étape "build_docker_image" utilise l'image Docker:latest et construit une image Docker étiquetée avec une version spécifiée. Cette étape utilise un service Docker-in-Docker (docker:dind) pour exécuter les commandes Docker nécessaires.

Les artefacts résultants (l'image Docker) sont sauvegardés, et cette étape est configurée pour être déclenchée manuellement avec when: manual.
