# grafana-x.x.x-mongods
Use specific granfana version (current build image is 7.4.2) with [JamesOsgood / mongodb-grafana](https://github.com/JamesOsgood/mongodb-grafana) plugin installed (Run proxy separately)

This image will not start proxy server inside the grafana-x.x.x-mongods instance. A separate [mongo-grafana_proxy](https://hub.docker.com/repository/docker/cnjohnniekwok/mongo-grafana_proxy) container can be run along side within the same network. If you want to run mongo proxy server within the same container you can do so as well, please follow [JamesOsgood](https://github.com/JamesOsgood/mongodb-grafana#install-and-start-the-mongodb-proxy-server) start mongo proxy guide.

Run grafana server ver. `7.4.2` with mongodb datasource pre-install container using `docker run` :

Create a bridge network
```
docker network create -d bridge <my-bridge-network>
```

Start grafana container in the network created from above:
```
docker run -d -p 3000:3000 --name="grafana-7.4.2-mongods" --network <my-bridge-network> cnjohnniekwok/grafana-x.x.x-mongods
```

Start side mongo_proxy container:
```
docker run -d p 80001:80 --name="mongo-grafana_proxy" --network <my-bridge-network> cnjohnniekwok/mongo-grafana_proxy
```


Grafana Settings:
Grafana settings -> Data Source -> Add Data Source -> MongoDB DataSource:

Uder HTTP Setting:

URL: `mongo-grafana_proxy:3333`

Access: `Server`

Under MongoDB Details:

MongoDB URL: `mongodb://<dbUser>:<dbPassword>@mongodb:27017`

MongoDB Database: `<dbName>`
