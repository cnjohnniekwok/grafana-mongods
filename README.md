# mongodb-grafana-docker-image ([Docker Hub](https://hub.docker.com/repository/docker/cnjohnniekwok/grafana-mongods))
Use specific granfana version (current build image is 7.4.2) with [JamesOsgood / mongodb-grafana](https://github.com/JamesOsgood/mongodb-grafana) plugin installed (Run proxy separately)

This image will not start proxy server inside the grafana-mongods instance. A separate mongo-grafana-proxy-docker-image ([DockerHub](https://hub.docker.com/repository/docker/cnjohnniekwok/mongo-grafana_proxy) | [GitHub](https://github.com/cnjohnniekwok/mongodb-grafana-proxy-docker-image)) can be run along side within the same network. If you want to run mongo proxy server within the same container you can do so as well, please follow [JamesOsgood](https://github.com/JamesOsgood/mongodb-grafana#install-and-start-the-mongodb-proxy-server) start mongo proxy guide.

Run grafana server ver. `7.4.2` with mongodb datasource pre-install container using `docker run` :

Create a bridge network
```
docker network create -d bridge mongodb-grafana
```

Start grafana container in the network created from above:
```
docker run -d -p 3000:3000 --name="mongodb-grafana7.4.2" --network mongodb-grafana cnjohnniekwok/mongodb-grafana-docker-image:7.4.2
```

Start side mongo_proxy container:
```
docker run -d --name="mongodb-grafana-proxy" --network mongodb-grafana cnjohnniekwok/mongo-grafana-proxy-docker-image
```


***Grafana MongoDB datasource settings:***

Grafana Configuration -> Data Sources tab -> MongoDB

Uder HTTP Setting:

URL: `mongodb-grafana-proxy:3333` (Mongo proxy listens to port 3333)

Access: `Server`

Under MongoDB Details:

MongoDB URL: `mongodb://<dbUser>:<dbPassword>@mongodb:27017`

 -OR-

MongoDB URI: `mongodb+srv://<dbUser>:<dbPassword>@<dbCluster>.<dbHost.net>/`

MongoDB Database: **LEAVE THIS BLANK**
