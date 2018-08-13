To create a Swarm cluster: 
Use the shell script (setup-swarm.sh) to setup a cluster with 3 nodes (1 Manager & 2 Workers)

Issue the following command to execute the script:
```
chmod +x setup-swarm.sh
./setup-swarm.sh
```
At this moment, we have 3 nodes. 

A lot of this code used https://github.com/mlabouardy/alb-go as a basis, and followed the corresponding tutorial https://hackernoon.com/architecting-a-highly-scalable-golang-api-with-docker-swarm-traefik-875d1871cc1f.