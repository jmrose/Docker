## 설치하기 

<pre>

$ curl -s https://get.docker.com/ | sudo sh
$ sudo usermod -aG docker $USER // 현재 접속중인 사용자에게 권한주기 - sudo 없이 사용하기
$ docker version


Docker 저장소 GPG key 추가
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 8118E89F3A912897C070ADBF76221572C52609D



Docker 저장소 추가
$ echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee /etc/apt/sources.list.d/docker.list


$ sudo apt-get update
$ sudo apt-get install -y docker-engine
$ sudo apt-get install -y docker-compose



sudo 없이 명령어 사용하도록 설정
$ sudo usermod -aG docker $(whoami)


$ docker -v
$ docker-compose -v

</pre>


## 삭제하기

어떤 이유로 도커를 삭제하시고자 한다면 아래 처럼 간단히 삭제 할 수 있습니다.

$ sudo apt-get autoremove --purge docker-engine
$ sudo rm -fR /var/lib/docker 
## 사용자가 만든 디렉토리/파일은 사용자가 직접 삭제해야 합니다.