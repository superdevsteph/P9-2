# MyERP

## Organisation du répertoire

*   `doc` : documentation
*   `docker` : répertoire relatifs aux conteneurs _docker_ utiles pour le projet
    *   `dev` : environnement de développement
*   `src` : code source de l'application


## Environnement de développement

Les composants nécessaires lors du développement sont disponibles via des conteneurs _docker_.
L'environnement de développement est assemblé grâce à _docker-compose_
(cf docker/dev/docker-compose.yml).

Il comporte :

*   une base de données _PostgreSQL_ contenant un jeu de données de démo (`postgresql://127.0.0.1:9032/db_myerp`)



### Lancement

    cd docker/dev
    docker-compose up


### Arrêt

    cd docker/dev
    docker-compose stop


### Remise à zero

    cd docker/dev
    docker-compose stop
    docker-compose rm -v
    docker-compose up

### SONARQUBE
    
    ÉTAPE 1 : INSTALLEZ DOCKER
Pour MacOs : https://www.docker.com/products/docker-desktopPour Windows : https://store.docker.com/editions/community/docker-ce-desktop-windowsPour Ubuntu & Debian, tapez cette commande dans votre terminal :
curl https://releases.rancher.com/install-docker/18.03

ÉTAPE 2 : INSTALLEZ SONARQUBE COMMUNITY ET LANCEZ-LE
Dans votre terminal, tapez les 2 commandes suivantes :
docker pull sonarqube
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
Vous pouvez maintenant accéder au serveur SonarQube et consulter l’interface sur http://localhost:9000. (ça peut durer quelques secondes avant de pouvoir accéder à l'interface...)
Vous pouvez vous connecter en utilisant le couple admin/admin.

ÉTAPE 3 : CONFIGUREZ L'ANALYSE DE VOTRE PROJET
A la racine de votre projet, créez un fichier sonar-project.properties et copiez-y le contenu suivant :

sonar.projectKey=votre_clé_unique
sonar.projectName=MonProjet
sonar.projectVersion=1.0
sonar.sources=src
sonar.exclusions=vendor/**
sonar.sourceEncoding=UTF-8

Étape 4 : lancez le scanner Dans votre terminal, dirigez-vous à la racine de votre projet et tapez la commande suivante :
docker run -ti -v votre_chemin_projet/:/root/src --link sonarqube newtmitch/sonar-scanner
Remarque : pensez à renseigner votre chemin après le flag -v. De plus, il est possible que cette commande ne fonctionne pas sous Windows. J’ai (seulement) testé sur Ubuntu et MacOs X.

ÉTAPE 5 : CONSULTEZ LE RAPPORT GÉNÉRÉ
Lorsque vous voyez “ANALYSE RÉUSSIE”, vous pouvez retourner sur l’interface de SonarQube (http://localhost:9000). Vous pouvez constater que votre projet a bien été analysé. Cliquez dessus, et admirez le résultat

L'écran "Measures" vous propose une visualisation d’ensemble de votre code analysé. Plus la couleur d’une bulle est proche de rouge, plus les “bogues” sont graves. La taille de la bulle indique le volume du bogue, 
alors que la position verticale de chaque bulle indique le temps estimé pour traiter les bogues. Magique, non ? Bon, en ce qui me concerne, cet écran ne m’indique rien de bien rassurant pour mon code.. Et vous ?
CONCLUSION
Sachez qu’il existe d’autres moyens d’utiliser SonarQube avec et sans Docker. Personnellement, je vois un réel intérêt d’utiliser SonarQube dans un processus d’intégration continue. Installez une instance de SonarQube 
sur l’un de vos serveurs et installez le scanner sur chaque poste de dev. Puis, en utilisant GitLab (on en reparlera plus en détails prochainement), vous pourrez vérifier que, lors de chaque commit, votre code n’est 
pas “trop moche” et qu’il peut être déployé sans risques... SonarQube vous propose énormément de fonctionnalités.# P9
