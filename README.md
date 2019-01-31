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

## Homework docker-2

- Создан новый проект в GCP
- Установлен SDK и сконфигурирован gcloud
- Установлен `docker-machine`
- Установлен `docker host` на ВМ в GCP через ` docker-machine create --driver google...`
- Проверил что docker хост успешно создан `docker-machine ls`
- Сравнил выводы
```
docker run --rm -ti tehbilly/htop
docker run --rm --pid host -ti tehbilly/htop
```
- Создал файлы `Dockerfile`, `mongod.conf`, `db_config`, `start.sh`
- Собрал образ через `docker build -t reddit:latest .`
- Просмотрел все образыв том числе и промежуточные `docker images -a`
- Запустил контейнер ` docker run --name reddit -d --network=host reddit:latest`
- Проверил что контейнер запущен `docker-machine ls`
- Открыл порт через `gcloud compute firewall-rules create`
- Зарегистрировался под alxkondgcptest на docker hub
- Пометил и загрузил образ на docker hub 
``` 
docker tag reddit:latest alxkondgcptest/otus-reddit:1.0
docker push alxkondgcptest/otus-reddit:1.0
```
- Запустил запуск контейнера из другой консоли `docker run --name reddit -d -p 9292:9292 alxkondgcptest/otus-reddit:1.0`
- Посмотрел логи контейнера `docker logs reddit -f`
- Запустил контейнер в интерактивном режиме `docker exec -it reddit bash` 

## Homework docker-3

- Создал `Dockerfile` для отдельных сервисов
- Скачал образ MongoDB `docker pull mongo:latest`
- Собрал образы с сервисами
```
docker build -t alxkondgcptest/post:1.0 ./post-py
docker build -t alxkondgcptest/comment:1.0 ./comment
docker build -t /ui:1.0 ./ui
```
- Создал отдельную сеть reddit `docker network create reddit`
- Запустил контейнеры
```
docker run -d --network=reddit \
--network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit \
--network-alias=post alxkondgcptest/post:1.0
docker run -d --network=reddit \
--network-alias=comment alxkondgcptest/comment:1.0
docker run -d --network=reddit \
-p 9292:9292 alxkondgcptest/ui:1.0
```
- Выключил контейнеры docker `kill $(docker ps -q)`
- Создал volume 
- Перезапустил контейнеры, добавив ключ `-v reddit_db:/data/db` 
```
sudo docker run -d --network=reddit \
--network-alias=post_db \
--network-alias=comment_db -v reddit_db:/data/db mongo:latest
sudo docker run -d --network=reddit \
--network-alias=post alxkondgcptest/post:1.0
sudo docker run -d --network=reddit \
--net work-alias=comment alxkondgcptest/comment:1.0
sudo docker run -d --network=reddit \
-p 9292:9292 alxkondgcptest/ui:1.0
```
