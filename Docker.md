[Docker cheatsheet](https://quickref.me/docker.html)
[Docker compose  cheatsheet](https://devhints.io/docker-compose)

Для того, чтобы работать с docker, необходимо добавить пользователя в группу `docker`:
```bash 
usermod -aG docker username
```


Удалять контейнеры можно по первым символам CONTAINER ID

```bash
docker rm c6
```
