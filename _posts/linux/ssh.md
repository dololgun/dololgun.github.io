ssh에 대한 몇 가지 설정을 다룬다.

## private network 만 허용

다음 경로의 ssh 설정 파일을 수정한다.

```bash
# nano /etc/ssh/sshd_config
```

설정 파일에 `ListenAddress 0.0.0.0` 항목을 찾는다. 여기에 설정된 ip만 접속할 수 있다.