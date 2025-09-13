# run-ollama

Load the image

```
docker load -i output/ollama-deepseek-1.5b.tar
docker tag $(docker images -q | head -n1) ollama-with-deepseek-1.5b:latest
```

Run the Stack

```
docker-compose up -d
```

Acesse Open WebUI

```
http://localhost:3000
```

If everything is running, now is time to block the containers internet external acess the containers to install

```
docker ps
```

Get the open-web and ollama containers_id

enter in the containers

```
docker exec -it container_id_openwebui bash
```

```
docker exec -it container_id_ollama bash
```

and make

```
apt update && apt install -y iputils-ping curl
```

Now, lets block the internet acess

```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ollama
```

```
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' openwebui
```

Get the IP and make <container_ip>

```
sudo iptables -I DOCKER-USER -s <container_ip> ! -d 172.18.0.0/16 -j DROP
```

Now, enter in the containers and make

```
ping google.com
curl http://google.com
```

If you not receive info, the external acess to internet is blocked for the container