FROM jenkins:latest
MAINTAINER m.taekwon.se@gmail.com
 
# Root 권한의 사용자로 접속하여 기본 OS 업데이트 진행 및 jenkins 사용자를 sudo 권한 등록 / lib 삭제의 경우에는 docker images 용량 최소화를 위해
USER root
RUN apt-get update \
      && apt-get install -y sudo \
      && rm -rf /var/lib/apt/lists/*

# console 에서 sudo 설정 없이 사용하기위함.
RUN usermod -aG root jenkins

# jenkins 데몬에서 실행되는 스크립트를 sudo 권한을 획득하여 실행하기위함.
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers

# jenkis 사용자로 전환하여 설정한 plugins 설치 진행 
USER jenkins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
