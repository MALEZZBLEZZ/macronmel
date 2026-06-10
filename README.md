# Macronmel - Projet Nginx Docker CI/CD

## Présentation

Ce projet a été réalisé dans le cadre d'un exercice de découverte de la conteneurisation et de l'intégration continue.

L'objectif est de déployer un site web statique à l'aide de Nginx dans un conteneur Docker et de mettre en place une pipeline GitHub Actions permettant de valider automatiquement la configuration.

## Technologies utilisées

* Docker
* Nginx
* Git
* GitHub
* GitHub Actions

## Structure du projet

```text
.
├── .github/
│   └── workflows/
│       └── build.yml
├── backup/
│   └── nginx.conf
├── Dockerfile
├── nginx.conf
├── index.html
├── background.png
└── README.md
```

## Construction de l'image Docker

Construire l'image :

```bash
docker build -t macron-nginx .
```

## Lancement du conteneur

```bash
docker run -d \
  --name macron-nginx \
  -p 8070:80 \
  macron-nginx
```

Le site est alors accessible à l'adresse :

```text
http://localhost:8070
```

ou

```text
http://IP_DU_SERVEUR:8070
```

## Configuration Nginx

Le serveur Nginx utilise le fichier :

```text
nginx.conf
```

Cette configuration permet :

* l'écoute sur le port 80 ;
* la publication du site statique ;
* la définition de la page d'accueil `index.html`.

## Pipeline GitHub Actions

Le workflow est déclenché manuellement grâce à :

```yaml
workflow_dispatch
```

La pipeline réalise les opérations suivantes :

1. Récupération du code source.
2. Vérification de la présence des fichiers nécessaires.
3. Construction de l'image Docker.
4. Validation de la configuration Nginx avec :

```bash
nginx -t
```

## Test d'échec de la pipeline

Afin de valider le bon fonctionnement de l'intégration continue, une erreur volontaire a été introduite dans la configuration Nginx.

L'étape :

```bash
nginx -t
```

détecte alors l'erreur et provoque l'échec de la pipeline.

Une fois la correction effectuée, le workflow repasse avec succès.

## Sauvegarde

Une copie de la configuration Nginx est conservée dans :

```text
backup/nginx.conf
```

afin de pouvoir restaurer rapidement une version fonctionnelle.

## Auteur

Mathieu Pigeon

Projet pédagogique réalisé dans le cadre d'une initiation à Docker, Nginx et GitHub Actions.
