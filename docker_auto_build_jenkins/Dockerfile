FROM jenkins:latest
MAINTAINER moontaekwon@docker.com
 
# Root 권한의 사용자로 접속하여 기본 OS 업데이트 진행 및 jenkins 사용자를 sudo 권한 등록
USER root
RUN apt-get update \
      && apt-get install -y sudo \
      && rm -rf /var/lib/apt/lists/*
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers

# jenkis 사용자로 전환하여 설정한 plugins 설치 진행 
USER jenkins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
