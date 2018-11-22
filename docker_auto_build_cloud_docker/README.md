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

우선 docker cloud에 source provider로 github 저장소를 설정합니다.

- https://cloudc.docker.com 접속 후 로그인 > account setting
![cloud_docker_source_provider](/docker_auto_build_cloud_docker/images/cloud_docker_source_provider.png)

-  cloud docker에 create repository를 한 후 해당 repository에 build탭 > configuation auto builds > 원하는 repository를 세팅합니다.
![cloud_docker_build_config_1](/docker_auto_build_cloud_docker/images/cloud_docker_build_config_1.png)

- Build Rule을 설정한다.
build context 설정을 통해 dockerfile을 위치를 명시 할 수 있습니다.
![cloud_docker_build_config_2](/docker_auto_build_cloud_docker/images/cloud_docker_build_config_2.png)
  * Build context는 repository의 root path를 나타낸다. 즉 build시 config관련 파일이나 README 파일은 build context의 경로 기준으로 찾고 만약 파일이 없다면 default인 '/' path에서 찾게 된다.
  * Build rule로는 tag 및 branch로 설정 할 수 있고 example에 있듯이 regular expression을 통해 tag, branch를 지정 할 수 있다.
  * Docker tag경우는 auto build 됬을 시 docker image tag를 나타낸다.
