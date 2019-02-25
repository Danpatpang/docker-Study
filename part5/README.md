# part5

## Dockerfile을 사용한 구성 관리

**Dockerfile이란?**

Docker 상에서 작동시킬 컨테이너의 구성 정보를 기술하기 위한 파일.

1. 베이스가 될 Docker 이미지
2. Docker 컨테이너 안에서 수행한 명령
3. 환경변수 등의 설정
4. Docker 컨테이너 안에서 작동시킬 데몬 실행

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/1.png?raw=true)

Dockerfile은 텍스트 형식의 파일로 확장자는 필요 없다.

`명령 인수`

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/2.png?raw=true)

Dockerfile에 주석을 쓰는 경우는 맨 앞에 `#`을 붙인다.

**Dockerfile 작성**

Dockerfile에는 Docker 컨테이너를 어떤 Docker 이미지로부터 생성할지를 반드시 기술해야 한다.

`FROM [이미지명]`

`FROM [이미지명]:[태그명]`

`FROM [이미지명]@[다이제스트]`

→ FROM 명령은 필수 항목이며 태그명 생략 시 최신 버전이 적용된다.

## Dockerfile의 빌드와 이미지 레이어

Dockerfile을 빌드하면 Dockerfile에 정의된 구성을 바탕으로 한 Docker 이미지를 작성할 수 있다.

**Dockerfile로부터 Docker 이미지 만들기**

`docker build -t [생성할 이미지명]:[태그명] [Dockerfile의 위치]`

`docker build -t sample:1.0 /home/docker/sample`

→ -t 옵션은 dockerfile에 이름을 지정할 수 있다.

→ -f 옵션은 파일 확장자를 지정할 수 있다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/3.png?raw=true)

베이스 이미지와 작성한 이미지의 이미지 ID가 동일하다.

→ 각각 다른 이름이 붙어 있지만 실체는 모두 동일한 이미지.

`docker build - < Dockerfile`

→ 표준 입력을 경유하여 Dockerfile을 지정하여 빌드.

→ Docker는 이미지를 빌드할 때 자동으로 중간 이미지를 생성한다.

**Docker 이미지의 레이어 구조**

Dockerfile을 빌드하여 이미지를 작성하면 Dockerfile의 명령별로 이미지를 작성한다.

작성된 여러 개의 이미지는 레이어 구조로 되어있다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/4.png?raw=true)

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/5.png?raw=true)

→ 작성한 이미지는 다른 이미지와 공유되므로 공통 베이스 이미지를 바탕으로 여러 개의 이미지를 작성할 경우, 베이스 이미지의 레이어가 공유된다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/6.png?raw=true)

- Docker Hub에서도 이미지를 레이어로 겹쳐서 작성함으로써 공통되는 이미지는 공유하여 관리한다.

## 멀티스테이지 빌드를 사용한 애플리케이션 개발<예제>

개발 환경에서 사용한 라이브러리나 툴이 제품 환경에서 반드시 사용되는 것은 아니다.

제품 환경에서는 애플리케이션 실행을 위해 최소한의 필요 모듈만 배치하는 것이 리소스, 보안 관점에서 효율적이다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/7.png?raw=true)

1. Dockerfile 만들기

    ![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/8.png?raw=true)

    → 개발 환경 : Go 언어 기반, 바이너리 파일 작성

    → 제품 환경 : Linux 명령 실행, `--from`으로 복사 실행

2. Docker 이미지 빌드

    `docker build -t greet .`

    golang 다운로드 ⇒ builder 생성 ⇒ 바이너리 파일 생성 ⇒ 복사

    ![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/9.png?raw=true)

3. Docker 컨테이너 시작

    `docker container run -it --rm greet asa`

    `docker container run -it --rm greet --lang=es asa`

## 명령 및 데몬 실행

**명령 실행**

환경 구축을 위한 명령을 실행할 때 RUN을 사용

`RUN [실행하고 싶은 명령]`

