**Потребуется**

- gitlab (Можно использовать https://gitlab.com/)
- Раннер с докером (Так же можно использовать от https://gitlab.com/ или поставить свой https://docs.gitlab.com/runner/install/)
- Серер с docker swarm (https://docs.docker.com/engine/swarm/ , для простоты можно взять один сервер с докер и выполнить команду `docker swarm init`)
- Регистр докера (Так же есть на gitlab)

**Подготовка**

Предпологается, что есть gitlab и подключен раннер.

Для подключения из Раннера к серверу с swarm потребуется сгенерировать сертификаты и добавить их в Variables в gitlab. 
Статья по генерации сертификатов https://itc-life.ru/nepreryvnaya-integraciya-i-razvertyvanie-na-primere-gitlab-ci-docker-swarm/

В Variables должны быть добавлены как минимум:
- CI_REGISTRY_PASSWORD - пароль к gitlab
- CI_REGISTRY_USER - логин от gitlab
- TLSCACERT
- TLSCERT
- TLSKEY
- CI_DOCKER_HOST - сервер где настроен docker swarm, пример tcp://130.193.38.166:2376


------
Предворительно создаем образы сервисов с нашими настройками, файл docker/nginx/Dockerfile и заливаются в регистр докера. 
Эти образы указываюьтся в docker/nginx/DockerfileBuild, это не обязательно, но скорость сборки будет происходить быстрее. 


------
На сервере docker swarm созадем файл hmtl, любой. И создаем config на этот файл с именем sec_html используется docker/docker-compose.deploy.yml


----
В файле docker/docker-compose.deploy.yml нужно прописать свои image




**Запуск**

Отправляем коммит, в разделе Pipelines появится запись. После выполнения всех этапов на сервере docker swarm появятся запущенные сервисы `docker service ls`. 






**Разное**

Создать образ

- `docker login -u <login> -p <pass> registry.gitlab.com`
- `docker build -t registry.gitlab.com/<login>/<project>/nginx -f docker/nginx/DockerfileBuild .`
- `docker push registry.gitlab.com/<login>/<project>/nginx`


