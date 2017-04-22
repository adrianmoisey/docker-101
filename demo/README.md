# Docker Talk

## Creating the cluster

Spin up three digital ocean machines:

```
. .token
IMAGE=23755729
doctl -t $TOKEN compute image list | grep Docker
doctl -t $TOKEN compute droplet create swarm01 \
  --image $IMAGE --region lon1 --size 2gb --ssh-keys 6915365
doctl -t $TOKEN compute droplet create swarm02 \
  --image $IMAGE --region lon1 --size 2gb --ssh-keys 6915365
doctl -t $TOKEN compute droplet create swarm03 \
  --image $IMAGE --region lon1 --size 2gb --ssh-keys 6915365
```

Get their IPs:

```
doctl -t $TOKEN compute droplet list | grep swarm
```

On the master:

```
ufw disable
docker swarm init --advertise-addr=x.x.x.x
```

On the workers:

```
ufw disable
IP=x.x.x.x
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
doctl -t $TOKEN compute droplet delete swarm01
doctl -t $TOKEN compute droplet delete swarm02
doctl -t $TOKEN compute droplet delete swarm03
```
