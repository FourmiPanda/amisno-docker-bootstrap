## Projet AMiSNO

### Pré requis

Ce projet utilise [docker-compose](https://docs.docker.com/compose/) et des conteneurs disponible sur la [page docker hub](https://hub.docker.com/search?q=fourmipanda&type=image).

Avant l'installation, [télécharger et installer Docker](https://docs.docker.com/docker-for-windows/install/).
Le projet a été concue sur la version 19.03.12 de Docker.

### Installation

Installation is done using the
[`docker-compose up` command](https://docs.docker.com/compose/reference/up/):

```bash
$ docker-compose up
```

### Dépendances

La liste des dépendances : 

- https://github.com/FourmiPanda/amisno-docker-bootstrap
- https://github.com/FourmiPanda/amisno-config-server
- https://github.com/FourmiPanda/amisno-config-repo
- https://github.com/FourmiPanda/amisno-app-front
- https://github.com/FourmiPanda/amisno-infra-eureka
- https://github.com/FourmiPanda/amisno-spring-gateway
- https://github.com/Xabibax/amisno-order-service-v3
- https://github.com/Xabibax/amisno-product-catalog

### Information sur les *Edge Services*

#### Spring Cloud Configuration
<img src="img/spring-cloud.png" width="200" />

Spring Cloud Configuration permet de centraliser les fichiers de configurations utilisé par les microservices. Les fichiers de configuration
sont stocké dans un [répertoire](https://github.com/FourmiPanda/amisno-config-repo) git. Ils sont utilisés par les microservices au runtime.

#### Netflix Eureka
<img src="img/eureka.png" width="200" />

Netflix Eureka est un annuaire qui fait le lien entre un service et une adresse IP. Par exemple, si un service A veut faire appel à un service
B, le service B devra au préallable s'enrengistrer sur Eureka.

<img src="https://miro.medium.com/max/1400/1*4F7jh6PNf5CDK6pN4H9UHw.jpeg" width="400" />


#### Spring Cloud Gateway
<img src="img/spring-cloud.png" width="200" />

Spring cloud gateway est le point d'entrée de l'application. Le gateway crée une table de routage à partir de l'annuaire Eureka.

#### Netflix Ribbon
<img src="img/ribbon.png" width="200" />

Netflix Ribbon est un Load balancer coté client, c'est à dire que c'est à la charge de l'appelant de choisir le serveur à attaquer.
Ribbon est configurable depuis le fichier de configuration de l'application (application.yml ou bootstrap.yml).
Par exemple : 
```yml
<clientName>.ribbon.NFLoadBalancerRuleClassName = com.netflix.loadbalancer.WeightedResponseTimeRule # où clientName represente le nom que l'on renseigne dans le décorateur @RibbonClient(name= "<clientName>")
```
La stratégie de load balancing par défaut et le [Round-Robin](https://fr.wikipedia.org/wiki/Round-robin_(informatique))


### Documentation externe

  * [Docker](https://www.docker.com/)
  * [Microservices](https://microservices.io/)
  * [Configuration Ribbon](https://cloud.spring.io/spring-cloud-netflix/multi/multi_spring-cloud-ribbon.html)
  * [Désendettement projet Netflix](https://javaetmoi.com/2019/11/desendettement-de-spring-cloud-netflix/)

### Sécurité

Si vous trouvez un bug, veuillez nous contacter s'il vous plait. :)

### Contributeurs

Les auteurs d'AMiSNO sont [FourmiPanda](https://github.com/FourmiPanda) &  [Xabibax](https://github.com/Xabibax)

### License

  [MIT](LICENSE)
