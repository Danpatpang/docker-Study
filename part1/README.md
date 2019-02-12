# part1

기존의 시스템 개발

- 고객의 업무 요구사항 이해
- 요구사항에 맞춰 설계
- 프로그래밍 언어를 사용하여 시스템 구축
- 사양대로 기능이 구현되었는지 테스트

오늘날

- 네트워크나 OS의 도입
- 시스템과 데이터베이스 서버와 같은 미들웨어의 설계
- 운용 관리와 같은 인프라 구축의 기초 지식 및 구성 관리

**인프라 엔지니어, 오퍼레이터의 역할 → 애플리케이션 엔지니어**

*업무 연속성 계획*

재해와 같은 리스크 발생 시 목표로 하는 복구 기간 안에 중요한 기능을 복구시켜 피해를 최소한으로 하기 위해 미리 준비해 두는 계획.

*오버레이 네트워크 (가상 네트워크)*

[https://atthis.tistory.com/6](https://atthis.tistory.com/6)

물리 네트워크 위에 성립되는 가상의 컴퓨터 네트워크.

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/16.png?raw=true)

## 시스템 기반의 기초 지식

Docker는 애플리케이션 실행 환경을 작성 및 관리하기 위한 플랫폼.

- 애플리케이션 엔지니어 / 인프라 엔지니어 구분이 있었으나 클라우드의 등장으로 경계가 허물어짐. (둘 다 이해할 줄 알아야 한다!!!)

*(인프라 vs 플랫폼 ??)*

**시스템 기반의 구성 요소**

1. 기능 요구사항 : 시스템의 기능으로서 요구되는 사항
2. 비기능 요구사항 : 시스템의 성능, 신뢰성, 확장성, 운용성, 보안 등과 같은 요구사항

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/7.png?raw=true)

- 하드웨어 : 시스템 기반을 구성하는 물리적인 요소 (건물 공조, 보안, 소화 설비 등도 포함)
- 네트워크
- OS : 하드웨어나 네트워크 장비를 제어하기 위한 기본 소프트웨어 (리소스, 프로세스 관리)
- 미들웨어 : 서버가 특정 역할을 하기 위한 기능을 갖고 있는 소프트웨어

**클라우드와 온프레미스**

1. 온프레미스

    자사에서 시스템 구축부터 운용까지를 모두 수행.

    초기 시스템 투자에 드는 비용이 크며, 확장이 어려움.

	![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/18.png?raw=true)

2. public 클라우드

    불특정 다수에게 제공되는 클라우드 서비스.

    이용한 시간이나 데이터 양에 따라 요금을 지불.

    IaaS(인프라 - 시스템 기반 부분) / PaaS(플랫폼) / SaaS(서비스)

	![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/17.png?raw=true)

3. private 클라우드

    특정 기업에만 제공되는 클라우드 서비스.

    보안을 확보하기 쉬우며, 독자적인 기능이나 서비스 추가가 쉬움.

    ![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/9.png?raw=true)

**클라우드가 적합한 케이스**

- 트래픽의 변동이 많은 시스템 (예약, 쇼핑몰)
- 재해 대책으로 해외에 백업을 구축하고 싶은 시스템 (업무 연속성 계획)
- 서비스를 빨리 제공하고 싶은 시스템

**온프레미스가 적합한 케이스**

- 높은 가용성이 요구되는 시스템
- 기밀성이 높은 데이터를 다루는 시스템
- 특수한 요구사항이 있는 시스템

- 어떤 시스템을 온프레미스에 남기고 클라우드를 사용할지 판단하는 것이 중요!!

**시스템 기반의 구축 / 운용 흐름**

폭포형 개발 : 정해진 순서대로 진행

애자일 개발 : 각 단계를 작은 단위로 나눠 반복하면서 개발

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/15.png?raw=true)

- 애플리케이션 개발의 경우 릴리스 후에는 버그 수정, 추가 기능이 개발 메인이므로 개발 인원이 줄어들지만, 인프라의 경우 감시, 보안, 업그레이드 등 많은 부분을 신경써야 한다.
- 이 시스템 운용에 걸리는 유지보수 기간을 가능한 줄이고 안정화 시키는 게 중요!
- 자동화할 수 있는 부분은 가능한 한 자동화하자!

## 하드웨어 / 네트워크 기초 지식

**서버 장비**

