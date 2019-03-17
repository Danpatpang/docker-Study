# Docker-compose(Scale)

## 내용

![](https://github.com/Danpatpang/docker-Study/blob/master/docker-compose/img/1.png?raw=true)

- 웹 애플리케이션 서버의 고가용성을 위해서 앞단에 HAProxy를 구성한다.
- 사용자 트래픽이 들어오면 HAProxy를 통해 서버로 트래픽을 분산하여 전달한다.
- 새로운 웹 애플리케이션 서버가 추가되면 HAProxy는 이를 인지하여 추가된 서버에도 트래픽을 전달한다.
- 기존 웹 애플리케이션 서버가 제거되면 HAProxy가 이를 인지하여 트래픽 전달 대상에서 제외한다.
- HAProxy와 웹 애플리케이션 서버를 docker-compose로 구성하고 scale 명령으로 웹 애플리케이션 서버를 확장한다.

## 준비

1. 디렉토리 생성 및 이동

    mkdir ~/docker-practice-scalable-web
    cd ~/docker-practice-scalable-web

2. 웹 애플리케이션 서버를 node로 제작.

    var http = require('http');
    var os = require('os');
    http.createServer((req, res) => {
    	res.writeHead(200, {'Content-Type':'text/html'});
    	res.end("<h1>I'm" + os.hostname() + "</h1>");
    }).listen(8000);

3. Dockerfile 작성 및 이미지 빌드

(1) Dockerfile 작성

`vi Dockerfile`

    #a를 누르면 입력할 수 있다.
    #입력이 끝난 후 Esc를 누르고 :wq를 치고 엔터를 누르면 저장된다.
    FROM node
    RUN mkdir -p /usr/src/app
    COPY index.js /usr/src/app
    EXPOSE 8080
    CMD [ "node", "/usr/src/app/index" ]

(2) 이미지 빌드

`docker build -t node-server ~/docker-practice-scalable-web/`

    Sending build context to Docker daemon  3.072kB
    Step 1/5 : FROM node
    latest: Pulling from library/node
    22dbe790f715: Pull complete 
    0250231711a0: Pull complete 
    6fba9447437b: Pull complete 
    c2b4d327b352: Pull complete 
    270e1baa5299: Pull complete 
    08ba2f9dd763: Pull complete 
    edf54285ab13: Pull complete 
    4d751c169397: Pull complete 
    Digest: sha256:065610e9b9567dfecf10f45677f4d372a864a74a67a7b2089f5f513606e28ede
    Status: Downloaded newer image for node:latest
     ---> 9ff38e3a6d9d
    Step 2/5 : RUN mkdir -p /usr/src/app
     ---> Running in c258d645d8f2
    Removing intermediate container c258d645d8f2
     ---> 762ff6213427
    Step 3/5 : COPY index.js /usr/src/app
     ---> 26282ebb8ff5
    Step 4/5 : EXPOSE 8000
     ---> Running in e32a2b1d7e49
    Removing intermediate container e32a2b1d7e49
     ---> 292834b216c0
    Step 5/5 : CMD [ "node", "/usr/src/app/index" ]
     ---> Running in b7226c1ed19d
    Removing intermediate container b7226c1ed19d
     ---> bc49779c6857
    Successfully built bc49779c6857
    Successfully tagged node-server:latest

(3)이미지 확인

`docker images`

## Docker-compose 실행

1. docker-compose 설치

    `sudo apt-get install docker-compose`

    `sudo apt upgrade docker-compose`

2. docker-compose.yml 파일 작성

    `vi docker-compose.yml`

        #a를 누르면 입력할 수 있다.
        #입력이 끝난 후 Esc를 누르고 :wq를 치고 엔터를 누르면 저장된다.version: '2'
        services:
          web:
            image: node-server
          lb:
            image: dockercloud/haproxy
            links:
             - web
            ports:
             - '80:80'
            volumes:
              - /var/run/docker.sock:/var/run/docker.sock

    - 다음과 같은 에러가 발생할 경우 docker-compose.yml의 version을 변경해야 한다.

        ERROR: Version in "./docker-compose.yml" is unsupported. You might be seeing this error because you're using the wrong Compose file version. Either specify a version of "2" (or "2.0") and place your service definitions under the `services` key, or omit the `version` key and place your service definitions at the root of the file to use version 1.
        For more on the Compose file format versions, see https://docs.docker.com/compose/compose-file/

3. docker-compose를 통해 컨테이너 생성

    `docker-compose -p practice up -d`

        Creating network "practice_default" with the default driver
        Pulling lb (dockercloud/haproxy:latest)...
        latest: Pulling from dockercloud/haproxy
        1160f4abea84: Pull complete
        b0df9c632afc: Pull complete
        a49b18c7cd3a: Pull complete
        Digest: sha256:040d1b321437afd9f8c9ba40e8340200d2b0ae6cf280a929a1e8549698c87d30
        Status: Downloaded newer image for dockercloud/haproxy:latest
        Creating practice_web_1
        Creating practice_lb_1

4. 컨테이너 동작 확인

    `docker ps`

        CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                   NAMES
        30af8b02cab2        dockercloud/haproxy   "/sbin/tini -- docke…"   3 minutes ago       Up 3 minutes        443/tcp, 0.0.0.0:80->80/tcp, 1936/tcp   practice_lb_1
        2f7f5ab0e595        node-server           "node /usr/src/app/i…"   3 minutes ago       Up 3 minutes        8000/tcp                                practice_web_1

5. 접속을 위한 IP확인 및 접속

    curl wgetip.com
    http://[출력된 IP 주소]

## 컨테이너 확장

1. web 서버의 컨테이너 수를 10개로 확장

    `docker-compose -p practice scale web=10`

        Creating and starting practice_web_2 ... done
        Creating and starting practice_web_3 ... done
        Creating and starting practice_web_4 ... done
        Creating and starting practice_web_5 ... done
        Creating and starting practice_web_6 ... done
        Creating and starting practice_web_7 ... done
        Creating and starting practice_web_8 ... done
        Creating and starting practice_web_9 ... done
        Creating and starting practice_web_10 ... done

2. HAProxy 로그 확인

    `docker logs practice_lb_1`

        frontend default_port_80
          bind :80
          reqadd X-Forwarded-Proto:\ http
          maxconn 4096
          default_backend default_service
        backend default_service
          server practice_web_1 practice_web_1:8000 check inter 2000 rise 2 fall 3
          server practice_web_10 practice_web_10:8000 check inter 2000 rise 2 fall 3
          server practice_web_2 practice_web_2:8000 check inter 2000 rise 2 fall 3
          server practice_web_3 practice_web_3:8000 check inter 2000 rise 2 fall 3
          server practice_web_4 practice_web_4:8000 check inter 2000 rise 2 fall 3
          server practice_web_5 practice_web_5:8000 check inter 2000 rise 2 fall 3
          server practice_web_6 practice_web_6:8000 check inter 2000 rise 2 fall 3
          server practice_web_7 practice_web_7:8000 check inter 2000 rise 2 fall 3
          server practice_web_8 practice_web_8:8000 check inter 2000 rise 2 fall 3
          server practice_web_9 practice_web_9:8000 check inter 2000 rise 2 fall 3
        INFO:haproxy:Config check passed
        INFO:haproxy:Reloading HAProxy
        INFO:haproxy:Restarting HAProxy gracefully
        INFO:haproxy:HAProxy is reloading (new PID: 15)
        INFO:haproxy:===========END===========

## 컨테이너 제거

1. 컨테이너 중지

    `docker-compose -p practice stop`

        Stopping practice_web_10 ... done
        Stopping practice_web_7 ... done
        Stopping practice_web_9 ... done
        Stopping practice_web_8 ... done
        Stopping practice_web_6 ... done
        Stopping practice_web_5 ... done
        Stopping practice_web_4 ... done
        Stopping practice_web_3 ... done
        Stopping practice_web_2 ... done
        Stopping practice_lb_1 ... done
        Stopping practice_web_1 ... done

2. 컨테이너 제거

    `docker-compose -p practice rm`

        Going to remove practice_web_10, practice_web_7, practice_web_9, practice_web_8, practice_web_6, practice_web_5, practice_web_4, practice_web_3, practice_web_2, practice_lb_1, practice_web_1
        Are you sure? [yN] y
        Removing practice_web_10 ... done
        Removing practice_web_7 ... done
        Removing practice_web_9 ... done
        Removing practice_web_8 ... done
        Removing practice_web_6 ... done
        Removing practice_web_5 ... done
        Removing practice_web_4 ... done
        Removing practice_web_3 ... done
        Removing practice_web_2 ... done
        Removing practice_lb_1 ... done
        Removing practice_web_1 ... done