# Docker 기반 개발환경(PaaS) 구축!!
본 문서는 hands-on에 가까운 문서이며 구체적인 개념 설명은 참조링크를 활용합니다.  

## Overview
docker hub를 이용한 기본적으로 공유되고 있는 container 이미지를 이용하여 개발환경을 구축하고 개발환경 에서의 OS영역 만은 Immutability(불변성) 을 보장되게 하는 내용입니다

---
### docker  
- [docker wiki](https://en.wikipedia.org/wiki/Docker_(software)) 
- [docker-docs install](https://docs.docker.com/docker-for-mac/install/)
### docker hub  
- [Overview of Docker Hub](https://docs.docker.com/docker-hub/)
---

기본적인 docker 설치를 완료 했다는 조건으로 진행 합니다.
dockerhub 에서의 기본적인 container 이미지를 이용하여 입맛에 맞도록 재빌드 하는 방법을 진행 합니다.
* 안정성이 보장되지 않은 container 이미지의 경우에는 보안적으로 취약할수 있습니다. 1차 적인 방법은 docker search 를 이용한 조회시에 stars 의 갯수를 확인하는 방법 입니다.
(stars = 즐겨찾기)

```
docker --help
```
![help](/docker_paas/images/docker-help.png)
```
docker -v
```
![v](/docker_paas/images/docker-v.png)
```
docker search ubuntu
```
![search](/docker_paas/images/docker-search.png)
```
docker run -it -p 8080:80 --name=ubuntu_test ubuntu
```
![run](/docker_paas/images/docker-run-it.png)
dockerhub 를 이용한 container 이미지의 내용을 회사표준 및 공동개발을 진행하는 개발자들에게 공유하기위한 표준 container 이미지를 작성한다.
```
apt-get update
apt-get install apache2
cd /var/www/html
vi index.html
```
첫번째 줄에 내용 추가.
* Moon 회사에 맞는 개발환경 구축 완료

![ubuntu-apache2](/docker_paas/images/ubuntu-apache2.png)

위의 작업 이후에 container 에서 작업을 종료하고 (container 콘솔 안에서 exit 명령어를 통한 종료), container 에서 작업한 내용을 아래의 절차에 따라 저장 및 공유한다.

```
docker commit ubuntu_test moontaekwon/ubuntu-docker-images:1.0
docker images
```
![docker-commit](/docker_paas/images/docker-commit.png)
```
docker push moontaekwon/ubuntu-docker-images:1.0
```
![docker-push](/docker_paas/images/docker-push.png)
위의 작업으로 dockerhub 로 업로드 되었으며, 해당 제작한 이미지를 가지고 어디서든 pull(다운로드) 받아서 사용이 가능하다.
![dockerhub-moon](/docker_paas/images/dockerhub-moon.png)
```
docker pull moontaekwon/ubuntu-docker-images:1.0
docker images
```
![docker-images2](/docker_paas/images/docker-images2.png)
```
docker run -it -p 8080:80 --name=moon moontaekwon/ubuntu-docker-images:1.0
```
```
service apache2 start
```
![ubuntu-apache2](/docker_paas/images/ubuntu-apache2.png)


* 18.11.20 - 18.11.23
    * docker paas (11.21)
    * dockerfile (11.23)
    * docker in docker (11.23)