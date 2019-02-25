# part4

## Docker 이미지 조작

**Docker Hub**

코드를 빌드하는 기능이나 이미지를 관리하는 기능을 갖춘 공식 리포지토리 서비스.

→ 공식 이미지 외에도 사용자가 작성한 Docker 이미지를 공개할 수 있다.

`이미지명:태그명`

→ `debian:7`은 Debain 버전 7을 베이스 이미지로 갖는 Docker 이미지를 의미한다.

→ latest는 리포지토리에 공개된 최신판 이미지를 의미한다.(항상 바뀌므로 주의)

**이미지 다운로드**

`docker image pull [옵션] 이미지명[:태그명]`

→ `docker image pull centos:7` CentOS 버전 7 다운로드

→ `docker image pull -a centos` 모든 태그를 다운로드(-a 옵션에는 태그 불가)

url 다운로드의 경우 프로토콜은 제외하고 사용한다.

→ `docker image pull gcr.io.tensorflow/tensorflow` TensorFlow 다운로드

**이미지 목록 표시**

`docker image ls [옵션] [리포지토리명]`

→ `-a` 모든 이미지 표시

→ `— digests` 다이제스트를 표시할지 말지

→ `— no -trunc` 결과를 모두 표시

→ `-q` Docker 이미지 ID만 표시

Docker 이미지를 작성하면 고유한 이미지 ID가 부여된다.

또한 Docker 레지스트리에 업로드한 이미지는 식별을 위해 다이제스트가 부여된다.

→ 제 3자의 변조를 막기 위해 DCT(Docker Content Trust) 기능을 사용한다.

→ 이미지 배포 시 서명 → 다운로드 시 공개 키를 통한 검증

**이미지 상세 정보 확인**

`docker image inspect`

→ 이미지 ID, 작성일, Docker 버전, CPU 아키텍처 등이 출력된다.

→ 결과 값은 JSON 형식으로 출력된다.

→ `— format` 옵션을 사용하면 원하는 정보를 얻을 수 있다.

`docker image inspect —format="{{ .Os}}" centos:7`

`docker image inspect —format="{{ . ContainerConfig.Image}}" centos:7`

**이미지 태그 설정**

이미지 태그에는 식별하기 쉬운 버전명을 붙이는 것이 일반적이다.

`<사용자명>/이미지명[:태그명]`

→ 태그를 붙인 이미지와 원래 이미지 ID는 동일하다. (이미지 자체의 변화는 없다)

**이미지 검색**

`docker search [옵션] <검색 키워드>`

→ Docker Hub에 공개되어 있는 이미지 검색

→ `—no-trunc` 모든 결과 표시

→ `—limit` n 건의 검색 결과 표시

→ `—filter=stars=n` star n개 이상 표시

- 공식 Docker 이미지는 OFFICIAL이 OK로 표시된다.
- Docker Hub에 공개되어 있는 이미지는 모두 안전하다고 볼 수 없다.
- 그래서 공식 이미지인지 Dockerfile이 공개되어 있는지 확인할 필요가 있다.

**이미지 삭제**

`docker image rm [옵션] 이미지명 [이미지명]`

→ `-f` 이미지를 강제로 삭제

→ `—no-prune` 중간 이미지를 삭제하지 않음

→ 이미지명에는 REPOSITORY 또는 IMAGE ID를 지정한다.

→ IMAGE ID의 경우 앞의 몇 자리만 적어도 된다.

`docker image prune [옵션]`

→ 사용하지 않은 이미지를 삭제할 때 사용

→ `-a` 사용하지 않은 이미지를 모두 삭제

→ `-f` 강제 삭제

**Docker Hub 로그인 / 로그아웃**

`docker login [옵션] [서버]`

→ `-u` 사용자명, `-p` 비밀번호

`docker logout [서버명]`

→ 서버명을 지정하지 않았을 때는 Docker Hub에 액세스.

**이미지 업로드**

`docker image push 이미지명[:태그명]`

`docker image push <사용자명>/이미지명[:태그명]`

→ Docker Hub에 사용자 계정으로 해당 이미지를 업로드

