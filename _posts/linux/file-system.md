## 파일 시스템

### df

리눅스 시스템 전체 마운트 된 파일 시스템의 디스크 사용량을 알아본다.

* -h : 용량을 보기 좋게 표시한다.

### du

디렉토리의 디스크 사용량을 알아본다.

### smartctl

물리디스크의 정보를 확인한다.

```bash
$ smartctl -i /dev/sda
$ smartctl -i /dev/sdb
```

### fdisk

디스크를 파티션 한다.

#### -l

디스크의 파티션 정보를 조회한다.

### parted

편하게 하드디스크를 관리할 수 있는 유틸이다. 

```
# yum install parted                    [On RHEL/CentOS and Fedora]
```

parted 명령어를 입력하면 프로그램으로 진입한다. 

종료하려면 `quit` 을 입력한다.

#### print 

현재의 하드 디스크의 정보를 출력한다.

#### select

다른 하드 디스크를 선택한다.

```
(parted) select /dev/sdb
```

 #### mklabel

하드 디스크에 라벨을 준다. 다음 목록에서 선택한다. 

**aix, amiga, bsd, dvh, gpt, mac, msdos, pc98, sun, or loop**

#### mkpart

파티션을 생성한다.

```
Partition name?  []? LGH2
File system type?  [ext2]? ext4
Start? 1
End? 8000GB
```

#### rm 

파티션을 삭제한다. 

### mkfs

파티션을 포맷한다.

```
mkfs.ext4 /dev/sdb1
```

