### 컨테이너 내부 파일 복사하기

```bash
// 컨테이너를 생성만 한다.
$ docker create --name samba-config dperson/samba
// 생성한 컨테이너 내부의 파일을 복사한다.
$ docker cp samba-config:/etc/samba/smb.conf ./
// 컨테이너를 삭제한다.
$ docker rm samba-config
```