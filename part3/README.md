# part3

Docker 설치는 도입할 호스트 OS에 따라 절차가 다르다.

## Mac

→ 웬만하면 설치 다 된다.

## Windows

→ Windows 10 Pro 또는 Enterprise(64bit)

→ VirtualBox와 같은 서드파티 제품 가상화 환경 설치하지 않을 것

→ Hyper-V를 유효화할 것

## Linux

배포판이나 버전에 따라 설치 절차가 다르다.

**사전 준비**

`sudo apt-get update`

→ apt(advanced packaging tool) 업데이트

`sudo apt-get install -y \` 

`apt-transport -https \` 

`curl \` 

`software-properties-common`

→ HTTPS를 경유하여 리포지토리를 사용할 수 있도록 함

======================

apt-transport -https

curl

software-properties-common

======================

`curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) | sudo apt-key add -`

→ Docker의 공식 GPG 키 추가

======================

GPG

======================

`sudo apt-key fingerprint 0EBFCD88`

→ GPG 키 확인

`sudo add-apt-repository \`

`"deb [arch=amd64] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) \`

`$(lsb_release -cs) \`

`stable"`

`sudo apt-get update`

→ 리포지토리 등록

**Docker 설치**

`sudo apt-get install docker-ce`

→ Docker 설치 (항상 Y를 입력할 것)

**Docker에서 'Hello world'**

Docker 컨테이너를 작성 및 실행할 때는 `docker container run`  사용.

- `docker container run <Docker 이미지> <실행할 명령>`

`docker container run ubuntu:latest /bin/echo 'Hello world'`

→ 우분투의 이미지를 Docker 컨테이너에 작성 후 실행 및 echo 출력.

1. 명령이 실행되면 로컬 환경에 Docker 이미지 존재 유무를 확인.
2. 없다면 다운로드가 실행되고 명령이 실행.
3. 있다면 바로 명령이 실행.

![](https://github.com/Danpatpang/docker-Study/blob/master/part3/img/1.png?raw=true)

![](https://github.com/Danpatpang/docker-Study/blob/master/part3/img/2.png?raw=true)

- 로컬 환경에 있는 Docker 이미지를 로컬 캐시라고 한다.

**Docker 버전 확인**

`docker version`

→ Docker의 버전, OS, 아키텍처 등을 확인할 수 있다.

- Docker는 클라이언트 / 서버 아키텍처를 채택하고 있어서 Docker 클라이언트와 Docker 서버가 Remote API를 경유하여 연결되어 있다.
- 즉, Docker 명령은 서버로 보내져 처리된다.

**Docker 실행 환경 확인**

`docker system info`

→ 컨테이너 수, 버전, 스토리지, OS 종류 등 상세 설정을 볼 수 있다.

**Docker 디스크 이용 현황**

`docker system df`

→ Docker가 사용하고 있는 디스크의 이용 상황을 볼 수 있다.

## 웹 서버 구축하기

Nginx는 대량의 요청을 처리하는 대규모 사이트에서 주로 사용되며, 리버스 프록시나 로드밸런서와 같은 기능을 갖고 있다.

**이미지 다운로드**

Docker 이미지는 공식 리포지토리인 Docker Hub에서 구할 수 있다.

이미지에는 애플리케이션을 작동하기 위해 필요한 것들이 패키징되어있다.

`docker pull nginx`

→ nginx 이미지 다운로드

`docker image ls`

→ 다운로드된 이미지 확인

**Nginx 작동하기**

`docker container run —name webserver -d -p 80:80 nginx`

→ Docker 이미지 nginx를 사용하여 webserver라는 이름의 컨테이너를 기동

→ `-p` 옵션을 붙여 브라우저에서 80번 포트에 대한 액세스 허가

- 실행 결과로 출력되는 문자열은 컨테이너 ID로 Docker 컨테이너를 식별하기 위한 것.

**Nginx 작동 확인**

localhost는 로컬 루프백 주소이므로 [http://localhost:80](http://localhost:80으로) 으로 작동 확인

`docker container ps`

→ 실행시킨 Nginx 서버의 상태 확인

`docker container stats`

→ 컨테이너의 상세 내용 확인

**Nginx 실행 및 중지**

`docker stop webserver`

→ 컨테이너 중지

`docker start webserver`

→ 컨테이너 시작