jenkins 서버를 중지한다. 

```shell
systemctl stop jenkins
```

jenkins 서버의 서비스를 제거한다. 

```shell
systemctl disable jenkins
```

yum을 이용해 패키지를 제거한다.

```shell
yum remove jenkins
```

아래 경로의 파일이나 폴더를 모두 삭제한다.

```
/etc/init.d/jenkins
/var/lib/jenkins
/etc/yum.repos.d/jenkins.repo

```

