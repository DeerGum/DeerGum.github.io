---
title:  "[Linux] 라즈베리파이4 apt mirror server 문제해결"
search: true
categories: 
  - linux
tag:
  - apt
last_modified_at: 2022-04-26T08:06:00-05:00
---

싸피에서 진행했던 프로젝트를 살려두고 싶어서 내 자취방에 굴러다니는 라즈베리파이4를 서버로 활용해보기로 했다.

![라즈베리파이](https://user-images.githubusercontent.com/47655983/165224456-c82180ef-9ceb-4ff3-9c6d-51685430762b.png)

<br>

> 열일하고 있는 라즈베리파이들

<br>

기존에도 서버 구축등에 활용하고 있엇지만 8기가 모델에 32비트OS를 깔아서 사용하고 있던 걸 이제야 발견하고 이번기회에 64비트 라즈비안OS로 바꿔주었다. 

OS를 바꿔주고 난 뒤에 맨 처음으로 java8을 설치하려고 했는데 apt list에 8버전이 없었다.(젠킨스를 사용해야 하는데 상위자바버전보단 자바8에서 잘 돌아갔던 기억이 있어서 일부러 8버전을 찾고 있었다)

<br>

찾아보니 라즈비안 설치시 기본으로 제공되는 apt mirror서버에서는 jdk가 11버전부터 있어서 8버전을 설치하기 위해선 mirror서버를 바꿔주어야 했다.

[라즈비안의 mirror서버를 보여주는 사이트](https://www.raspbian.org/RaspbianMirrors)를 참고해서 카이스트에서 제공하는 mirror서버를 사용하기로 하고 변경해주었다.

ssh을 통해 라즈베리파이에 접속하고 다음 명령어를 입력해 mirror 서버를 변경해줄 수 있다.

<br>

```bash
sudo nano /etc/apt/sources.list
```

<br>

![터미널1](https://user-images.githubusercontent.com/47655983/165225628-8315e451-c13d-4177-b0d9-b55916478c9d.png)

명령어 입력시 위와 같은 화면이 뜨는데 맨 처음에는 최상단 3개의 디폴트 주소로 되어있다.

나는 기존의 주소를 주석처리하고 카이스트의 mirror서버 주소를 아래에 추가해서 수정하고 저장했다.

<br>

기존 mirror주소보다 더 빠르게 설치할 수 있겠다는 기대를 품고 `apt-get update` 명령어를 통해 잘 작동하는지 테스트해봤지만 

다음과 같은 에러를 확인할 수 있었다.

<br>

```bash
$ sudo apt update                                                                              
Get:1 http://ftp.kaist.ac.kr/raspbian/raspbian bullseye InRelease [15.0 kB]
Get:2 http://ftp.kaist.ac.kr/raspbian/raspbian bullseye/non-free Sources [140 kB]
Get:3 http://ftp.kaist.ac.kr/raspbian/raspbian bullseye/main Sources [12.2 MB]
Get:4 http://archive.raspberrypi.org/debian bullseye InRelease [23.7 kB]
Get:5 http://archive.raspberrypi.org/debian bullseye/main arm64 Packages [272 kB]
Get:6 http://archive.raspberrypi.org/debian bullseye/main armhf Packages [279 kB]
Get:7 http://ftp.kaist.ac.kr/raspbian/raspbian bullseye/contrib Sources [81.9 kB]
Get:8 http://ftp.kaist.ac.kr/raspbian/raspbian bullseye/main armhf Packages [13.2 MB]
Get:9 http://ftp.kaist.ac.kr/raspbian/raspbian bullseye/contrib armhf Packages [60.2 kB]
Get:10 http://ftp.kaist.ac.kr/raspbian/raspbian bullseye/non-free armhf Packages [106 kB]
Fetched 26.4 MB in 9s (2,928 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
N: Skipping acquire of configured file 'main/binary-arm64/Packages' as repository 'http://ftp.kaist.ac.kr/raspbian/raspbian bullseye InRelease' doesn't support architecture 'arm64'
N: Skipping acquire of configured file 'contrib/binary-arm64/Packages' as repository 'http://ftp.kaist.ac.kr/raspbian/raspbian bullseye InRelease' doesn't support architecture 'arm64'
N: Skipping acquire of configured file 'non-free/binary-arm64/Packages' as repository 'http://ftp.kaist.ac.kr/raspbian/raspbian bullseye InRelease' doesn't support architecture 'arm64'
```

<br>

특정 레포지토리가 arm64 아키텍쳐를 지원하지 않는다는 의미인 것 같은데

라즈비안을 위한 mirror주소인데 64비트OS를 지원하지 않는다는게 조금 의아했지만 안되는 건 안되는거니깐 방법을 찾아야했다.

<br>

열심히 구글링해보니깐 mirror주소를 raspbian으로 하지말고 debian으로 하라는 의견이 있어서 시도해보았다. (라즈비안은 데비안계열 리눅스이다.)

<br>

![터미널2](https://user-images.githubusercontent.com/47655983/165227529-c7db2ade-0a94-422f-93e2-f7d46619c923.png)

<br>

위와 같이 주소를 바꿔주고 `apt-get update` 명령어를 입력해주니

<br>
```bash
& sudo apt update
Get:1 http://ftp.kaist.ac.kr/debian bullseye InRelease [116 kB]
Get:2 http://ftp.kaist.ac.kr/debian bullseye/main Sources [8,627 kB]
Hit:3 http://archive.raspberrypi.org/debian bullseye InRelease
Get:4 http://ftp.kaist.ac.kr/debian bullseye/contrib Sources [43.0 kB]
Get:5 http://ftp.kaist.ac.kr/debian bullseye/non-free Sources [80.6 kB]
Get:6 http://ftp.kaist.ac.kr/debian bullseye/main armhf Packages [7,944 kB]
Get:7 http://ftp.kaist.ac.kr/debian bullseye/main arm64 Packages [8,070 kB]
Get:8 http://ftp.kaist.ac.kr/debian bullseye/main Translation-en [6,241 kB]
Get:9 http://ftp.kaist.ac.kr/debian bullseye/contrib armhf Packages [40.2 kB]
Get:10 http://ftp.kaist.ac.kr/debian bullseye/contrib arm64 Packages [40.8 kB]
Get:11 http://ftp.kaist.ac.kr/debian bullseye/contrib Translation-en [46.9 kB]
Get:12 http://ftp.kaist.ac.kr/debian bullseye/non-free armhf Packages [56.3 kB]
Get:13 http://ftp.kaist.ac.kr/debian bullseye/non-free arm64 Packages [69.6 kB]
Get:14 http://ftp.kaist.ac.kr/debian bullseye/non-free Translation-en [91.3 kB]
Fetched 31.5 MB in 11s (2,984 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
```

<br>

굳 잘 받아온다. 👍

<br>

하지만 안타깝게도 mirror 서버를 바꿔줬지만 이 서버에도 openjdk-8 버전이 없었다...(카이스트의 라즈비안 mirror서버엔 있지만 데비안 mirror서버엔 8버전이 없었다)

일단 11버전을 사용하고 문제가 있으면 8버전을 다시 찾아봐야겠다.

<br>

![우는짤](https://user-images.githubusercontent.com/47655983/165229565-72d8d5a8-6cb8-4bd1-96ee-8fe09ee32913.png)
