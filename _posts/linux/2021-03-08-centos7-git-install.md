---
title: "CentOS 7 git 최신버전 설치하기"
date: 2021-03-08 09:00:00 +0900
categories: linux
---

CentOS7에 기본 설치되어 있는 git의 버전은 1.8이다. yum 저장소에 등록되어 있는 것도 1.8이기 때문에 yum update로도 최신버전으로 업그래이드할 수 없다.

최신버전을 설치하기 위해 다음과 같이 해야 한다.

```bash
$ yum install http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
```

위의 명령어는 wandisco 저장소를 yum에 등록한다.

이제 다음 명령어로 이미 설치되어 있는 git을 제거하자.

```bash
$ yum remove git
```

이제 다시 설치하자.

```bash
$ yum install git
```

git 버전을 확인한다.

```bash
$ git --version
git version 2.22.0
```



