# Docker Talk

## Docker Compose for local

Create docker instances:

```
docker-compose up
```

Let's take a look around:

```
docker ps
docker-compose ps
```

Some playing around:

```
docker-compose scale worker=2
docker-compose exec web /bin/bash
```

## Creating the cluster

Spin up three digital ocean machines:

```
. .token
IMAGE=`doctl -t $TOKEN compute image list | grep docker-16.04 | cut -f1`
doctl -t $TOKEN compute droplet create swarm0{1,2,3} \
  --image $IMAGE --region lon1 --size 2gb --ssh-keys 6915365
```

Get their IPs:

```
doctl -t $TOKEN compute droplet list | grep swarm
```

On the master:

```
ufw disable
docker swarm init --advertise-addr=x.x.x.x  # Public IP
```

On the workers:

```
ufw disable
IP=x.x.x.x  # public IP
TOKEN=token
docker swarm join --token $TOKEN $IP:2377
```

On the master again:

```
docker node ls
```

Stack:

```
docker stack deploy --compose-file docker-compose.yml lint-review
```

Destroy them:

```
doctl -t $TOKEN compute droplet delete swarm0{1,2,3}
```
