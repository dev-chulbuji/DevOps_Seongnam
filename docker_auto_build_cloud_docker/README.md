# cloud docker를 활용한 docker auto build
본 문서는 hands-on에 가까운 문서이며 구체적인 개념 설명은 참조링크를 활용합니다.  

## Overview
docker cloud를 활용해 docker build를 자동화 하는 문서이며 github 저장소에 소스코드가 push 되었을 때 webhook으로 docker cloud에서 auto build를 하는 내용입니다.

---
### docker  
- [docker wiki](https://en.wikipedia.org/wiki/Docker_(software))  
- [Eric Han님의 docker 기초 확실히 다지기](https://futurecreator.github.io/2018/11/16/docker-container-basics/?fbclid=IwAR1arjnFYoFXDqPp4ZBTSiZJcHFiZpbsUSEfjQT19u9uwhz-GH2s4Gg_FPs)


### docker cloud  
docker cloud는 docker hub 위에서 docker 생태계에 좀 더 포괄적인 기능들을 제공해준다. 

  - [Overview of Docker Cloud](https://docs.docker.com/docker-cloud/)  
  - [Overview of Docker Hub](https://docs.docker.com/docker-hub/)

---
# 1. docker cloud setting
우선 docker cloud에 source provider로 github 저장소를 설정합니다.

- https://cloud.docker.com 접속 후 로그인 > account setting
![cloud_docker_source_provider](/docker_auto_build_cloud_docker/images/cloud_docker_source_provider.png)

-  cloud docker에 create repository를 한 후 해당 repository에 build탭 > configuation auto builds > 원하는 repository를 세팅합니다.
![cloud_docker_build_config_1](/docker_auto_build_cloud_docker/images/cloud_docker_build_config_1.png)

- Build Rule을 설정한다.
build context 설정을 통해 dockerfile을 위치를 명시 할 수 있습니다.
![cloud_docker_build_config_2](/docker_auto_build_cloud_docker/images/cloud_docker_build_config_2.png)
  * Build context는 repository의 root path를 나타낸다. 즉 build시 config관련 파일이나 README 파일은 build context의 경로 기준으로 찾고 만약 파일이 없다면 default인 '/' path에서 찾게 된다.
  * Build rule로는 tag 및 branch로 설정 할 수 있고 example에 있듯이 regular expression을 통해 tag, branch를 지정 할 수 있다.
  * Docker tag경우는 auto build 됬을 시 docker image tag를 나타낸다.



> 자 이제 docker 자동 빌드를 위한 docker cloud setting이 끝났다.  
이제 github에 소스코드를 배포하면 docker cloud에서 자동으로 hook을 받아 build가 이뤄진다. (물론 build rule에 Autobuild를 enable 했을 시!)
github repository setting에 가보면 자동으로 webhook이 걸려있는걸 확인 할 수 있다.

---

# 2. Dockerfile 및 소스코드 push

Dockerfile 및 간단한 node express기반의 api server를 배포하고 docker cloud에서 자동으로 build가 되는지 알아본다.

```
npm init
touch .gitignore app.js Dockerfile .dockerignore
```
- [app.js source](https://github.com/dev-chulbuji/DevOps_Seongnam/blob/master/docker_auto_build_cloud_docker/app.js)
- [Dockerfile source](https://github.com/dev-chulbuji/DevOps_Seongnam/blob/master/docker_auto_build_cloud_docker/Dockerfile)
- [package.json source](https://github.com/dev-chulbuji/DevOps_Seongnam/blob/master/docker_auto_build_cloud_docker/package.json) 

```
git init
git remote add origin {{github repository url}}
git branch --set-upstream-to=origin/master master

git add .
git commit -m "{{commit message}}"

git pull --allow-unrelated-histories
git push
```

이제 소스 코드가 repository로 push가 되면 cloud docker에서 build가 실행된다.
![cloud_docker_build_config_2](/docker_auto_build_cloud_docker/images/cloud_docker_build.png)



build가 완성되면 docker가 설치되어 있는 환경에서 테스트를 해본다.
```
docker run -dit --name auto-build-test {{cloud docker username / repositoryname}}

// ex: docker run -dit --name auto-build-test ladmusiciankim/auto_build_test

docker ps -a
```

http://localhost:3000 으로 접속해서 test 해본다.

![chceck localhost](/docker_auto_build_cloud_docker/images/localhost_3000.png)
```
docker exec -it auto-build-test bash
cd /usr/src/app

// source code 확인
```