## Docker 컨테이너 생성 / 시작 / 정지

**Docker 컨테이너의 라이프 사이클**

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/1.png?raw=true)

1. 컨테이너 생성
    - 이미지로부터 컨테이너 생성.
    - 이미지의 실체는 "Docker에서 작동하기 위해 필요한 디렉토리 및 파일들"
    - `docker container create`을 통해 스냅샷 생성

        ![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/2.png?raw=true)

2. 컨테이너 생성 및 시작
    - 이미지로부터 컨테이너 생성 후 컨테이너 상에서 임의의 프로세스를 시작.
    - `docker container run`

        ![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/3.png?raw=true)

3. 컨테이너 시작
    - 정지 중인 컨테이너를 시작.
    - `docker container start`
4. 컨테이너 정지
    - 실행 중인 컨테이너를 정지
    - `docker container stop`, `docker container restart`
5. 컨테이너 삭제
    - 정지 중인 컨테이너 삭제
    - `docker container rm`

**컨테이너 생성 및 시작**

`docker container run [옵션] 이미지명[:태그명] [인수]`

→ `-a` 표준 입력, 출력, 오류 출력에 접근한다.

→ `—cidfile` 컨테이너 ID를 파일로 출력한다.

→ `-d` 컨테이너를 생성하고 백그라운드에서 실행한다.

→ `-i` 컨테이너의 표준 입력을 연다.

→ `-t` 단말기 디바이스를 사용한다.

- 대화식으로 사용하기 위해 보통 `-i`, `-t` 옵션 사용

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/4.png?raw=true)

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/5.png?raw=true)

→ `—name` 옵션을 통해 컨테이너에 이름을 붙일 수 있다.

→ 생성된 컨테이너에서 /bin/cal을 실행.

→ /bin/bash를 실행할 경우 컨테이너 안에서 쉘이 실행된다.

**컨테이너의 백그라운드 실행**

`docker container run [실행 옵션] 이미지명[:태그명] [인수]`

→ 백그라운드에서 실행

→ `-d` 백그라운드에서 실행

→ `-u` 사용자명을 지정

→ `—restart=[no | on-failure | on-failure:n | always |unless-stopped]` 명령의 실행 결과에 따라 재시작

→ `—rm` 명령 실행 완료 후에 컨테이너를 자동 삭제

- 백그라운드에서 실행하기 위해 `-d` 옵션 사용

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/6.png?raw=true)

→ 결과 화면이 표시되지는 않지만 컨테이너의 ID가 표시된다.

→ 백그라운드에서 실행되는지 확인 하기 위해 `docker container logs` 명령 사용.

→ 자동 삭제를 원할 시 `—rm`, 재시작을 원할 시 `—restart` 옵션을 지정.

- 단, rm과 restart 명령은 동시에 줄 수 없다.

**컨테이너의 네트워크 설정**

`docker container run [네트워크 옵션] 이미지명[:태그명] [인수]`

→ `—add-host=[호스트명:IP 주소]` /etc/hosts에 호스트명과 IP주소 정의

→ `—dns=[IP 주소]` 컨테이너용 DNS 서버 주소 지정

→ `—expose` 지정한 범위의 포트 번호 할당

→ `—net=[bridge |  none | host | NETWORK | container:<name | id>]` 컨테이너 네트워크 지정

→ `-h` 컨테이너 자신의 호스트명을 지정

→ `-p[호스트의 포트 번호]:[컨테이너의 포트 번호]` 호스트와 컨테이너의 포트 매칭

→ `-p` 호스트의 임의의 포트를 컨테이너에 할당

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/7.png?raw=true)

사용자 정의 네트워크는 `docker network create` 명령으로 실행.

**자원을 지정하여 컨테이너 생성 및 실행**

`docker container run [자원 옵션] 이미지명[:태그명] [인수]`

→ `-c` CPU의 사용 배분(기본 값 1024)

→ `-m` 사용할 메모리를 제한하여 실행

→ `-v` 호스트와 컨테이너의 디렉토리를 공유

`docker container run -v /Users/asa/webap:/usr/share/nginx/html nginx`

