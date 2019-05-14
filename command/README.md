# 명령어 정리

디스크 현황

    docker system df

실행 환경 확인

    docker system info

---

이미지 다운로드

    docker pull <image>

이미지 확인

    docker image ls

이미지 삭제

    docker rmi <image>
    # -f를 붙이면 컨테이너도 같이 삭제
    docker rmi -f <image>



---

컨테이너 확인

    # 동작중인 컨테이너 확인
    docker ps
    # 모든 컨테이너 확인
    docker ps -a

컨테이너 삭제

    docker rm <container>
    # 볼륨과 함께 삭제
    docker rm -v <container id>

컨테이너 모두 삭제

    docker rm `docker ps -a -q`

---

### 컨테이너가 중지된다고 고정 장치 볼륨이 자동으로 제거되지는 않는다.

볼륨 삭제

    # 볼륨의 경우 volume name을 모두 적어야 한다.
    docker volume rm <vloume full name>
    # 모든 볼륨 삭제
    docker volume prune

볼륨 확인

    docker volume ls

---

컨테이너 실행

    docker container run --name <name> <image>

컨테이너 상태 확인

    docker container stats

컨테이너 중지

    docker stop <name>

컨테이너 시작

    docker start <name>