1. Shell 형식

    `RUN apt-get install nginx`

    Docker 컨테이너에서 실행할 기본 쉘을 변경하고 싶을 때 사용.

2. Exec 형식

    `RUN ["/bin/bash", "-c", "apt-get install -y nginx"]`

    실행하고 싶은 명령을 JSON 배열로 지정.

    쉘을 경유하지 않고 직접 실행한다.

 

- RUN 명령은 Dockerfile에 여러 개 기술할 수 있으며 순차적으로 실행된다.

`docker history`

이미지 생성 시 어떤 명령이 실행되었는지 확인.

Dorkerfile을 빌드하면 기술된 명령마다 내부 이미지가 하나씩 작성된다.

생성되는 이미지의 갯수를 줄이기 위해서 한 줄에 쓸 수 있는 명령은 한줄에 작성하는 것이 좋다.

**데몬 실행(CMD)**

이미지를 바탕으로 생성된 컨테이너 안에서 명령을 실행하려면 CMD를 사용.

Dockerfile에는 하나의 CMD 명령을 기술할 수 있다.(여러 개일 경우 마지막만 유효)

`CMD [실행하고 싶은 명령]`

1. Shell 형식

    `CMD ngingx -g 'daemon off;'`

    RUN 명령의 구문과 동일하다.

2. Exec 형식

    `CMD ["nginx", "-g", "daemon off;"]`

    RUN 명령의 구문과 동일하다.

3. ENTRYPOINT 명령의 파라미터로 기술

**데몬 실행(ENTRYPOINT)**

ENTRYPOINT 명령에서 지정한 명령은 Dockerfile에서 빌드한 이미지로부터 Docker 컨테이너를 시작하기 때문에 `docker container run` 명령을 실행했을 때 실행된다.

`ENTRYPOINT [실행하고 싶은 명령]`

1. Shell 형식

    `ENTRYPOINT ngingx -g 'daemon off;'`

2. Exec 형식

    `ENTRYPOINT ["nginx", "-g", "daemon off;"]`

- CMD 명령과 ENTRYPOINT 명령은 `docker container run`의 차이가 있다.
- CMD의 경우 컨테이너 시작 시에 실행하고 싶은 명령을 정의해도 `docker container run` 명령에서의 내용을 우선 실행.
- ENTRYPOINT는 `docker container run`에서 실행.
- ENTRYPOINT와 CMD 명령은 조합해서 사용할 수 있음. (CMD 명령은 `docker container run` 명령 실행 시에 덮어 쓸 수 있다.)

**빌드 완료 후에 실행되는 명령**

ONBUILD 명령은 다음 빌드에서 실행할 명령을 이미지 안에 설정하기 위한 명령.

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/10.png?raw=true)

`ONBUILD [실행하고 싶은 명령]`

자신의 Dockerfile로부터 생성한 이미지를 베이스 이미지로 한 다음 Dockerfile을 빌드할 때 실행하고 싶은 명령을 기술.

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/11.png?raw=true)

1. 베이스 이미지 작성 후 빌드
2. 웹 콘텐츠 개발
3. 웹 서버용 이미지 작성
4. 웹 서버용 컨테이너 시작

- 인프라 구축과 관련된 이미지 작성과 애플리케이션 전개와 관련된 이미지 생성을 나눌 수 있다.
- dicker image inspect 명령으로 이미지에 ONBULID 명령이 있는지 확인할 수 있다.

**시스템 콜 시그널의 설정**

`STOPSIGNAL [시그널]`

컨테이너를 종료할 때에 송신하는 시그널을 설정.

**컨테이너의 헬스 체크 명령**

`HEALTHCHECK [옵션] CMD 실행할 명령`

컨테이너 안의 프로세스가 정상적으로 작동하고 있는지 확인.

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/12.png?raw=true)

`HEALTHCHECK --interval=5m --timeout=3s CMD curl -f http://localhost || exit 1`

