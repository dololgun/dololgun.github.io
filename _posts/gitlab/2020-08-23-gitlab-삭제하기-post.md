---
title: "gitlab 삭제하기"
date: 2020-08-23 16:00:00 +0900
categories: gitlab
---

설치한 gitlab을 삭제하는 방법이다. 아래 명령어를 실행한다.

```shell
sudo gitlab-ctl uninstall
sudo gitlab-ctl cleanse
sudo gitlab-ctl remove-accounts
sudo dpkg -P gitlab-ce || sudo yum -y remove gitlab-ce
```

명령어로 삭제한 후, 아래 폴더를 삭제한다.

```shell
/opt/gitlab
/var/opt/gitlab
/etc/gitlab
/var/log/gitlab
/etc/yum.repos.d/gitlab 관련파일
```



