---
title:  "[Linux] 리눅스 docker 설치"
search: true
categories: 
  - linux
tag:
  - docker
  - docker-compose
last_modified_at: 2022-04-27T08:06:00-05:00
---

프로젝트에서 배포를 해보면서 Docker를 사용해볼 일이 많았는데

이번에 프로젝트 서버를 내 라즈베리파이에 띄우는 작업을 하면서 docker 설치방법을 정리해보았다.

참고로 라즈베리파이를 기준으로 작성되었다.

## 목차
1. docker 설치
2. 사용자 설정
3. docker-compose 설치

<br>

## docker 설치

리눅스에 도커를 설치하는 방법은 다양하지만 나는 스크립트 파일하나로 간단하게 설치하는 방법을 쓰겠다.

우선 다음 명령어를 통해 스크립트파일을 다운받는다.
```bash
$ curl -fsSL https://get.docker.com -o get-docker.sh
```

그 후 스크립트 파일을 실행시킨다.
```bash
$ sudo sh get-docker.sh
# Executing docker install script, commit: 0221adedb4bcde0f3d18bddda023544fc56c29d1
+ sudo -E sh -c apt-get update -qq >/dev/null
+ sudo -E sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
+ sudo -E sh -c curl -fsSL "https://download.docker.com/linux/debian/gpg" | gpg --dearmor --yes -o /usr/share/keyrings/docker-archive-keyring.gpg
+ sudo -E sh -c chmod a+r /usr/share/keyrings/docker-archive-keyring.gpg
+ sudo -E sh -c echo "deb [arch=arm64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian bullseye stable" > /etc/apt/sources.list.d/docker.list
+ sudo -E sh -c apt-get update -qq >/dev/null
+ sudo -E sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq --no-install-recommends docker-ce docker-ce-cli docker-compose-plugin >/dev/null
+ version_gte 20.10
+ [ -z  ]
+ return 0
+ sudo -E sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq docker-ce-rootless-extras >/dev/null
+ sudo -E sh -c docker version
Client: Docker Engine - Community
 Version:           20.10.14
 API version:       1.41
 Go version:        go1.16.15
 Git commit:        a224086
 Built:             Thu Mar 24 01:47:24 2022
 OS/Arch:           linux/arm64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.14
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.15
  Git commit:       87a90dc
  Built:            Thu Mar 24 01:45:44 2022
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.5.11
  GitCommit:        3df54a852345ae127d1fa3092b95168e4a88e2f8
 runc:
  Version:          1.0.3
  GitCommit:        v1.0.3-0-gf46b6ba
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

================================================================================

To run Docker as a non-privileged user, consider setting up the
Docker daemon in rootless mode for your user:

    dockerd-rootless-setuptool.sh install

Visit https://docs.docker.com/go/rootless/ to learn about rootless mode.


To run the Docker daemon as a fully privileged service, but granting non-root
users access, refer to https://docs.docker.com/go/daemon-access/

WARNING: Access to the remote API on a privileged Docker daemon is equivalent
         to root access on the host. Refer to the 'Docker daemon attack surface'
         documentation for details: https://docs.docker.com/go/attack-surface/

================================================================================
```

스크립트 파일은 알아서 사용자의 리눅스 배포판과 cpu아키텍쳐를 확인하고 그에 맞는 설치파일로 docker를 설치해준다.

위의 과정 하나로 docker의 설치는 끝났지만 한가지 더 설정할 것이 남아있다.

<br>

## 사용자 설정

일반 사용자가 `sudo`없이 docker 명령어를 치면 다음과 같은 오류가 뜬다.
```bash
$ docker ps
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json": dial unix /var/run/docker.sock: connect: permission denied
```

docker daemon은 root 권한을 통해 실행되기 때문에 일반 사용자 계정으로는 바로 접근할 수 없기 때문이다.

이를 해결하기 위한 방법은 2가지가 있는데 Unix 그룹을 만들고 그룹에 사용자를 추가하는 방법과 `rootless`모드를 설정하는 것이다.

여기서는 사용자 설정을 통해 해결해보겠다.

### 도커 그룹에 사용자 추가
```bash
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
```

이후 재접속하고 다시 명령어를 입력하면 `sudo`없이도 잘 작동하는 걸 확인할 수 있다.

<br>

## docker-compose 설치

이제 docker와 함께 많이 사용하는 docker-compose를 설치해보자

라즈베리파이가 아닌 x86 시스템을 사용하는 리눅스는 아래 명령어를 통해 설치하자.

### 라즈베리파이 이외 시스템 docker-compose 설치 명령어
```bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

<br>

라즈베리파이는 아래 명령어로 `pip3`를 설치하고 `pip3`를 사용해 설치한다.

라즈베리파이는 위의 명령어로 설치하면 제대로 설치가 안되는 버그가 있다.

### 라즈베리파이 `pip3` 설치
```bash
sudo apt-get install libffi-dev libssl-dev
sudo apt install python3-dev
sudo apt-get install -y python3 python3-pip
```

### 라즈베리파이 docker-compose 설치
```bash
sudo pip3 install docker-compose
```

<br>
이후엔 권한과 심볼릭 링크 설정으로 마무리해준다.

### 도커 컴포즈에 권한 설정
```bash
$ sudo chmod +x /usr/local/bin/docker-compose
```

### 심볼릭 링크 설정
```bash
$ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

<br>

마지막으로 도커 컴포즈 명령어를 입력해 설치가 잘되었는지 확인해본다
```bash
$ docker-compose -version
docker-compose version 1.25.0, build unknown
```

<br>

만약 아래 처럼 docker-compose를 찾을 수 없다고 뜬다면
```bash
$ docker-compose
/usr/local/bin/docker-compose: 1: Not: not found
```

설치가 제대로 안되었거나 설정이 잘못되었을 가능성이 크다.

아래 명령어로 docker-compose를 지우고 재설치를 해보자
```bash
sudo rm /usr/local/bin/docker-compose
sudo apt remove docker-compose
sudo apt autoremove
```
