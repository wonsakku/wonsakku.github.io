---
title: "docker로 spring-boot app 배포해보기 - with Dockerfile"

categories:
    - docker

tags:
     - [docker, Dockerfile]

date: 2021-11-27
last_modified_at: 2021-11-27
---


# Dockerfile-test-01

> ### 아래의 과정은 Windows 10에서 spring-boot application을 jar파일로 묶은 후 
> ### vmware centos7에서 docker 설치 및 애플리케이션 배포 과정을 진행했다.

<br>

## 1. docker 설치
```yml
yum -y install yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo 
yum install -y docker-ce docker-ce-cli containerd.io

```

<br>

## 2. docker로 spring-boot app 배포해보기

<br>

> ### 참고 [스프링 공식 사이트](https://spring.io/guides/gs/spring-boot-docker/)

<br>

### (1) spring-boot 어플리케이션을 서버로 옮긴다.


<br>

### (2) Dockerfile 생성

<br>


```yml
vi Dockerfile
```

<br>

### (3) Dockerfile 작성

<br>

```Dockerfile
FROM openjdk:8-jdk-alpine
ARG JAR_FILE=target/dockertest.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```

* 설명

```yml
FROM : 어느 이미지를 사용할지를 지정해준다.
ARG : 
COPY : 파일을 docker container에 복사해준다. ${JAR_FILE}은 위의 ARG에서 선언한 값
ENTRYPOINT : container 실행 시 실행할 명령어

# ARG의 JAR_FILE을 입력시 절대경로로 입력했을 때 오류가 발생했다. 
# Dockerfile의 위치를 기준으로 상대경로를 잡아주자.
```

<br>


### (4) 디렉토리 구성

<br>

```cmd
[root@localhost spring-boot-app]# pwd
/root/spring-boot-app

[root@localhost spring-boot-app]# ls -R

.:
Dockerfile  target

./target:
dockertest.jar

```


<br>

### (5) docker build & docker run

```
docker build . -t docker-deploy-test
docker run -d -p 8080:8080 --name app-test docker-deploy-test
```

<br>

> docker build . -t docker-deploy-test

|옵션|설명|
|:----:|:----:|
|.|Dockerfile의 위치를 지정해준다.(Dockerfile이 있는 디렉토리에서 명령어를 실행했기 때문에 . )|
|t|build시 생성될 image의 REPOSITORY명이 된다.|

<br>

> docker run -d -p 8080:8080 --name app-test docker-deploy-test

<br>

|옵션|설명|
|:----:|:----:|
|d|백그라운드 환경에서 실행되게 해준다. -d 옵션이 없으면 cmd창을 닫는 후 프로그램이 종료된다.|
|p|포트포워딩 설정. 서버의 8080포트(: 앞의 포트번호)와 container의 8080포트(: 뒤의 포트번호)를 포트포워딩해준다.|





