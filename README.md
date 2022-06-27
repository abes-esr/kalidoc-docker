# qualimarc-docker

Qualimarc est l'outil de diagnostic des notices du Sudoc.

Ce dépôt contient la configuration docker 🐳 pour déployer l'application qualimarc en local sur le poste d'un développeur, ou bien sur les serveurs de dev, test et prod. 

Il utilise les images docker liées et générées par les codes de source suivants :
- https://github.com/abes-esr/qualimarc-api : l'API de qualimarc
- https://github.com/abes-esr/qualimarc-front : l'interface utilisateur de qualimarc


## Prérequis

Disposer de :
- ``docker``
- ``docker-compose``

## Installation

```bash
# adaptez /opt/pod/ avec l'emplacement où vous souhaitez déployer l'application
cd /opt/pod/
git clone https://github.com/abes-esr/qualimarc-docker.git

cd /opt/pod/qualimarc-docker/
cp .env-dist .env
# personnaliser alors le contenu du .env
docker-compose up -d
```

Une base de données postgresql vide sera alors automatiquement initialisée. Ses données binaires seront placées dans le répertoire persistant suivante (attention le user unix de ce répertoire est celui du conteneur postgresql qui n'est pas le même que celui que vous utilisez pour installer l'application) :
```
/opt/pod/qualimarc-docker/volumes/qualimarc-db/pgdata/
```

## Démarrage et arrêt

```bash
# pour démarrer l'application
cd /opt/pod/qualimarc-docker/
docker-compose up -d
```

Remarque : retirer le ``-d`` pour voir passer les logs dans le terminal et utiliser alors CTRL+C pour stopper l'application

```bash
# pour stopper l'application
cd /opt/pod/qualimarc-docker/
docker-compose stop
```

## Configuration

Pour configurer l'application, vous devez créer et personnaliser un fichier ``/opt/pod/qualimarc-docker/.env``.

Les paramètres à placer dans le ``.env`` et des exemples de valeurs sont indiqués dans ce fichier [``.env-dist``](https://github.com/abes-esr/qualimarc-docker/blob/develop/.env-dist)

Pour configurer votre application, le mieux est de procéder ici :
```
cd /opt/pod/qualimarc-docker/
cp .env-dist .env
# ouvrez le fichier .env pour le personnaliser
```

## Supervision


```bash
# pour visualiser les logs de l'appli
cd /opt/pod/qualimarc-docker/
docker-compose logs -f --tail=100
```

Cela va afficher les 100 dernière lignes de logs générées par l'application et toutes les suivantes jusqu'au CTRL+C qui stoppera l'affichage temps réel des logs.

## Sauvegardes

Les éléments suivants sont à sauvegarder:
- ``/opt/pod/qualimarc-docker/.env`` : contient la configuration spécifique de notre déploiement
- ``/opt/pod/qualimarc-docker/volumes/qualimarc-db/dump/`` : contient les dumps quotidiens de la base de données postgresql de qualimarc

Il est possible de régler la fréquence et le moment de la génération des dump de postgresql en modifiant la variable : XXXXXTODOpréciser

## Développements

### Admin de postgresql
Pour avoir l'interface d'admin web de postgresql (basée sur [Adminer](https://www.adminer.org/)), lancez ceci :
```bash
cd /opt/pod/qualimarc-docker/
docker-compose -f docker-compose.yml -f docker-compose.adminer.yml up
```
Accédez ensuite à Adminer sur l'URL suivante : http://127.0.0.1:9081/

### Mise à jour de la dernière version

Pour récupérer et démarrer la dernière version de l'application :
```bash
docker-compose pull
docker-compose up
```
Le ``pull`` aura pour effet de télécharger l'éventuelle dernière images docker disponible pour la version glissante en cours (ex: ``develop-api`` ou ``main-api``). Sans le pull c'est la dernière image téléchargée qui sera utilisée.

## Architecture

<img src="https://docs.google.com/drawings/d/e/2PACX-1vQE2ImlIkdPWhz_mUH3LeGg-tAUtiqTzJXx4zP5UjHY75Cl9Snw2gj1M0ww6iYJf_kM-gBtLGdJcCgb/pub?w=1332&amp;h=645">

([lien](https://docs.google.com/drawings/d/1ki8ih3egccbf1SBsW4uDTAp7v5hCThQijG3ghki_d9c/edit) pour modifier le schéma)
