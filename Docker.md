[Docker cheatsheet](https://quickref.me/docker.html)
[Docker compose  cheatsheet](https://devhints.io/docker-compose)


- Dockerfile + Docker-compose.yml
-  Github
-  Cheat sheet
## Run a new Container

```bash
$ docker run <image_name>                          # create and run a container from an image
$ docker run --name <container_name> <image_name>  # run with custom name
$ docker run -d <image_name>                       # run in the background
$ docker run -v <volume>:<container_path> <image_name>     # run with a mounted volume
$ docker run -p <host_port>:<container_port> <image_name>  # run with a mapped port
```
___

## Manage Containers

```bash
$ docker ps|ps -a                        # list of currently running|all containers
$ docker exec -it <container_name> bash  # open a bash inside a running container
$ docker start|stop <container_name|id>  # start or stop an existing container
$ docker rm  <container_name|id>         # remove a stopped container
```
___

## Manage Images

``` bash
$ docker images             # show list of all images
$ docker rmi <image_name>   # delete an image 
```

``` bash
$ docker build -t myapp .      # create image `myapp:latest`
$ docker build -t myapp:v1 .   # create image `myapp:v1`
$ docker build .               # create image without name and tag
```
___

## Docker Compose

Docker Compose is a tool for defining and running multi-container applications.

Compose simplifies the control of your entire application stack, making it easy to manage services, networks, and volumes in a single YAML configuration file. Then, with a single command, you create and start all the services from your configuration file.


> [!Note] 
> Все команды `docker compose`  завсисят от пути, в котором выполняется команда.
>  Docker Compose ищет `compose.yml` в **текущей директории**

```bash
$ docker compose up -d        # start all the services defined in compose.yaml in background
$ docker compose down         # stop and remove the running services
$ docker compose start|stop   # start all the containers or stop without deleting containers

$ docker compose ps           # list containers
```

Пример проекта `balcon-msk`

```
├── docker-compose.yml
├── mysql
│   └── data
├── nginx
│   └── default.conf
├── php
│   └── dockerfile
└── www
    └── balcon-msk
```

```yaml
# docker-compose.yml

services:
  mysql:
    image: mysql:5.7
    container_name: mysql
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: balcon-msk
    ports:
      - "3306:3306"
    volumes:
      - ./mysql/data:/var/lib/mysql
    networks:
      - network

  php:
    build: ./php
    container_name: php
    restart: unless-stopped
    volumes:
      - ./www:/var/www
    networks:
      - network
    depends_on:
      - mysql

  nginx:
    image: nginx:stable
    container_name: nginx
    restart: unless-stopped
    ports:
      - "80:80"
    volumes:
      - ./www:/var/www
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    networks:
      - network
    depends_on:
      - php

networks:
  network:
```
___

## Storage

Data written to the container layer doesn't persist when the container is destroyed. Data written to the container layer doesn't persist when the container is destroyed. This means that it can be difficult to get the data out of the container if another process needs it.  

Docker supports some types of storage mounts for storing data outside of the writable layer of the container.

### Volumes

Volumes are persistent data stores for containers, created and managed by Docker, isolated from the core functionality of the host machine. Volumes are a good choice for the following use cases:
- Volumes are easier to back up or migrate than bind mounts.
- You can manage volumes using Docker CLI commands or the Docker API.
- Volumes can be more safely shared among multiple containers.
- When your application requires high-performance I/O.

```bash
$ docker run -v <volume-name>:<mount-path> <image_name>  
$ docker run -v mysql_data:/var/lib/mysql mysql

$ docker run --mount type=volume,src=<volume-name>,dst=<mount-path> <image_name>
$ docker run --mount type=volume,src=mysql_data,dst=/var/lib/mysql mysql

# In general, `--mount` is preferred. The main difference is that the `--mount` flag is more explicit and supports all the available options.
```

`Volumes` are not a good choice if you need to access the files from the host, as the volume is completely managed by Docker. Use `bind mounts` if you need to access files or directories from both containers and the host.

#### Manage volumes

```bash
$ docker volume ls                 # list of all volumes
$ docker volume create my_volume   # create a new volume
$ docker volume rm my_volume       # delete a volume
```

### Bind Mount

When you use a bind mount, a file or directory on the host machine is mounted from the host into a container. By contrast, when you use a volume, a new directory is created within Docker's storage directory on the host machine, and Docker manages that directory's contents.

```bash
$ docker run -v <volume-path>:<container-path> <image_name>  
$ docker run -v /some/content:/usr/share/nginx/html nginx

$ docker run --mount type=bind,src=<volume-path>,dst=<container-path> <image_name>
$ docker run --mount type=bind,src=/some/content,dst=/usr/share/nginx/html nginx
```
___

## Networking

Container networking refers to the ability for containers to connect to and communicate with each other, and with non-Docker network services.

Containers have networking enabled by default, and they can make outgoing connections. A container has no information about what kind of network it's attached to, or whether its network peers are also Docker containers. A container only sees a network interface with an IP address, a gateway, a routing table, DNS services, and other networking details.

### Drivers

Docker Engine has a number of network drivers, as well as the default "bridge". On Linux, the following built-in network drivers are available:

- `Bridge`: Тип сети по умолчанию. Контейнеры получают внутренние IP-адреса из подсети 172.17.0.0/16. Чтобы достучаться до такого контейнера извне, необходимо использовать проброс портов

- `Host`: Контейнер не имеет собственного IP-адреса и использует сетевой стек хоста напрямую. Порты контейнера открываются прямо на IP-адресе сервера, что ускоряет работу, но убирает изоляцию портов. 

- `None`: Полная сетевая изоляция. У контейнера есть только локальный интерфейс (loopback), он не имеет внешнего IP и не может выходить в интернет.

### User-defined networks

With the default configuration, containers attached to the default bridge network have unrestricted network access to each other using container IP addresses. **They cannot refer to each other by name**.

It can be useful to separate groups of containers that should have full access to each other, but restricted access to containers in other groups.

You can create custom, user-defined networks, and connect groups of containers to the same network. Once connected to a user-defined network, containers can communicate with each other using container IP addresses or container names.

## Standard vs user-defined bridge

| Характеристика | **Standard bridge**           | **User-defined bridge**       |
| -------------- | ----------------------------- | ----------------------------- |
| **DNS**        | ❌ Только по IP                | ✅ Автоматический DNS          |
| **Изоляция**   | ❌ Все контейнеры в одной сети | ✅ Каждая сеть изолирована     |
| **Подсеть**    | Фиксированная (172.17.0.0/16) | Настраиваемая (можно выбрать) |
Для реальных проектов рекомендуется создавать собственные сети.
___

## Tips

Для того, чтобы работать с `docker` на linux и запускать команды без `sudo`, необходимо добавить пользователя в группу `docker`:

```bash 
usermod -aG docker username
```


Удалять контейнеры можно по первым символам CONTAINER ID

```bash
docker rm c6
```
