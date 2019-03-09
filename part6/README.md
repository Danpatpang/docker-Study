# part6

## Docker 이미지의 자동 생성 및 공개

Dockerfile을 GitHub에 관리하고 DockerHub와 연결하면 Dockerfile로부터 자동으로 Docker 이미지를 생성하고 공개할 수 있다.

**Automated Build의 흐름**

→ GitHub에서 관리되는 Dockerfile을 바탕으로 Docker 이미지를 자동으로 빌드하는 기능

![](https://github.com/Danpatpang/docker-Study/blob/master/part6/img/1.png?raw=true)

**DockerHub의 링크 설정**

Settings → Linked Accounts → Link GitHub

## Docker Registry를 사용한 프라이빗 레지스트리 구축

**로컬 환경에 Docker 레지스트리 구축**

Docker Store에 공개되어 있는 공식 이미지인 registry를 사용하면 프라이빗 네트워크 안에서 구축할 수 있다.

- version 0 - python / version2 - go 호환성 X
- Docker 레지스트리는 프라이빗 네트워크 안에서만 이미지를 공개할 수 있다.

`docker search registry` - registry 검색

`docker image pull registry` - registry 다운로드

`docker image ls registry` - registry 이미지 확인

`docker container run -d -p 5000:5000 —name registry registry` - registry 컨테이너 시작

**Docker 이미지 업로드**

`docker image tag [로컬 이미지] [업로드할 레지스트리의 주소:포트 번호]/[이미지명]` : 도커 이미지에 태그 추가

`docker image push [업로드할 레지스트리의 주소:포트 번호]/[이미지명]` : 이미지 업로드

`docker image rm [업로드할 레지스트리의 주소:포트 번호]/[이미지명]` : 이미지 삭제

## 클라우드 서비스를 사용한 프라이빗 레지스트리 구축

Docker 이미지는 인프라 구성 요소에서 애플리케이션의 개발 환경 및 실행 모듈도 포함하기 때문에 용량이 큰 것도 있다.

→ 해당 문제를 해결하기 위해 클라우드 서비스 사용

![](https://github.com/Danpatpang/docker-Study/blob/master/part6/img/2.png?raw=true)

- 라즈베리파이는 ARM 프로세서이므로 x86 프로세서용 Docker 이미지는 사용할 수 없다.
- 라즈베리파이에 사용할 수 있는 이미지는 `rpi-`라는 이름이 붙어 있다.