# part7

## 여러 컨테이너 관리의 개요

**웹 3계층 시스템 아키텍처**

1. 프론트 서버

    HTTP 요청과 응답을 반환하는 서버 기능을 가진다.

    웹 프론트 서버 또는 웹 서버라고 한다.

    요청에 대한 처리가 메인 업무이므로 부하가 높은 경우 처리 대수를 늘리거나, 로드밸런서를 사용하여 부하를 분산한다.

2. 애플리케이션 서버

    업무 처리를 실행하는 서버.

3. 데이터베이스 서버

    영구 데이터를 관리하기 위한 서버.

    영구 데이터는 높은 가용성이 요구되기 때문에 클러스터링과 같은 기술로 다중화하는 경우가 많다.

    백업이나 원격지 보관 등과 같은 대책 필요.

    ![](https://github.com/Danpatpang/docker-Study/blob/master/part7/img/1.png?raw=true)

**영구 데이터의 관리**

스토리지의 저장 영역에는 한계가 있으며 고장 등으로 데이터가 소멸될 가능성도 있기 때문에 영구 데이터를 적절히 관리할 필요가 있다.

- 데이터의 백업 및 복원
- 로그 수집

    여러 개의 서버로 된 분산 환경에서 통합 감시를 하는 경우는 로그 수집 전용 서버를 마련하는 것이 일반적이다.

- Docker 컨테이너는 트래픽의 증감에 맞춰 필요할 때 실행하고 필요 없어지면 파기하는 일회성 운용에 적합하다.
- 컨테이너 안은 중요한 영구 데이터를 저장하는데 적합하지 않다.

**Docker Compose**

여러 컨테이너를 모아서 관리하기 위한 툴.

`docker-compose.yml`라는 파일에 컨테이너의 구성 정보를 정의함으로써 여러 컨테이너를 일관적으로 관리한다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part7/img/2.png?raw=true)

## 웹 애플리케이션을 로컬에서 움직여 보자

C**ompose 구성 파일 작성**

1. 샘플 애플리케이션 복제

        git clone https://github.com/assahiho/dockertext2
        cd dockertext2/chap07/

2. Compose 정의 파일 확인

**여러 Docker 컨테이너 시작**

각각의 컨테이너를 작동시키기 위한 이미지의 다운로드나 빌드를 하나의 명령으로 모두 실행.

`docker-compose up` : 컨테이너 시작

`docker-compose ps` : 컨테이너 확인

**여러 Docker 컨테이너 정지**

`docker-compose stop` : 컨테이너 정지

`docker-compose down` : 리소스 삭제

## Docker Compose를 사용한 여러 컨테이너의 구성 관리

**docker-compose.yml 개요**

**Compos**e 정의 파일에 시스테 안에서 가동하는 여러 서버들의 구성을 모아서 정의한다.

YAML 형식으로 기술한다.

여러 컨테이너의 설정 내용을 모아서 하나의 파일에 기술하며 버전을 지정할 때는 파일의 맨 앞에 정의.

`version : '3.3'`

**이미지 지정**

    services:
    webserver:
    	image: ubuntu

베이스 이미지를 지정하려면 `image`를 사용한다.

이미지의 태그가 없을 경우 최신 버전으로 다운로드 된다.

DockerHub에 공개되어 있는 이미지는 모두 지정 가능하다.

**이미지 빌드**

    services:
    	webserver:
    		build: . # 피리어드로 현재 디렉토리를 나타낸다.

Dockerfile에 기술하고 그것을 자동으로 빌드하여 베이스 이미지로 지정.

`build`에는 현재 Dockerfile의 파일 경로를 지정한다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part7/img/3.png?raw=true)

    services:
    	webserver:
    		build:
    			context: /data
    			dockerfile: Dockerfile-alternate
    			args:
    				prjectno: 1
    				user: asa

디렉토리의 경로나 Git 리포지토리의 URL은 `context`로 지정

도커파일의 이름은 `dockerfile`로 지정

아규먼트의 경우 `args`로 지정

**컨테이너 안에서 작동하는 명령 지정**

    command: /bin/bash

컨테이너에서 작동하는 명령은 `command`로 지정한다.

**컨테이너 간 연결**