**컨테이너를 생성 및 시작하는 환경을 지정**

`docker container run [환경설정 옵션] 이미지명[:태그명] [인수]`

→ `-e` 환경변수를 설정

→ `—env-file=[파일명]` 환경변수를 파일로부터 설정

→ `—read-only[true | false]` 컨테이너의 파일 시스템을 읽기 전용

→ `-w` 컨테이너의 작업 디렉토리 지정

→ `-u` 사용자명 지정

환경변수를 정의한 파일로부터 일괄적으로 등록하고 싶은 경우

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/8.png?raw=true)

컨테이너의 작업 디렉토리를 지정하여 실행하고 싶은 경우

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/9.png?raw=true)

**가동 컨테이너 목록 표시**

`docker container ls [옵션]`

→ `-a` 모든 컨테이너 표시

→ `-f` 표시할 컨테이너의 필터링

→ `-n` 마지막으로 실행된 n건의 컨테이너만 표시

→ `-l` 마지막으로 실행된 컨테이너만 표시

→ `—no-trunc` 정보를 생략하지 않고 표시

→ `-q` 컨테이너 ID만 표시

→ `-s` 파일 크기 표시

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/10.png?raw=true)

- Docker는 컨테이너마다 CONTAINER ID가 할당된다.

**컨테이너 가동 확인**

`docker container stats [컨테이너 식별자]`

→ Ctrl + C를 눌러 명령을 종료

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/11.png?raw=true)

**컨테이너 시작 / 정지 / 재시작**

`docker container start [옵션] <컨테이너 식별자> [컨테이너 식별자]`

→ 정지하고 있는 컨테이너를 시작한다. (여러 개를 실행하고 싶다면 식별자 여러 개 선언)

→ `-a` 표준 출력, 표준 오류 출력을 연다.

→ `-i` 컨테이너의 표준 입력을 연다.

`docker container stop [옵션] <컨테이너 식별자> [컨테이너 식별자]`

→ 실행 중인 컨테이너를 정지한다.

→ `-t` 컨테이너의 정지 시간을 지정 (기본 10초)

→ `docker container kill` 명령을 사용하여 강제 정지 할 수 있다.

`docker container restart [옵션] <컨테이너 식별자> [컨테이너 식별자]`

→ 컨테이너 재시작

→ `-t` 컨테이너의 재시작 시간을 지정 (기본 10초)

→ `docker container restart -t 2 webserver` 컨테이너 2초 후에 재시작

- GUI 대신 CLI를 사용하는 이유는 네트워크를 통한 조작에 편리하고 조작에 의한 실수를 줄일 수 있기 때문이다.
- 최종적으로 자동화를 시키는 것에 유리하다.

**컨테이너 삭제**

`docker container rm [옵션] <컨테이너 식별자> [컨테이너 식별자]`

→ 정지하고 있는 컨테이너 삭제

→ `-f` 실행 중인 컨테이너 강제 삭제

→ `-v` 할당한 볼륨을 삭제

→ 정지 중인 모든 컨테이너를 삭제하려면 `docker container prune` 명령을 사용한다.

**컨테이너 중단 / 재개**

`docker container pause <컨테이너 식별자>`

→ 컨테이너 중단

`docker container unpause <컨테이너 식별자>`

→ 컨테이너 재개

## Docker 컨테이너 네트워크

`docker network ls [옵션]`

→ `-f` 출력 필터링

→ `--no-trunc` 상세 정보 출력

→ `-q` 네트워크 ID만 표시

Docker는 기본 값으로 bridge, host, none을 만든다.

`docker network ls -q -f driver=bridge`

컨테이너 시작 시 네트워크를 명시적으로 지정하지 않았을 때는 bridge 네트워크가 기본 값이 된다.

`docker network create [옵션] 네트워크`

→ `-d` 네트워크 브릿지

→ `--ip-range` IP 주소의 범위 지정

→ `--subnet` 서브넷 지정

→ `--ipv6` IPv6 네트워크를 유효화할지 말지 지정

`docker network create -d=bridge web-network`

`docker network ls -f driver=bridge`

**네트워크 연결**

