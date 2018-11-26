## docker in docker
docker container 이미지 에서 docker를 설치하여 container 이미지를 생성하고 운영 할수 있을까?

---
### dind 
- [dind-github](https://github.com/jpetazzo/dind)
---
container 이미지 안에서 docker 설치 및 container 이미지를 생성하여 dockerhub 업로드 하기 까지의 과정을 수동으로 테스트 해보았습니다.
- [Dockerfile source](https://github.com/jpetazzo/dind/blob/master/Dockerfile)
```
docker build -t dind .
docker run --privileged -t -i dind
```
docker continer console
```
docker -v & docker images & docker ps -a # docker 동작 유무 확인.
docker pull ubuntu
docker run -it --name=moon ubuntu
```
dind continer console
```
cat /etc/*release
exit
```
docker continer console
```
docker commit moon moontaekwon/dind:1.0
docker push moontaekwon/dind:1.0
```
![dind-push](/docker_paas/images/dind-push.png)
* 위의 continer 이미지를 이용한 jenkins 추가 설치 및 설정등을 dockerfile 로 재구성하여, jenkins를 이용한 자동배포등 구성해 보면 좋을것 같다.

별도의 centos/ubuntu를 이용한 dockerinstall 이후 진행하려 헀으나, systemctl등의 이슈로 인하여 조금더 자세하게 확인을 해봐야겠다.