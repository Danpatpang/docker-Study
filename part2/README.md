# part2

Docker는 컨테이너 기술을 사용하여 애플리케이션의 실행 환경을 구축 및 운용하기 위한 플랫폼이다.

실행에 필요한 것을 하나로 모아 Docker 이미지로 관리함으로써 애플리케이션의 이식성을 높일 수 있다.

## 컨테이너 기술의 개요

**컨테이너**

호스트 OS상에 논리적인 구획(컨테이너)를 만들고, 애플리케이션을 작동시키기 위해 필요한 라이브러, 애플리케이션 등을 하나로 모아 별도의 서버인 것처럼 사용할 수 있게 만드는 것.

- 오버헤드가 적으므로 가볍고 빠르다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/1.png?raw=true)

- 보통 호스트 OS의 경우 여러 애플리케이션은 똑같은 시스템 리소스를 사용한다. 이때 작동하는 애플리케이션은 디렉토리를 공유하고 동일한 IP주소로 통신하므로 서로 다른 미들웨어나 라이브러리를 사용할 경우 각 애플리케이션에 영향을 주지 않도록 주의해야 한다.
- 이에 반해 컨테이너 기술은 시스템 자원을 마치 각 애플리케이션이 점유하고 있는 것처럼 보이게 한다.
- 컨테이너는 컨테이너 별로 모을 수 있기 때문에 여러 개의 컨테이너를 조합하여 하나의 애플리케이션을 구축할 수 있다.

=======================

**서버 가상화**

1. 호스트형 서버 가상화
    - 오버헤드가 커진다.

        ![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/2.png?raw=true)

2. 하이퍼바이저형 서버 가상화
    - 컨테이너 기술과 비슷하지만 이식성 부분에서 효율이 떨어짐.

    ![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/4.png?raw=true)

=======================

**컨테이너 역사**

- FreeBSD Jail

    Unix의 FreeBSD 기술로 시스템을 분할할 수 있다.

    - 프로세스의 구획화 : Jail 밖의 프로세스에는 영향 X
    - 네트워크의 구획화 : 각각 다른 IP 주소 부여
    - 파일 시스템의 구획화 : 조작할 수 있는 파일 제한
- Solaris Containers
    - zone 기능 : 하나의 OS 공간을 가상적으로 분할
    - 리스스 매니저 기능
- Linux Containers(LXC)

    Linux 상에서 사용하는 컨테이너 환경.

## Docker의 개요

Docker는 애플리케이션의 실행에 필요한 환경을 하나의 이미지로 모아두고, 해당 이미지를 사용하여 다양한 환경에서 애플리케이션 실행 환경을 구축 및 운용하기 위한 오픈소스 플랫폼이다.

**프로그래머에게 Docker란?**

![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/5.png?raw=true)

![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/6.png?raw=true)

인프라 환경을 컨테이너로 관리한다.

→ 애플리케이션의 실행에 필요한 모든 파일 및 디렉토리들을 컨테이너로 모아버린다. (Docker 이미지 작성)

→ 개발부터 테스트, 제품 환경까지 애플리케이션 엔지니어가 수행 가능

→ 변화에 강한 시스템을 구축(이식성이 강함)

![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/7.png?raw=true)

- 시스템 개발에서 애플리케이션의 실행 환경에 제약이 많으면 특정 업체에 의존하는 시스템이 되어버리거나 개발 속도가 떨어진다.

## Docker의 기능

**Docker 이미지를 만드는 기능(Build)**

Docker에서는 하나의 이미지에는 하나의 애플리케이션만 넣어 두고, 여러 개의 컨테이너를 조합하여 서비스를 구축하는 방법을 권장.

![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/8.png?raw=true)

Docker 이미지는 애플리케이션의 실행에 필요한 파일들이 저장된 디렉토리다.

Docker 이미지는 수동으로 만들 수 있으며, Dockerfile을 이용하여 자동으로 만들 수도 있다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/9.png?raw=true)

- Docker 이미지는 겹쳐서 사용할 수 있고, 이를 통해 새로운 이미지를 만들 수도 있다.
- Docker 구성에 변경이 있는 부분은 이미지 레이어로 관리된다.

**Docker 이미지를 공유하는 기능(Ship)**

Docker 이미지는 Docker 레지스트리(Docker Hub)에서 공유할 수 있다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/10.png?raw=true)

~~Docker Container Trust를 이용해서 이미지의 제공자를 검증할 수 있다.~~

**Docker 컨테이너를 작동시키는 기능(Run)**

Linux 상에서 컨테이너 단위로 서버 기능을 작동시킨다.

Docker 이미지만 있으면 Docker가 설치된 환경 어디서든지 컨테이너를 작동시킬 수 있다. (여러 개의 컨테이너 가동 가능)

Docker의 경우 이미 움직이는 OS 상에서 프로세스를 실행하므로 빠르다. (하나의 Linux 커널을 여러 개의 컨테이너에서 공유)

컨테이너 안에서 작동하는 프로세스를 하나의 그룹으로 관리하고, 그룹마다 각각 다른 파일, 네트워크를 할당한다. (그룹이 다르면 프로세스나 파일에 대한 접근 불가)

→ Linux 커널의 namespace, cgroups 기술로 구현됨.

![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/11.png?raw=true)

- 컨테이너 여러 대를 하나의 호스트에서 실행하는 것보다 여러 대의 호스트에서 역할에 맞춰 컨테이너를 실행(시스템 운용 설계)
- Docker는 '연도 2, 월 2'로 버전 표시.

**Docker 컴포넌트**

![](https://github.com/Danpatpang/docker-Study/blob/master/part2/img/12.png?raw=true)

1. Docker Engine(핵심 기능)

    Docker 이미지를 생성하고 컨테이너를 가동

2. Docker Registry(이미지 공유)
3. Docker Compose(컨테이너 일원 관리)

    여러 개의 컨테이너 구성 정보를 코드로 정의하고, 실행 환경을 구성하는 컨테이너들을 관리하기 위한 툴

4. Docker Machine(실행 환경 구축)

    Docker 실행 환경을 명령으로 자동 생성하기 위한 툴

5. Docker Swarm(클러스터 관리)

    Docker 호스트를 클러스터화하기 위한 툴

## Docker의 작동구조

컨테이너를 구획하하는 장치(napespace)

릴리즈 관리 장치(cgroups)

네트워크 구성(가상 bridge / 가상 NIC)

Docker 이미지의 데이터 관리 장치