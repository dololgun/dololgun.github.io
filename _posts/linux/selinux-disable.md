selinux 상태 확인

```
$ getenforce
```

Enforcing 이 나오면 활성화 된 것

2가지 방법

설정파일 수정



커맨드로 비활성화

```
$ sudo setenforce 0
```

다시 확인

```
getenforce
```



커맨드로 활성화

```
$ sudo setenforce 1
```