- CPU
- 메모리
- 스토리지 (대부분 다중화)
- 클라우드 하드웨어의 선정 = 가상 머신의 사양 선정
- 클라우드의 경우 시험 삼아 가볍게 사용할 수 있지만, 제공되는 서비스가 너무 많아서 고민

**네트워크 주소**

- 물리 주소 : 네트워크 부품에 물리적으로 할당되는 주소.
- IP 주소 : 인터넷이나 인트라넷에 연결된 장비에 할당되는 식별 번호.

**OSI 참조 모델과 통신 프로토콜**

OSI 참조 모델이란 국제표준화기구(ISO)가 책정한 컴퓨터의 통신 기능을 계층 구조로 나눈 개념 모델.

1계층 ~ 4계층 : 하위 계층 / 5계층 ~ 7계층 : 상위 계층

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/8.png?raw=true)

- 응용 계층

    웹의 HTTP나 메일 전송을 하는 SMTP 등과 같은 애플리케이션 특화 프로토콜

- 표현 계층

    데이터의 저장 형식, 압축, 문자 인코딩과 같은 데이터의 표현 형식을 규정

- 세션 계층

    커넥션, 데이터 전송 타이밍 규정 (request / response로 구성)

- 전송 계층

    데이터 전송을 제어(TCP, UDP)

    전송 오류의 검출이나 재전송을 규정(데이터를 상대 노드로 확실히 보내는 역할)

- 네트워크 계층

    서로 다른 네트워크 간에 통신을 하기 위한 규정(라우팅)

    - 서로 다른 네트워크에 데이터 패킷을 전송하는 것을 라우팅이라고 한다.
    - 패킷을 어디에서 어디로 전송할지에 대한 정보는 라우팅 테이블이 가지고 있다.
- 데이터 링크 계층

    동일한 네트워크 안에 있는 노드간의 통신을 규정

- 물리 계층

    통신 장비의 물리적 전기적 특성을 규정(전압, 전류, 커넥터의 모양)

**방화벽**

내부 네트워크와 외부와의 통신을 제어하고, 내부 네트워크의 안전을 유지하기 위한 기술.

- 보안을 확보하기 위한 가장 좋은 방법은 불필요한 통신을 차단하는 것.

1. 패킷 필터형 : 통과하는 패킷을 포트 번호나 IP주소를 바탕으로 필터링(80번 - http)
2. 애플리케이션 게이트웨이형 : 프로토콜 레벨에서 외부와의 통신을 제어(프록시 서버)

**라우터 / 레이어 3 스위치**

라우터 : 2개 이상의 서로 다른 네트워크 간을 중계하기 위한 통신 장비.

레이어 3 스위치 : 라우팅을 하드웨어로 처리하므로 고속으로 작동.

3계층인 네트워크 계층에서 작동하며 어떤 루트를 통해 데이터를 전송할지 판단.

1. 정적 경로 : 라우팅 테이블을 바탕으로 정해진 경로
2. 동적 경로 : 라우팅 프로토콜에서 설정된 경로

## OS(Linux) 기초 지식

1991년 핀란드의 Linus Torvalds가 개발한 Unix 호환 서버 OS.

보안에 뛰어나며 안정적으로 작동된다는 특징을 갖고 있음. (서버용)

최근에는 스마트폰이나 임베디드 장비의 OS로서 작동.

- Linux 커널

    OS의 코어가 되는 부분

- Linux 배포판

    Linux 커널과 함께 userland를 포함

    - userland에서는 디바이스에 직접 액세스할 수 없기 때문에 커널을 통해 처리가 이루어진다.

	![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/2.png?raw=true)

	![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/6.png?raw=true)

**Linux 커널**

하드웨어 제어에 관한 OS의 핵심 기능, C와 어셈블리어로 쓰여진다.

- 디바이스 관리
- 프로세스 관리
- 메모리 관리

Linux 커널을 조작하기 위해서 Shell을 사용한다.

쉘은 사용자의 명령을 커맨드로 받아, 그것을 Linux 커널에 전달한다.

명령을 모아 텍스트 파일에 기술한 것을 쉘 스크립트라고 한다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/21.png?raw=true)
![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/22.png?raw=true)

**Linux 파일 시스템**

보통 컴퓨터가 데이터를 읽어 들일 때 데이터가 어떤 디스크에 어떻게 저장되어 있는지를 의식해야 한다.