`docker network connect [옵션] 네트워크 컨테이너`

→ `--ip` IPv4 주소

→ `--ip6` IPv6 주소

→ `--alias` 

→ `--link` 다른 컨테이너에 대한 링크

- 네트워크에 대한 연결은 컨테이너를 시작할 때 지정할 수도 있다.
- `docker container run -itd --name=webapp --net=web-network nginx`

**네트워크 해제**

`docker network disconnet 네트워크 컨테이너`

**네트워크 상세 정보 확인**

`docker network inspect [옵션] 네트워크`

**네트워크 삭제**

`docker network rm [옵션] 네트워크`

네트워크를 삭제는 disconnect 명령을 사용하여 네트워크 해제 후 사용할 수 있다.

## 가동 중인 Docker 컨테이너 조작

**가동 컨테이너 연결**

`docker container attach`

→ `Ctrl + P` 컨테이너 내부의 프로세스 종료

→ `Ctrl + C` 연결한 컨테이너 종료

**가동 컨테이너에서 프로세스 실행**

`docker container exec [옵션] <컨테이너 식별자> <실행할 명령>`

→ 백그라운드에서 실행될 경우 docker container attach로 쉘이 작동하지 않는 경우가 있다.

→ 해당 경우를 해결하기 위해서 사용.

→ 해당 명령어는 실행 중인 컨테이너에서만 사용할 수 있다.

→ `-it` 컨테이너의 표준 입력을 열고 단말 디바이스를 사용

**가동 컨테이너에서 프로세스 확인**

`docker container top`

실행 중인 프로세스의 pid, user, 실행 중인 명령이 표시된다.

**가동 컨테이너의 포트 전송 확인**

`docker container port`

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/12.png?raw=true)

![](https://github.com/Danpatpang/docker-Study/blob/master/part4/img/13.png?raw=true)

**컨테이너 이름 변경**

`docker container rename <old> <new>`

**컨테이너 안의 파일 복사**

`docker container cp <컨테이너 식별자>:<컨테이너 내부 파일 경로> <호스트의 파일 경로>`

`docker container cp <호스트의 파일> <컨테이너 식별자>:<컨테이너 내부 파일 경로>`

→ 컨테이너와 호스트 간의 파일을 복사할 때 사용

**컨테이너 조작의 차이점 확인**

`docker container diff <컨테이너 식별자>`

→ 이미지로부터 생성되었을 때와 차이점을 확인

→ `A` 추가된 파일

→ `D` 삭제된 파일

→ `C` 수정된 파일

## Docker 이미지 생성

**컨테이너로부터 이미지 작성**

`docker container commit [옵션] <컨테이너 식별자> [이미지명 [:태그명]]`

→ `-a` 작성자 지정

→ `-m` 메시지 지정

→ `-c` 커밋 시 도커 명령 지정

→ `-p` 컨테이너를 일시 정지하고 커밋

**컨테이너를 tar 파일로 출력**

Docker에서는 가동 중인 컨테이너의 디렉토리를 모아서 tar 파일로 출력할 수 있다.

`docker container export <컨테이너 식별자>`

`docker container export webserver > webserver_latest.tar`

→ 파일 추출

`tar -tf webserver_latest.tar`

→ 파일 상세 정보 확인

**tar 파일로부터 이미지 작성**

`docker image import <파일 or URL> | - [이미지명[:태그명]]`

→ import 명령으로는 하나의 파일만 실행되므로 이미지가 여러 개일 경우 합쳐 놓아야 한다.

→ root 권한으로 실행할 것.

**이미지 저장**

`docker image save [옵션] <저장 파일명> [이미지명]`

`docker image save -o tensorflow.tar tensorflow`

**이미지 읽어 들이기**

`docker image load [옵션]`

`docker image load -i tensorflow.tar`

=====

export / import와 save / load의 차이

→ save는 대상이 이미지, export는 대상이 컨테이너

=====

**불필요한 이미지 / 컨테이너 삭제**

`docker system prune [옵션]`

→ `-a` 사용하지 않는 리소스 모두 삭제

→ `-f` 강제 삭제