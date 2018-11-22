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

> https://cloudc.docker.com 접속 후 로그인 > account setting
![cloud_docker_source_provider](/images/cloud_docker_source_provider.png)