하지만 데이터를 이용하는 애플리케이션의 입장에서 보면 데이터가 어디에 저장되어 있든지 상관없는 것이 좋다.

- Linux는 VFS(가상 파일 시스템) 장치를 사용해 각 디바이스를 파일로 취급한다.

	![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/20.png?raw=true)

- ext2 : Linux 운영체제에서 널리 이용되던 파일 시스템.
- ext3 : Linux에서 주로 사용되는 파일 시스템.
- ext4 : exr3의 후속 저널링 파일 시스템.
- tmpfs : Unix 계열 OS에서 임시 파일을 위한 장치.
- UnionFS : 여러 개의 디렉토리를 하나의 디렉토리로 취급.

**Linux 디렉토리 구성**

Linux는 커널을 비롯하여 각종 커맨드나 설정 파일이 디렉토리에 배치.

디렉토리 목록은 FHS라는 규격에 의해 표준화되어 있다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/14.png?raw=true)

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/12.png?raw=true)

**Linux  보안 기능**

1. 계정에 대한 권한 설정
2. 네트워크 필터링을 사용한 보안 기능
3. SELinux : 미국 국가안전보장국이 제공하는 강제 액세스 제어 기능

## 미들웨어 기초 지식

미들웨어 :  OS와 업무 처리를 수행하는 애플리케이션 사이에 들어가는 소프트웨어.

(OS가 갖고 있는 기능 확장, 서버 기능 제공, 애플리케이션 공통 기능 제공 등)

**웹 서버 / 웹 애플리케이션 서버**

- 클라이언트의 요청에 따라 응답으로 반환하거나 다른 서버를 호출함.

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/3.png?raw=true)

**데이터베이스 서버**

- 시스템이 생성하는 다양한 데이터를 관리하기 위한 미들웨어.

    관계형 데이터베이스

	![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/10.png?raw=true)

    비관계형 데이터베이스

    - 병렬분산처리나 유연한 스키마 설정으로 대량의 데이터 축적, 병렬 처리에 뛰어남.
    - 다수의 사용자 액세스를 처리해야 할 때 주로 사용.

    ![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/19.png?raw=true)

**시스템 감시 툴**

시스템의 감시 대상인 서버나 장비의 상태를 감시하여 정해진 액션을 실행하는 것.

- 시스템을 안정적으로 가동시키기 위해 시스템 관리자는 시스템이 어떤 상태로 가공되고 있는지 감시해야 한다.

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/13.png?raw=true)

- 미들웨어의 특징이나 기능에 따라 요구사항에 맞게 조합하여 이용할 줄 알아야 함.

## 인프라 구성 관리 기초 지식

**인프라 구성 관리**

인프라를 구성하는 하드웨어, 네트워크, OS 등의 정보를 관리하고 적절한 상태로 유지하는 작업.

온프레미스 : 인프라 환경의 규모가 커질수록 부하가 커진다.(유지보수)

클라우드 :  인프라 구축에 물리적인 제약 X.(파기 후 새로 생성)

**코드를 사용한 구성 관리**

온프레미스 : 여러 대의 서버를 한 대씩 수작업으로 설정해야 함.

(버전 업이나 보안 패치 적용을 효율적으로 하기 힘들다.)

→ 프로그램 코드에 적힌 내용대로 자동으로 설정해주는 장치 도입.

(이미지를 통해서 인프라 구축)

![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/4.png?raw=true)

- 이와 같은 기술을 Infrasturucture as Code라고 한다.

**대표적인 인프라 구성 관리 툴**

- OS의 시작을 자동화
- OS나 미들웨어 설정을 자동화
- 여러 서버의 관리를 자동화 (Kubernetes)

**지속적 인티그레이션 / 지속적 딜리버리**

- 지속적 인티그레이션

    코드 추가 및 수정 시마다 단위 테스트를 실행하고 확실하게 작동하는 코드를 유지하는 방법.

	![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/1.png?raw=true)

- 지속적 딜리버리

    폭포형 개발 → 애자일 개발

	![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/11.png?raw=true)

- 배포까지 시간이 오래 걸린다.

	![](https://github.com/Danpatpang/docker-Study/blob/master/part1/img/5.png?raw=true)

- 짧은 사이클의 개발과 릴리즈를 반복

    (개발 기간이 짧으므로 문제가 발생할 확률이 크다)

→ 디플로이먼트 사용 (현재 작동 릴리즈 / 버전업 후의 릴리즈)