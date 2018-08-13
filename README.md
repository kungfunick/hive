# Hive Docker Swarm
## Prerequisites
Make sure Docker is fully installed - my installation had docker supplementry files but I had to run ``` brew cask install docker``` to fix it.

To create a Swarm cluster: 
Use the shell script (setup-swarm.sh) to setup a cluster with 3 nodes (1 Manager & 2 Workers)

Issue the following command to execute the script:
```
chmod +x setup-swarm.sh
./setup-swarm.sh
```

## Stage 1
At this moment, we have 3 nodes. 

* We use an overlay network named traefik-net, on which we add the services we want to expose to Traefik.
* We use constraints to deploy the APIs on workers & Traefik on Swarm manager.
* Traefik container is configured to listen on port 80 for the standard HTTP traffic, but also exposes port 8080 for a web dashboard.
* The use of docker socket ```/var/run/docker.sock``` This allows Traefik to listen to Docker Daemon events, and reconfigure itself when containers are started/stopped.
* The label traefik.frontend.rule is used by Træfik to determine which container to use for which Request Path.
* The configs part create a configuration file for Traefik from config.toml(it enables the Docker backend)

## Stage 2

Run ```docker swarm init``` or ```docker swarm join``` to initialise the swarm

In order to deploy our stack, we should execute the following command:
```
docker stack deploy --compose-file docker-compose.yml api
```

However, we may encounter this error: 
```
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```
Make sure that docker is running! Took a while to track it down. 


To check the overlay network:
```
docker network ls
```

Traefik configuration:
```
docker config ls
```

To display the configuration content:
```
docker config inspect api_traefik-config — pretty
```

And finally, to list all the services:
```
docker stack ps api
```

### Credits
A lot of this code used https://github.com/mlabouardy/alb-go as a basis, and followed the corresponding tutorial https://hackernoon.com/architecting-a-highly-scalable-golang-api-with-docker-swarm-traefik-875d1871cc1f.
https://www.upcloud.com/support/how-to-configure-docker-swarm/ was quite informative too, so I've used a lot of that advice too.
Things went wrong, so https://forums.docker.com/t/cannot-connect-to-the-docker-daemon-is-the-docker-daemon-running-on-this-host/8925 helped a bit too