다른 컨테이너에 대한 링크 기능을 사용하고 연결하고 싶을 때는 `link` 사용.

    #logserver라는 이름의 컨테이너와 링크 '서비스명:앨리어스명'
    links:
    	- logserver
    	- logserver:log01

**컨테이너 간 통신**

컨테이너가 공개하는 포트는 ports로 지정한다.

`'호스트 머신의 포트 번호:컨테이너의 포트 번호'`

호스트 머신에 대한 포트를 공개하지 않고 링크 기능을 사용하여 연결하는 컨테이너에게만 포트를 공개할 때는 `expose`를 지정한다.

    #port
    ports:
    	- "3000"
    	- "8000:8000"
    	- "49100:22"
    	- "127.0.0.1:8001:8001"
    #expose
    expose:
    	- "3000"
    	- "8000"

**서비스의 의존관계 정의**

여러 서비스의 의존관계를 정의할 때는 `depends_on`을 지정한다.

    #webserver 컨테이너를 시작 전에 db 컨테이너와 redis 컨테이너 시작
    services:
    	webserver:
    		build: .
    		depends_on:
    			- db
    			- redis
    	redis:
    		image: redis
    	db:
    		image: postgres

- 컨테이너의 시작 순서만 제어할 뿐 컨테이너상의 애플리케이션이 이용 가능해질 때까지 기다리고 제어하지 않는다.

**컨테이너 환경변수 지정**

환경변수를 지정할 때는 `environment` 지정한다.

설정하고 싶은 환경변수의 수가 많을 때는 `env_file`을 읽어들인다.

    #배열 형식
    environment:
    	-HOGE=fuga
    	-FOO
    #해시 형식
    environment:
    	HOGE: fuga
    	FOO:
    
    env_file:
    	- ./envfile1
    	- ./app/envfile2
    	- /tmp/envfile3

- API 키와 같은 비밀정보의 관리는 오케스트레이션 기능을 이용할 것.

**컨테이너 정보 설정**

    #컨테이너명 지정
    container_name: web-container
    #컨테이너 라벨 설정
    labels:
    	- "com.example.description=Accounting webapp"
    	- "com.example.department=Finance"
    #해시 형식으로 지정
    labels:
    	com.example.description: "Accounting webapp"
    	com.example.department: "Finance"

- 설정한 라벨은 `docker-compose config`로 확인한다.

## Docker Compose를 사용한 여러 컨테이너의 운용

**Docker Compose의 버전 확인**

`docker-compose --version`

- 현재 디렉토리 안에 `docker-compose.yml` 파일이 없다면 명령 실행 시 오류 발생.

**Docker Compose의 기본 명령**

docker compose의 명령은 docker-compose.yml을 저장한 디렉토리에서 실행된다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part7/img/4.png?raw=true)

**여러 컨테이너 생성**

`docker-compose up [옵션] [서비스명 .]`

![](https://github.com/Danpatpang/docker-Study/blob/master/part7/img/5.png?raw=true)

→ `docker-compose up -d`

→ `docker-compose up --build`

→ `docker-compose up --scale [서비스명=수]`

**여러 컨테이너 확인**

→ `docker-compose ps`

→ `docker-compose ps - q`

→ `docker-container ls`

→ `docker-container logs`

**컨테이너에서 명령 실행**

→ `docker-compose run server_a /bin/bash`

**여러 컨테이너 시작/정지/재시작**

→ `docker-compose start`

→ `docker-compose stop`

→ `docker-compose restart`

**여러 컨테이너 일시 정지/재개**

→ `docker-compose pause`

→ `docker-compose unpause`

**서비스의 구성 확인**

→ `docker-compose port [옵션] <서비스명> <프라이빗 포트 번호>`

→ `docker-compose config`

**여러 컨테이너 강제 정지/삭제**

→ `docker-compose kill -s SIGINT`

→ `docker-compose kill -s SIGKILL`

→ `docker-compose rm`

**여러 리소스의 일괄 삭제**

실행 중인 컨테이너를 정지시키고, 이미지, 네트워크, 볼륨을 일괄 삭제

→ `docker-compose down [옵션]`

![](https://github.com/Danpatpang/docker-Study/blob/master/part7/img/6.png?raw=true)