# qualimarc-docker

Configuration docker pour déployer l'application qualimarc qui se compose de plusieurs modules :
- https://github.com/abes-esr/qualimarc-back 
- https://github.com/abes-esr/qualimarc-front

Ce dépôt permet de déployer l'application qualimarc en local, en dev, en test et en prod.

## Prérequis

Disposer de :
- ``docker``
- ``docker-compose``

## Installation

```bash
git clone https://github.com/abes-esr/qualimarc-docker.git
cd qualimarc-docker/
cp .env-dist .env
# personnaliser le contenu du .env
docker-compose up -d
```

Une base de données postgresql vide sera alors automatiquement initialisée. Ses données binaires seront placée dans le répertoire persistant suivante (attention le user unix de ce répertoire est celui du conteneur postgresql qui n'est pas le même que celui que vous utilisez pour installer l'application) :
```
qualimarc-docker/volumes/qualimarc-db/pgdata/
```

## Démarrage et arrêt

```bash
# pour démarrer l'application
cd qualimarc-docker/
docker-compose up -d
```

Remarque : retirer le ``-d`` pour voir passer les logs dans le terminal et utiliser alors CTRL+C pour stopper l'application

```bash
# pour stopper l'application
cd qualimarc-docker/
docker-compose stop
```

## Supervision


```bash
# pour visualiser les logs de l'appli
cd qualimarc-docker/
docker-compose logs -f --tail=100
```

Cela va afficher les 100 dernière lignes de logs générées par l'application et toutes les suivantes jusqu'au CTRL+C qui stoppera l'affichage temps réel des logs.

## Sauvegardes

Les éléments suivants sont à sauvegarder:
- ``qualimarc-docker/.env`` : contient la configuration spécifique de notre déploiement
- ``qualimarc-docker/volumes/qualimarc-db/dump/`` : contient les dumps quotidiens de la base de données postgresql de qualimarc

Il est possible de régler la fréquence et le moment de la génération des dump de postgresql en modifiant la variable : XXXXXTODOpréciser

## Développements

### Admin de postgresql
Pour avoir l'interface d'admin web de postgresql (basée sur [Adminer](https://www.adminer.org/)), lancez ceci :
```bash
cd qualimarc-docker/
docker-compose -f docker-compose.yml -f docker-compose.adminer.yml up
```
Accédez ensuite à Adminer sur l'URL suivante : http://127.0.0.1:9081/

### Mise à jour de la dernière version

Pour récupérer et démarrer la dernière version de l'application :
```bash
docker-compose pull
docker-compose up
```
Le ``pull`` aura pour effet de télécharger l'éventuelle dernière images docker disponible pour la version glissante en cours (ex: ``develop-web`` ou ``main-web``). Sans le pull c'est la dernière image téléchargée qui sera utilisée.

## Architecture

<img src="https://docs.google.com/drawings/d/e/2PACX-1vQE2ImlIkdPWhz_mUH3LeGg-tAUtiqTzJXx4zP5UjHY75Cl9Snw2gj1M0ww6iYJf_kM-gBtLGdJcCgb/pub?w=1332&amp;h=645">

([lien](https://docs.google.com/drawings/d/1ki8ih3egccbf1SBsW4uDTAp7v5hCThQijG3ghki_d9c/edit) pour modifier le schéma)
