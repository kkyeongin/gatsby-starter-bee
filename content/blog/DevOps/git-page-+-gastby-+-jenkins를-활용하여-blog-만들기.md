---
title: Git Page+Docker+Jenkins
date: 2019-02-12 19:11:05
category: DevOps
---
![Alt text](../../assets/gatsby_how_to_work.jpg)

## 블로그 만들기(1)

* Goal
  * Why Gatsby?
  * Setup Git page
  * Install Docker
  * Installing Jenkins with Docker and Jenkins Run
  * Make a Docker image

### Why Gatsby

이전에 몇번 git blog를 만들어 보려 hexo 를 사용했었다.

Gatsby는 Static Site Generator 이다.
Jekyll & Hexo 또한 같은 정적 사이트 생성기이며 처음에는 Hexo를 이용해서 블로그를 만들었었다.
Jekyll 은 Ruby on Rails 기반이며, Hexo는 Node.js기반이여서 나는 고치기 쉬울것 같은 Hexo를 선택해서 사용하였다.
사용하던 중 프론트엔드 쪽 지식이 별로 없어 Hexo theme가 맘에 안들어도 꾸역꾸역 쓰고 있었는데..
그 와중에 맘에 드는 템플릿(https://gatsby-starter-bee.netlify.com/)을 만나 바꾸게 되었다.

### 1. Setup GitHub page

첫번째 방법으로는

1. git page(https://pages.github.com/)설정은 github에서 repo를 만들고 Repository name 을 [username].github.io로 설정해 주면 된다.
2. 그 후 소스코드를 관리하는 repository를 생성해 publishing을 관리하는 repository([username].github.io)에 deploy 하면 된다.

두번째 방법으로는

1. 소스코드 관리하는 repository를 두고 gh-pages branch를 생성하면 된다. 그 후 gh-pages deploy하면 github page에서 자동으로 publishing된다. 이 같은 방법은 따로 다른 repository를 손대지 않아도 되는 장점이 있다.

주소는 htttps://[username].github.io/[repository-name]이다.

원래부터 첫번째 방법을 사용해 왔기에 github에 내 서버 ssh key를 등록 했다.

등록 방법은 : [https://jootc.com/p/201905122827](https://jootc.com/p/201905122827) 참고하기 바란다.

### 2. Install Docker

* OS : ubuntu:18.04
* Docker : 18.09.7

Docker를 사용한 이유는 AWS를 EC2서버를 사용하다 바꾸는 경우가 가끔 있었고 다양한 환경의 서버들을 다루기 꺼려져 Docker를 사용하였습니다. Docker의 경우 설치하고 한번 사용한 이미지 그대로 가져와서 사용하면 되서 환경에 영향을 받지 않기 때문이다.
삽질을 줄일 수 있다는 점이나 안정성에서 만족하며 사용하고 있다.

```sh
# Docker CE (Community Edition) install
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

OK
```

* apt-transport-https: 패키지 관리자가 https를 통해 파일 및 데이터를 전송할 수 있도록합니다.
  
* ca-certificates: 시스템 및 웹 브라우저가 보안 인증서를 확인할 수 있도록합니다.
  
* curl: ftp 같이 웹에서 데이터를 받아올 수 있습니다.

* software-properties-common: software managent 를 위해 script를 추가하는 api

```sh
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs)  stable"
```

* [다른 repositories에 관한 정보](https://docs.docker.com/v17.09/engine/installation/linux/docker-ce/ubuntu/#set-up-the-repository) 를 확인한 수 받으시면 됩니다.

```sh
sudo apt-get update

sudo apt-get install docker-ce   # Docker CE (Community Edition)
```

확인을 위해

```sh
docker --version
```

### 3. Installing Jenkins with Docker and Jenkins Run

도커를 깔았으면 Docker hub에 있는 Jenkins image를 받는다.

![Docker image url](https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/image-url.png)

```sh
docker pull jenkins/jenkins
```

* <b>주의할점</b>은 jenkinsci 와 jenkins:latest등 다른 이미지들은 deprecated 됐다. 삽질하지 말고 jenkins/jenkins를 받자.

```sh
docker pull jenkins/jenkins
```

jenkins을 실행시키려면 Docker run!

```sh
docker run -it\
-u root \
-p [setting-port]:8080 -p setting-port]:50000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v $(which docker):/usr/bin/docker \
-v /home/jenkins_home:/var/jenkins_home \
jenkins/jenkins
```

### 4. Make a Docker image

![create docker image](https://docs.docker.com/v17.09/engine/userguide/storagedriver/images/container-layers.jpg)

도커 이미지는 컨테이너를 실행하기 위한 모든 정보를 가지고 있기 때문에 보통 용량이 수백메가MB에 이릅니다. 처음 이미지를 다운받을 땐 크게 부담이 안되지만 기존 이미지에 파일 하나 추가했다고 수백메가를 다시 다운받는다면 매우 비효율적일 수 밖에 없습니다.

도커는 이런 문제를 해결하기 위해 레이어(layer)라는 개념을 사용하고 유니온 파일 시스템을 이용하여 여러개의 레이어를 하나의 파일시스템으로 사용할 수 있게 해줍니다.
``

```sh
FROM ubuntu:16.04
USER root

# Setup node.js build env
RUN apt-get update
RUN apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install -y nodejs build-essential git python
RUN node -v && npm -v
RUN npm install -g yarn

# Use --build-arg ~/.ssh/id_rsa
ARG ssh_prv_key
ARG ssh_pub_key

# Authorize SSH Host
RUN mkdir -p /root/.ssh && \
    chmod 0700 /root/.ssh && \
    ssh-keyscan github.com > /root/.ssh/known_hosts

# Add the keys and set permissions
RUN echo "$ssh_prv_key" > /root/.ssh/id_rsa && \
    echo "$ssh_pub_key" > /root/.ssh/id_rsa.pub && \
    chmod 600 /root/.ssh/id_rsa && \
    chmod 600 /root/.ssh/id_rsa.pub

# git config
RUN git config --global user.email "kkyeongin@gmail.com"

USER ubuntu:16.04
```

Jenkins에서 Docker를 띄워 빌드를 진행할 예정이여서 추가적으로 Docker이미지를 만들었다.
기본으로 ubuntu:16.04를 받아서 추가적으로 필요한 Node.js 프로젝트 빌드 환경과 GitHub Publishing 레파지토리에 Deploy할때 쓸 ssh key도 등록 하였다.

```sh
docker build \
-t [Dockr image name:tag] \
--build-arg ssh_prv_key=[private ssh key pwd]\
--build-arg ssh_pub_key=[public ssh key pwd]\
[Dockerfile pwd]
```

이렇게 이미지 까지 만들면 이제 Jenkins만 실행해 설정해 주면 된다.

### 참고

* [https://docs.docker.com/install/](https://docs.docker.com/install/)
* [https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04](https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04)
* [Basic CI/CD for Python projects with Docker and Jenkins](https://medium.com/faun/basic-ci-cd-for-python-projects-with-docker-and-jenkins-38eeb547fb28)
* [초보를 위한 도커 안내서 - 도커란 무엇인가?](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
* [configuring-a-publishing-source-for-your-github-pages-site](https://help.github.com/en/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)