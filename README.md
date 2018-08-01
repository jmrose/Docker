# Docker - Tutorial

## Install
  
* Docker 설치하기  
$ curl -s https://get.docker.com/ | sudo sh    
<code>_docker는 root 권한이 필요. root가 아닌 사용자에게 권한주기 - sudo 없이 사용하기_</code>  
$ sudo usermod -aG docker $USER 
$ sudo usermod -aG docker $(whoami)  
$ docker version  
$ docker -v  
$ docker-compose -v  

* Docker 저장소 GPG key 추가  
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 8118E89F3A912897C070ADBF76221572C52609D   

* Docker 저장소 추가  
$ echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee /etc/apt/sources.list.d/docker.list  
$ sudo apt-get update  
$ sudo apt-get install -y docker-engine  
$ sudo apt-get install -y docker-compose


## Uninstall
_어떤 이유로 도커를 삭제하시고자 한다면 아래 처럼 간단히 삭제 할 수 있습니다._  
$ sudo apt-get autoremove --purge docker-engine  
$ sudo rm -fR /var/lib/docker  

_사용자가 만든 디렉토리/파일은 사용자가 직접 삭제해야 합니다._
  
  
<br/><br/>

## 명령어

* 이미지 가져오기  
$ docker pull ubuntu

* 컨테이너 실행하기  
_docker 이미지 run을 통해 컨테이너 생성과 함께 컨테이너 시작. 마지막엔 실행시킬 이미지명:태그를 입력합니다._  
docker run <옵션> <이미지 이름> <실행할 파일>  
$ docker run -it -p 80:80 -p 443:443 -p 8080:8080 --name dev-web dev-web:0.4  

|옵션|설명|
|---|---|
|-d|  detached mode 흔히 말하는 백그라운드 모드|
|-p|	호스트와 컨테이너의 포트를 연결 (포워딩)|
|-v|	호스트와 컨테이너의 디렉토리를 연결 (마운트)|
|-e|	컨테이너 내에서 사용할 환경변수 설정|
|–name|	컨테이너 이름 설정|
|–rm|	프로세스 종료시 컨테이너 자동 제거|
|-it|	-i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션|
|–link|	컨테이너 연결 [컨테이너명:별칭]|

* 최신 Ubuntu image를 데몬으로 실행하기  
$ docker run -dit --name myUbuntu ubuntu:latest  
$ docker run -it -p 8080:3000 -v /home  
  
* 이미지를 컨테이너로 실행하며 셸로 이동  
$ docker run --rm -it [이미지명] /bin/zsh  
  
* 쉘에서 종료하지 않고 빠져나오기  
$ ctrl + q + p

  
* 특정 포트로 연결하여 실행시키기  
$ docker run --rm -it -p <외부포트>:<컨테이너포트> <이미지명> <실행할 명령>  
  
* 현 디렉토리 내의 Dockerfile을 이용해 Image Build  
$ docker build . -t [이미지이름(태그명)]  
$ docker build . -t jmrose/nginx-python  
// DockerFile을 지정하여  
$ docker build . -t [이미지태그명] -f [파일명]  
$ docker build . -t jmrose/nginx-python -f Dockerfile.base  

* Build 된 Image를 DockerHub에 올리기  
$ docker push jmrose/nginx-python

* 생성된 image 목록 보기  
$ docker images -a  
  
* 이미지 삭제  
$ docker rmi Image  
_Delete name docker images force_  
$ docker rmi -f $(docker images --filter "dangling=true" -q)  
  
* 패턴으로 이미지 삭제  
$ docker images -a | grep "pattern" | awk '{print $3}' | xargs docker rmi  
  
* 전체 이미지 삭제  
$ docker rmi $(docker images -a -q)  
$ docker rmi $(docker images -q)  
  
##### Container 명령어

* List  
$ docker ps -a  
  
* Stop  
$ docker stop $(docker ps -a -q)

* Start  
$ docker start $(docker ps -a -q)  
  
* remove all  
$ docker rm $(docker ps -a -q)  
  
* remove one  
$ docker rm docker_ode_servcer
  
* 컨테이너에 명령 실행  
$ docker exec [container id] [실행할 명령]  
$ docker exec -it  c456623003b1 -c xxx  
  
* 실행중인 컨테이너에 접속 후 셸 실행하고 싶을 경우  
$ docker exec -it [container id] /bin/zsh  
$ docker exec -it  c456623003b1 /bin/zsh  
  
##### Volume ( 참조 : [V](https://www.joinc.co.kr/w/man/12/docker/Guide/DataWithContainer) )
* 볼륨 목록 확인  
$ docker volume ls   
  
* 볼륨 삭제  
$ docker volume rm xxxxxxxxxxxxx  
  
* 볼륨 정보 확인
$ docker volume inspect xxxxxxxxxx  
  
  
 <br/><br/>
## Error 모음

<pre>
docker 이미지 삭제 시 에러 :
# docker rmi 013aeeba503d
Error response from daemon: conflict: unable to delete 013aeeba503d (must be forced) - image is referenced in multiple repositories

강제로 삭제하기
# docker rmi -f c9d990395902
</pre>

<pre>
Docker를 ubuntu에 설치하고 실행하려 합니다. 일반 계정으로 접속 후 현재 상태를 보려고 다음 명령을 실행하면 권한오류가 발생합니다.

ubuntu:~$ docker ps -a

Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.27/containers/json?all=1: dial unix /var/run/docker.sock: connect: permission denied

ubuntu:~$

기본적으로 docker라는 그룹에서 동작하기 때문에 일반 계정으로 접근하려고 하면 권한오류가 발생합니다.

그래서 대부분 sudo 명령을 이용하여 실행하기도 합니다.

해결방법
해결방법은 단순합니다. 현재 계정에 "docker"라는 그룹을 추가해주면 됩니다.
ubuntu:~$ sudo gpasswd -a ${USER} docker
[sudo] password for oofbird: 
Adding user oofbird to group docker
ubuntu:~$
그 후 docker를 재기동 한 뒤 재접속 또는 "newgrp docker" 명령을 하시면 됩니다.

ubuntu:~$ sudo service docker restart



재 접속 또는

ubuntu:~$ newgrp docker



정상동작을 확인하기 위해

ubuntu:~$ docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES



sudo gpasswd -a jmrose party01

그룹의 암호를 설정합니다.

이 암호는 그룹에 포함되지 않는 사용자가 그룹으로 로그인(newgrp)하기 위해서 사용됩니다.

디렉토리 권한
sudo setfacl -m d:g:infose:wrx infose

http://www.linux4u.co.kr/RedhatAS/s1-acls-setting-default.html
</pre>


