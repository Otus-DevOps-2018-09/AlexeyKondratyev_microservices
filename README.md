# AlexeyKondratyev_microservices
AlexeyKondratyev microservices repository

## Homework docker-1

- Установил docker
- Запустил контейнер `hello-world` командой `docker run hello world`
- Посмотрел список запущенных контейнеров командой `docker ps`
- Посмотрел список всех контейнеров командой `docker ps -a`
- Посмотрел список образов командой `docker images`
- Создал и запустил контейнер через `docker run -it ubuntu:16.04 /bin/bash`
- Запустил bash в контейнере `docker exec -it <u_container_id> bash`
- Создал новый образ из контейнера через `docker commit`
- Сравнил чем отличается образ от контейнера
```
docker inspect <u_container_id>
docker inspect <u_image_id>
```
- Посмотрел занятое дисковое пространство через `docker system df`
- Удалил все контейнеры командой `docker rm $(docker ps -a -q)`
- Удалил все образы командой `docker rmi $(docker images -q)`
