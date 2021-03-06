# jenkins(dood)를 활용한 docker auto build + github
본 문서는 hands-on에 가까운 문서이며 구체적인 개념 설명은 참조링크를 활용합니다.

## Overview
docker outside fo docker (DooD)를 이용한 docker 안에서 구동되는 jenkins 어플리케이션이 호스트의 docker 명령어를 이용하여 자동 build 하는 과정을 테스트한 내용 입니다.

---
### reference link
- [jenkins](https://github.com/jenkinsci/jenkins)
- [node wiki](https://en.wikipedia.org/wiki/Node.js)
- [github-webhooks](https://developer.github.com/webhooks/)
- [ngrok](https://ngrok.com/)
- [sparse checkout](http://stackoverflow.com/questions/600079/is-there-any-way-to-clone-a-git-repositorys-sub-directory-only)

상세설명이 필요한 내용은 테스트 순서에 기재 하였습니다 그 밖의 내용은 아래의 사전설명 및 위의 링크를 확인하여 주시기 바랍니다.
(docker 설치완료 및 [docker_paas](https://github.com/dev-chulbuji/DevOps_Seongnam/tree/master/docker_paas) 내용을 숙지 했다고 생각하고 진행 합니다.)
* jenkins - Continuous Integration(지속적 통합) & Continuous Deliver(지속적 전달)용도로 많이 사용되는 툴중 하나 입니다.
* ngrok - localhost를 공용IP로 변환해주는 툴중 하나 입니다.

--- 
방법1) dockerhub download [moontaekwon/dood-jenkins](https://hub.docker.com/r/moontaekwon/dood-jenkins/)
```
docker run -d -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker -p 8080:8080 moontaekwon/dood-jenkins:1.0
```
방법2) docker build를 할 경우에는 아래의 소스코드를 이용 해주시면 됩니다. (아래의 파일 다운받은 이후 docker build 및 docker run)
- [Dockerfile](https://github.com/dev-chulbuji/DevOps_Seongnam/blob/master/docker_auto_build_jenkins/Dockerfile)
- [pulgins.txt](https://github.com/dev-chulbuji/DevOps_Seongnam/blob/master/docker_auto_build_jenkins/plugins.txt)
```
docker build -t moontaekwon/dood-jenkins .
docker run -d -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker -p 8080:8080 moontaekwon/dood-jenkins
```
확인.
![docker-jenkins-images](images/docker-jenkins-images.png)
![docker-jenkins-ps](images/docker-jenkins-images.png)
ngrok 설치 8080포트 공인DNS로 변환.
```
./ngrok http 8080 # 설치 경로에서 진행.
```
![ngrok-ps](images/ngrok-ps.png)

jenkins 설정
```
docker exec -it $CONTAINER-NAME bash
cat /var/jenkins_home/secrets/initialAdminPassword
exit
```
install suggested plugins && 계정 설정
![jenkins-install](images/jenkins-install-a.png)
github 연동을 위한 github plugin 확인
![jenkins-plugin](images/jenkins-plugin.png)
jenkins item 설정
![jenkins-itme-add-1](images/jenkins-item-add1.png)
![jenkins-item-add-2](images/jenkins-item-add2.png)
project url 지정 (github 주소)
![jenkins-item-a](images/jenkins-item-a.png)
소스코드 관리 (github *.git 주소 입력)
![jenkins-item-b](images/jenkins-item-b.png)
빌드 유발
![jenkins-item-c](images/jenkins-item-c.png)
빌드
* docker 명령어 과정에서 sudo 명령어 사용 및 빌드넘버로 변수를 사용.
    * git 을 통한 clone(다운로드)이후 폴더 이름 변경 및 해당 변경된 폴더에서 docker build 진행 이후 build한 docker iamges 를 이용하여 실행하는 과정
```
git clone https://github.com/Moon-Tae-Kwon/jenkins.git
mv jenkins $BUILD_NUMBER
sudo docker build -t moontaekwon/web-$BUILD_NUMBER $BUILD_NUMBER/.
sudo docker run -d -p 300$BUILD_NUMBER:3000 --name=moon$BUILD_NUMBER moontaekwon/web-$BUILD_NUMBER
```
![jenkins-item-d](images/jenkins-item-d.png)

---
github webhooks

jenkins 에서 등록한 github 주소로 이동하여 별도 생성한 repositories -> Settings -> webhooks 설정
* ngrok 에서 설정했던 jenkins 주소 등록 및 /github-webhook/ 추가 설정

![github-webhooks](images/github-webhooks-status.png)
확인 (외부 Root DNS에 등록되기 까지 5~10분 정도의 시간이 소유될수 있습니다.)
![github-webhooks-ps](images/github-webhooks-pa.png)

---
소스코드(github) push

node.js기반의 간단한 "hello" 웹사이트를 dockerfile로 재작하여 웹사이트의 내용을 변경한 이후 github로의 push 진행시 jenkins-itme(auto_build) 동작여부 확인.

server.js 내용을 변경하여 dockerfile build && run
```
docker build -t moontaekwon/web .
docker run -d -p 3000:3000 moontaekwon/web
```
확인
![webdockerfile-status](images/webdockerfile-status.png)
![web-status](images/web-status.png)

server.js 내용 변경 && 소스코드(github) push
![git-push](images/git-push.png)

* 위의 node.js 기반의 간단한 dockerfile이 필요할 경우 아래 명령어를 통한 다운로드
```
git clone https://github.com/Moon-Tae-Kwon/jenkins.git
```

---

jenkins build 확인.
![auto_build1](images/auto_build-status-1.png)
![auto_build2](images/auto_build-status-2.png)
![auto_build3](images/auto_build-status-3.png)

build execute sheel 내용 확인
![docker-jenkisn-web1](images/docker-jenkins-status1.png)
![docker-jenkisn-web2](images/docker-jenkins-status2.png)
docker iamges 및 container 웹사이트 내용 변경 확인 완료

---

위와 같은 방법으로 docker 안에서 container 형태로 구동되는 jnekins를 통한 dood(docker outside of docker)방식으로 auto_build을 진행하는 방법을 확인해 보았습니다.