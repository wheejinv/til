**Table Of Contents**
- [Docker Compose](#docker-compose)
    - [도커 컴포즈를 쓰지 않고 워드프레스 셋팅하는 경우](#도커-컴포즈를-쓰지-않고-워드프레스-셋팅하는-경우)
        - [언인스톨](#언인스톨)
    - [도커 컴포즈로 셋팅.](#도커-컴포즈로-셋팅)
        - [docker compose 실행](#docker-compose-실행)
        - [docker compose 중지](#docker-compose-중지)

# Docker Compose

Docker를 모르거나, 컨테이너를 만들 때 마다 복잡한 명령어를 사용하는게 부담스럽다면 Docker Compose가 그 대안이 될 수 있다.

Compose에서 서비스를 정의하는 YAML 파일을 만들고, 단일 명령을 사용하여 모두 실행하거나 모두 종료할 수 있다.

Compose를 활용하면 복잡한 컨테이너의 관계들을 명확하게 문서화 할 수 있다.

Compose를 사용할 경우 `중요한` 이점은 파일에서 애플리케이션 스택을 정의하고 프로젝트 리포지토리 루트에 파일을 저장하여 다른 사용자가 참여하기 쉽게 만들 수 있다는 것.

아래 커맨드는 같은 네트워트에 MYSQL 컨테이너를 사용하는 워드프레스를 구성할 때 쓰이는 명령어이다.

참고 링크

- [서말 - 도커 지식 지도](https://seomal.com/map/1/129)
- [생활코딩 강좌 링크](https://www.youtube.com/watch?v=EK6iYRCIjYs)

## 도커 컴포즈를 쓰지 않고 워드프레스 셋팅하는 경우

첫번째 네트워크 설정

```shell
docker network create wordpress_net
```

두번째 MYSQL 컨테이너를 실행

```shell
docker \
run \
    --name "db" \  #컨테이너 이름
    -v "$(pwd)/db_data:/var/lib/mysql" \
    -e "MYSQL_ROOT_PASSWORD=123456" \
    -e "MYSQL_DATABASE=wordpress" \
    -e "MYSQL_USER=wordpress_user" \
    -e "MYSQL_PASSWORD=123456" \
    --network wordpress_net \
mysql:5.7 #mysql 이미지를 기반으로 생성.
```

세번째 워드프레스 컨테이너를 실행

```shell
docker \
    run \
    --name app \
    -v "$(pwd)/app_data:/var/www/html" \
    -e "WORDPRESS_DB_HOST=db" \
    -e "WORDPRESS_DB_USER=wordpress_user" \
    -e "WORDPRESS_DB_NAME=wordpress" \
    -e "WORDPRESS_DB_PASSWORD=123456" \
    -e "WORDPRESS_DEBUG=1" \
    -p 8080:80 \
    --network wordpress_net \
wordpress:latest
```

### 언인스톨

볼륨 폴더들은 수동 삭제 했음. (`app_data`, `db_data`)

```shell
docker rm --force app
docker rm --force db
docker network rm wordpress_net
```

## 도커 컴포즈로 셋팅.

아래는 위의 명령어로 실행했던 커맨드와 일치한다.

docker-compose.yml 파일

```yaml
version: "3.7"

services:
  db:
    image: mysql:5.7
    volumes:
      - ./db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 123456
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress_user
      MYSQL_PASSWORD: 123456

  app:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - ./app_data:/var/www/html
    ports:
      - "8080:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: wordpress_user
      WORDPRESS_DB_PASSWORD: 123456
```

### docker compose 실행
```shell
docker-compose up
```

### docker compose 중지
```shell
docker-compose down
```