→ 5분마다 가동 중인 웹 서버의 메인 페이지를 3초 안에 표시할 수 있는지 확인

→ 결과는 `docker container inspect`로 확인

## 환경 및 네트워크 설정

**환경 변수 설정**

Dockerfile 안에서 환경변수를 설정하고 싶을 때 사용.

1. `ENV [key] [value]`

    단일 환경변수에 하나의 값을 설정.

    key 이후로는 모두 문자여로 취급(", ', 공백 포함)

    ![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/13.png?raw=true)

    ![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/14.png?raw=true)

2. `ENV [key]=[value]`

한번에 여러 개의 값을 설정.

변수앞에 \를 추가하면 이스케이프로 처리된다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part5/img/15.png?raw=true)

**작업 디렉토리 지정**

`WORKDIR [작업 디렉토리 경로]`

→ Dockerfile에 쓰여 있는 명령을 실행하기 위한 작업용 디렉토리 설정.

(RUN, CMD, ENTRYPOINT, COPY, ADD)

→ WORKDIR 명령은 Dockerfile 안에서 여러 번 사용할 수 있으며, 상대 경로를 지정한 경우 이전 WORKDIR 명령의 경로에 대한 상대 경로로 인식.

→ ENV 명령에서 지정한 환경변수를 사용할 수 있다.

**사용자 지정**

`USER [사용자명/UID]`

USER 명령에서 지정하는 사용자는 RUN 명령으로 미리 작성해야 한다.

**라벨 지정**

`LABEL <key>=<value`

이미지에 버전, 작성자, 코멘트 등과 같은 정보를 제공할 때 사용.

`docker image inspect` 명령으로 라벨을 확인할 수 있다.

- `MAINTAINER`보다 `LABEL` 사용을 권장

**포트 설정**

`EXPOSE <포트 번호>`

컨테이너의 공개 포트 번호를 지정.

Docker에게 실행 중인 컨테이너가 listen하고 있는 네트워크를 알려준다.

**Dockerfile 내 변수 설정**

`ARG <이름>[=기본값]`

Dokerfile 안에서 사용할 변수를 정의할 때 사용.

변수의 값에 따라 생성되는 이미지의 내용을 바꿀 수 있다.

`docker build . --build-arg <key>=<value>`

**기본 쉘 설정**

쉘을 설정하지 않으면 Linux는 "/bin/sh", Windows는 "cmd"가 된다.

`SHELL ["쉘의 경로", "파라미터"]`

## 파일 설정

**파일 및 디렉토리 추가**

`ADD <호스트의 파일 경로> <Docker 이미지의 파일 경로>`

`ADD ["<호스트의 파일 경로>" "<Docker 이미지의 파일 경로>"]`

→ ADD 명령은 호스트상의 파일이나 디렉토리, 원격 파일을 Docker 이미지 안으로 복사한다.

→ WORKDIR 명령에서 지정한 디렉토리를 기점으로 한다.

→ ADD 명령은 인증을 지원하지 않으므로 다운로드 인증이 필요한 경우 wget, crul을 사용한다.

**파일 복사**

`COPY <호스트의 파일 경로> <Docker 이미지의 파일 경로>`

`COPY ["<호스트의 파일 경로>" "<Docker 이미지의 파일 경로>"]`

→ ADD 명령과 COPY 명령은 매우 비슷하다.

→ ADD는 원격 파일의 다운로드나 아카이브 압축 해제 등과 같은 기능을 갖고 있지만, COPY는 호스트상의 파일을 이미지 안으로 복사 처리만 한다.

**볼륨 마운트**

`VOLUME ["/마운트 포인트"]`

컨테이너는 영구 데이터를 저장하는 데 적합하지 않으므로 영구 저장이 필요한 데이터는 로컬에 저장하는 것이 좋다.

- 영구 데이터는 호스트 또는 공유 스토리지 볼륨으로 마운트 하는 것이 가능하다.