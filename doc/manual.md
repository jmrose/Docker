
```docker

이미지 가져오기
$ docker pull ubuntu

이미지 가져오면서 생성하기
docker run <옵션> <이미지 이름> <실행할 파일>

docker 이미지 run을 통해 컨테이너 생성과 함께 컨테이너 시작. 마지막엔 실행시킬 이미지명:태그를 입력합니다.

-i(interactive), -t(Pseudo-tty) 옵션을 사용하면 실행된 Bash 셸에 입력 및 출력을 할 수 있습니다. 
--name 옵션으로 컨테이너의 이름을 지정할 수 있습니다. 이름을 지정하지 않으면 Docker가 자동으로 이름을 생성하여 지정합니다.
-d 옵션은 컨테이너를 백그라운드로 실행시키는 옵션입니다. 컨테이너 접속 후 해제하여도 컨테이너가 종료되지 않게 합니다.
-p 옵션으로 호스트 포트와 컨테이너 포트를 연결 합니다. -p 옵션을 추가하여 여러개의 포트를 연결할 수 있습니다. -p :
–rm 프로세스 종료시 컨테이너 자동 제거
-v 호스트와 컨테이너의 디렉토리를 연결 (마운트)
-e 컨테이너 내에서 사용할 환경변수 설정

$ docker run -i -t -d -p 80:80 -p 443:443 -p 8080:8080 --name dev-web dev-web:0.4

ubuntu image를 데몬으로 실행하기
$ docker run -dit --name myUbuntu ubuntu:latest

$ docker run -it -p 8080:3000 -v /home

이미지를 컨테이너로 실행하며 셸로 이동
$ docker run --rm -it [이미지명] /bin/zsh
>>>> 쉘에서 종료하지 않고 빠져나오기 
>>>> ctrl + q + p


이미지로 컨테이너를 실행하며 포트 연결
$ docker run --rm -it -p <외부포트>:<컨테이너포트> <이미지명> <실행할 명령>

현재 위치를 기준으로 Dockerfile을 이용해 새 이미지 생성
$ docker build . -t [이미지이름(태그명)]
$ docker build . -t jmrose/nginx-python

현재 위치를 기준으로 특정 파일을 이용해 새 이미지 생성
$ docker build . -t [이미지태그명] -f [파일명]
$ docker build . -t jmrose/nginx-python -f Dockerfile.base


실행중인 컨테이너에 명령 실행
$ docker exec [container id] [실행할 명령]
$ docker exec -it  c456623003b1 -c xxx

실행중인 컨테이너에 접속 후 셸 실행하고 싶을 경우
$ docker exec -it [container id] /bin/zsh
$ docker exec -it  c456623003b1 /bin/zsh
