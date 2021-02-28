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

### Dépendences

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

#### Netflix Eureka
![Logo Netflix Eureka](img/eureka.png)

#### Spring Cloud Gateway
![Logo Spring Cloud Gateway](img/spring-cloud.png "Spring Cloud Gateway")

#### Netflix Ribbon
![Logo Netflix Ribbon](img/ribbon.png "Netflix Ribbon")

### Documentation externe

  * [Docker](https://www.docker.com/)
  * [Microservices](https://microservices.io/)

### Sécurité

Si vous trouvez un bug, veuillez nous contacter s'il vous plait. :)

### Contributeurs

Les auteurs d'AMiSNO sont [FourmiPanda](https://github.com/FourmiPanda) &  [Xabibax](https://github.com/Xabibax)

### License

  [MIT](LICENSE)
