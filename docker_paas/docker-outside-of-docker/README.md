## Docker outside of Docker (DooD)
DinD의 아래 문제점을 해결 하기위해 DooD를 확인해 보았습니다, DooD는 호스트의 Docker 소켓을 게스트로 마운트하여 게스트 continer 에서 호스트에서의 docker install 환경을 이용하는 것으로 DinD와는 다른 형태의 테스트 입니다.
* docker in docker (dind)의 가장큰 문제점은 보안 측면에서의 호스트의 권한의 많은 부분을 사용할수 있는도록 하는 privileged 옵션을 부여해야 하는 부분에 입니다.
* --privileged 옵션 설명
  * Give extended privileges to this container
---

호스트의 docker 소켓을 마운트 하는 게스트 continer 생성 및 게스트 continer 에서의 continer images 생성 및 dockerhub 업로드 하는 부분까지 테스트를 해보도록 하겠습니다.

- [Dockerfile source](https://github.com/jpetazzo/dind/blob/master/Dockerfile)
  - 일반적인 dind images 가 아닌, ubuntu 및 docker 이미지로도 가능하나 continer에서 docker 명령어 실행시 docker 명령어를 찾지 못하여 빠른 테스트를 위해 기존에 테스트를 진행했던 dind images를 이용 했습니다.

```
docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock --name=dood dind
```
dood console
```
docker ps -a
```
![dood-ps-a](/docker_paas/images/dood-ps-a.png)
docker console
```
docker ps -a
```
![docker-console](/docker_paas/images/docker-console.png)
dood console
```
docker commit dood moontaekwon/dood:1.0
docker images
```
![dood-images](/docker_paas/images/dood-images.png)
dood console
```
docker login
docker push moontaekwon/dood:1.0
```
![dood-push](/docker_paas/images/dood-push.png)

* docker 호스트의 docker 소켓을 공유함으로 docker 호스트의 이미지 및 컨테이너 접속등을 모두 진행 할수 있었습니다. DinD 와는 또 다른 장점으로 생각 됩니다.