---
layout: post
title: "부스트캠프 챌린지 Day7"
tags: [boostcamp, virtual environment, shellscript]
---

docker를 이용해 가상환경을 설치하고 서버를 구축해봤다. 평소에 프론트엔드 개발에 치우쳐서 배포같은 건 등한시하고 살았는데, 그래서 굉장히 애먹었다. 아마 부스트캠프 챌린지 중 제일 어려웠던 주제가 아닌가 생각한다(ㅠㅠ) shell script도 처음 사용해봤는데 내가 지금까지 배워온 프로그래밍 언어와 상당히 달라서 어려웠다. 그래도 일단 기본 문법부터 배우고 미션에 도전했으면 더 잘 했을 것 같다. 무작정 구글링만 한 결과 오히려 더 꼬여서 고생했던.. 기본의 중요함을 배웠다^^..!

# **1. 가상환경 설치하기**

### **docker와 ubuntu 설치**

-   docker 설치: 사이트에서 desktop package를 받아서 설치할 수 있다.
-   터미널에 `docker version` 입력. Client와 Server가 뜨면 정상적으로 실행된 것

### **docker 실행 명령어**

    docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]

[자주 사용하는 docker 옵션](https://www.notion.so/a2b5fffb73d34552a9c7e72f8aaec737)

    docker run ubuntu:18.04
    // ubuntu:18.04가 존재하지 않으면 자동으로 다운받아서 설치

    docker run -it --rm ubuntu:18.04 bin/bash
    root@75120f247bde:/#
    // 설치가 끝나면 bash 로 실행, 위처럼 root@~ 로 바뀌면 컨테이너에 성공적으로 진입한 것

참고)

### **ssh 서버 구축하기**

docker에 설치된 ubuntu 컨테이너에 접속해 ubuntu에 ssh 구축하는 방법대로 진행하면 된다.

    apt-get update            													// ssh와 같은 package를 설치하기 전에 실행한다.
    apt-get install ssh
    vim etc/ssh/sshd_config													// config 파일을 수정, 임의의 포트 1010을 설정해줬다.

    root@75120f247bde:/# adduser user                               // user 추가
    root@75120f247bde:/# passwd user																// 지정한 계정의 password 추가
    Enter new UNIX password:
    Retype new UNIX password:
    passwd: password updated successfully

구축된 서버에 들어가 `ifconfig` 명령으로 ip를 확인, 로컬 컴퓨터에서 가상환경에 접근 가능하다.

### **리눅스 퍼미션**

리눅스 환경에서 사용자에 따라 쓰기, 읽기, 접근 등 권한을 설정해 줄 수 있는데, 8진수 명령어로 가능하다.

[Untitled](https://www.notion.so/225e7b71e19e4bb396090eb14353ac2c)

    chmod 764 backup

위의 명령어를 입력하면 소유자는 읽기, 쓰기, 실행, 그룹은 읽기, 쓰기만, 그 외의 모든 사용자는 읽기만 가능한 상태로 권한을 바꿀 수 있다. 권한은 `ls -l` 명령어를 이용해 확인 가능하다.

권한을 설정해줌으로써 추후 `scp` 등의 명령어 사용시 일어나는 `permission denied error`를 방지할 수 있다.

[참고자료](https://nachwon.github.io/shell-chmod/)

# **2. 쉘 스크립트**

```shell
#!/bin/zsh
today=$(date +%Y%m%d)
find . -mindepth 1 -maxdepth 1 -type d '!' -exec sh -c 'ls -1 "{}"|egrep -i -q ".(js)$"' ';' -print
find ./ -name '*.js' | zip backup_$today.zip -@
scp -P 1010 backup_$today.zip user@127.0.0.1:/home/user/backup
```

오늘 날짜를 YYMMDD 형태로 변환 후 , 변수에 할당

find 함수를 이용해 .js 파일이 존재하지 않는 디렉토리를 찾아 디렉토리 이름을 출력

find 함수를 이용해 .js 파일만 찾아 zip 압축, backup\_\$today.zip 의 파일명으로 압축한다

가상환경 내 지정된 위치에 압축 파일을 복사한다.

# **3. 타입스크립트 컴파일링**

local에서 `npm install -g typescript` 로 타입스크립트 컴파일러 설치, `tsc baseball.ts` 명령어를 통해 `baseball.js` 파일을 생성해 주었다.

로컬 컴퓨터에서 가상환경 접속 후, ws 명령어 입력시 다음과 같이 호스트 주소가 나타난다.
