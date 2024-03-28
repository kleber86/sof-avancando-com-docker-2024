# Criação do Cluster Swarm

# Criação do arquivo docker-compose.yml com as informações dos serviços.
```
version: '3'
services:
  web:
    image: jacksonlima91/app-python:v1
    deploy: 
      replicas: 5
      resources: 
        limits:
          cpus: "10docke"
          memory: 50mb
      restart_policy:
        condition: on-failure
    ports:
      - "4000:80"
    networks:
      - webnet
networks:
  webnet:
```
Comando: `docker stack deploy -c .\docker-compose.yml app-python-swarm_web`


# Usando o Docker Swarm, o comando abaixo lista os serviços:
```
docker service ls
``` 

# Comando para encerrar as instancias em execução:
```
docker service update --replicas 0 nome_do_serviço
```

# Comando para subir instancias em quantidades:
```
docker service update --replicas 5 nome_do_serviço
```

# Alterações no arquivo docker-compose.yml, executar o comando abaixo que é o mesmo de criação para atualizar:
```
docker stack deploy -c .\docker-compose.yml app-python-swarm_web
```

# Atualizando pilha
Adicionando mais um serviço e depois executar o comando para atualizar.
```
version: '3'
services:
  web:
    image: jacksonlima91/app-python:v1
    deploy: 
      replicas: 1
      resources: 
        limits:
          cpus: "10"
          memory: 50mb
      restart_policy:
        condition: on-failure
    ports:
      - "80:80"
    networks:
      - webnet
  visualizer:
    image: dockersamples/visualizer:stable
    ports: 
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy: 
      placement:
        constraints: [node.role == manager]
    networks:
      - webnet    
networks:
  webnet:
```