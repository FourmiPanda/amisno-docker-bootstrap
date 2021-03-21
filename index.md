## Projet AMiSNO

### Pré requis

Ce projet utilise [docker-compose](https://docs.docker.com/compose/) et des conteneurs disponible sur la [page docker hub](https://hub.docker.com/search?q=fourmipanda&type=image).

Avant l'installation, [télécharger et installer Docker](https://docs.docker.com/docker-for-windows/install/).
Le projet a été concue sur la version 19.03.12 de Docker.

### Installation

L'installation se fait via la commande
[`docker-compose up`](https://docs.docker.com/compose/reference/up/):
Placer vous dans le répertoires amisno-docker-bootstrap : 
> amisno-docker-bootstrap <br>
   |__ ...<br>
   |__ docker-compose.yml<br>
```bash
> docker-compose up
Creating network "amisno-docker-bootstrap_default" with the default driver
Creating amisno-docker-bootstrap_amisno-config-server_1 ... done
Creating amisno-docker-bootstrap_mongo_1                ... done
Creating amisno-docker-bootstrap_amisno-infra-eureka_1  ... done
Creating amisno-docker-bootstrap_amisno-cloud-gateway_1    ... done
Creating amisno-docker-bootstrap_amisno-catalog-service_1  ... done
Creating amisno-docker-bootstrap_amisno-order-service_1    ... done
Creating amisno-docker-bootstrap_amisno-catalog-service2_1 ... done
```

Si vous voulez modifiez un repo, il vous faudera recompiler la jar et re-créer une image Docker.
Exemple : 
```bash
> cd amisno-spring-gateway
amisno-spring-gateway > mvn clean install # Cette commande va créer un dossier target contenant la jar
amisno-spring-gateway > docker build -t <tagName> . # On créer ensuite l'image avec le tag <tagName>
amisno-spring-gateway > cd ../amisno-docker-bootstrap
amisno-docker-bootstrap > nano docker-compose.yml # Editer le fichier yml en mettant à jour le tag avec <tagName> 
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

Netflix Eureka est un annuaire qui fait le lien entre un service et une adresse IP. L'avantage d'Eureka est que les services n'ont pas tous besoin de connaitre l'IP de chacun à tout moment. L'information est centralisé sur le serveur Eureka. L'enrengistrement peut se faire via le SDK spring "Eureka-client" ou directement en attaquant l'API Rest.

<img src="https://miro.medium.com/max/1400/1*4F7jh6PNf5CDK6pN4H9UHw.jpeg" width="400" />

#### Spring Cloud Gateway
<img src="img/spring-cloud.png" width="200" />
Spring cloud gateway est le point d'entrée de l'application. Le gateway crée une table de routage à partir de l'annuaire Eureka.

<img src="https://user-images.githubusercontent.com/579465/42374683-31587ed2-8119-11e8-8b07-67ead64ac876.png" width="400" />

#### Netflix Ribbon
<img src="img/ribbon.png" width="200" />

Netflix Ribbon est un Load balancer coté client, c'est à dire que c'est à la charge de l'appelant de choisir le serveur à attaquer.
Ribbon est configurable depuis le fichier de configuration de l'application (application.yml ou bootstrap.yml).
Par exemple : 
```yml
<clientName>.ribbon.NFLoadBalancerRuleClassName = com.netflix.loadbalancer.WeightedResponseTimeRule # où clientName represente le nom que l'on renseigne dans le décorateur @RibbonClient(name= "<clientName>")
```

Dans l'exemple ci dessus, la classe com.netflix.loadbalancer.WeightedResponseTimeRule est une classe mis à disposition par Ribbon pour faire du load balancing, celle ci est customisable en implémentatant l'interface IRule. Plus d'information [ici](https://cloud.spring.io/spring-cloud-netflix/multi/multi_spring-cloud-ribbon.html).

La stratégie de load balancing par défaut et le [Round-Robin](https://fr.wikipedia.org/wiki/Round-robin_(informatique))

#### Resilience4j

<img src="img/resilience4j.png" width="200" />

Resilience4j est une libraire de Fault Tolerance pour java8.

Il permet d'ajouter un comportement aux erreurs renvoyé par l'applicaiton.

Le module principale est le circuit breaker (représenté par son logo), qui simule le comportement d'un circuit électrique avec 3 état :
* Fermé le courant passe, les fonctions peuvent être appellées
* Ouvert le courant ne passe pas, l'appel aux fonctions est refusé d'entré afin de gagner du temps sur le renvoie d'erreur
* Semi-ouvert le courant passe, état de transition où l'appel aux fonctions est autorisé selon certaines conditions avant de basculé dans l'état Ouvert si une erreur survient ou Fermé si aucun erreur n'apparaît


Dans le cadre d'un projet spring boot 2, il suffit d'jouter ces dépendances au fichier de builde (build.gradle dans un projet gradle) :

```gradle
dependencies {
    ...

    implementation group: 'org.springframework.boot', name: 'spring-boot-starter-actuator'
    implementation group: 'org.springframework.boot', name: 'spring-boot-starter-aop'

    implementation "io.github.resilience4j:resilience4j-spring-boot2:${resilience4jVersion}"
    implementation("io.github.resilience4j:resilience4j-reactor:${resilience4jVersion}")
    
    implementation("io.github.resilience4j:resilience4j-all:${resilience4jVersion}") // Optional, only required when you want to use the Decorators class
    /* To add a specific core-module
    implementation "io.github.resilience4j:resilience4j-circuitbreaker:${resilience4jVersion}"
    implementation "io.github.resilience4j:resilience4j-ratelimiter:${resilience4jVersion}"
    implementation "io.github.resilience4j:resilience4j-retry:${resilience4jVersion}"
    implementation "io.github.resilience4j:resilience4j-bulkhead:${resilience4jVersion}"
    implementation "io.github.resilience4j:resilience4j-cache:${resilience4jVersion}"
    implementation "io.github.resilience4j:resilience4j-timelimiter:${resilience4jVersion}"
    /**/
 }   
 ```

A partir de là, il est possible de configurer les modules dans le fichier de configuration de l'application (application.yml) :

```properties
resilience4j.circuitbreaker:
  configs:
    default:
      registerHealthIndicator: true
      minimumNumberOfCalls: 3
      permittedNumberOfCallsInHalfOpenState: 1
      automaticTransitionFromOpenToHalfOpenEnabled: true
      waitDurationInOpenState: 10s
      failureRateThreshold: 20
      eventConsumerBufferSize: 1
      recordExceptions:
        - org.springframework.web.client.HttpServerErrorException
        - java.util.concurrent.TimeoutException
        - java.io.IOException
  instances:
    productCatalog:
      baseConfig: default
 ```
 
 Enfin il faut décorer les fonctions souhaitées avec un module et les associer avec une instance :
 ```java
    @CircuitBreaker(name = productCatalog)
    @Bulkhead(name = productCatalog)
    @RateLimiter(name = productCatalog)
    private Phone findOne(Long Id) {
        ...
    }
 ```

### Documentation externe

  * [Docker](https://www.docker.com/)
  * [Microservices](https://microservices.io/)
  * [Configuration Ribbon](https://cloud.spring.io/spring-cloud-netflix/multi/multi_spring-cloud-ribbon.html)
  * [Documentation resilience4j](https://resilience4j.readme.io/docs)
  * [Désendettement projet Netflix](https://javaetmoi.com/2019/11/desendettement-de-spring-cloud-netflix/)

### Sécurité

Si vous trouvez un bug, veuillez nous contacter s'il vous plait. :)

### Contributeurs

Les auteurs d'AMiSNO sont [FourmiPanda](https://github.com/FourmiPanda) &  [Xabibax](https://github.com/Xabibax)

### License

  [MIT](LICENSE)
