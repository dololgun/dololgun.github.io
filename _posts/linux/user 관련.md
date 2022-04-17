## user 목록 보기

```bash
cat /etc/passwd
```

su -

ch

### group 목록 보기

```
cat /etc/group
```

```
[root@localhost ~]# tail -n 5 /etc/group
avahi:x:70:
slocate:x:21:
rngd:x:974:
tcpdump:x:72:
vboxsf:x:973:
```

- `X:Y:Z` 형식으로 나오는데, X는 그룹 이름, Y는 그룹 비밀번호, Z는 그룹 ID입니다.