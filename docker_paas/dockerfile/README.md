### Dockerfile 을 이용한 docker container 이미지 생성
아래의 설명에서는 간단한 dockerfile 작성을 통한 docker container 이미지 생성 방법을 기재 합니다.

---

- [Dockerfile source](/dockerfile/Dockerfile)

작성한 dockerfile을 다운로드 한이후 다운로드 받은 경로에서 진행합니다, 별도의 경로에서 진행할 경우에는 모든 경로는 기재 하도록 합니다.
```
docker build --tag moon-dockerfile:1.0 .
docker images
```
![dockerfile-images](/docker_paas/images/dockerfile-images.png)
```
docker run -it -p 8080:80 --name=moon moon-dockerfile:1.0
```
별도의 터미널 창을 열어서 container 상태 확인.
```
docker ps -a
```
![docker-ct-ps](/docker_paas/images/docker-ct-ps.png)
![dockerfile-apache-ch](/docker_paas/images/dockerfile-apache-ch.png)

* dockerfile 의 경우에는 linux 에서 사용되는 자체 명령어로도 구성이 가능하기 때문에 많은 활용성과 소스로의 관리에 대한 편의성과 무결성 그밖에 많은 장점을 가지고 있을것을 생각 됩니다.