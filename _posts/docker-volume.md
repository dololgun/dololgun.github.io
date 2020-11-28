bind mount 는 운영체제의 디렉터리 시스템의 영향을 많이 반면 volume은 완벽히 도커에 의해서 관리되기 때문에 머신에 독립적이다. 볼륨은 마운트에 비해 다음 장점을 가지고 있다. 

- Volumes are easier to back up or migrate than bind mounts.
- You can manage volumes using Docker CLI commands or the Docker API.
- Volumes work on both Linux and Windows containers.
- Volumes can be more safely shared among multiple containers.
- Volume drivers let you store volumes on remote hosts or cloud providers, to encrypt the contents of volumes, or to add other functionality.
- New volumes can have their content pre-populated by a container.
- Volumes on Docker Desktop have much higher performance than bind mounts from Mac and Windows hosts.

자세한건 모르겠고 무조건 볼륨을 쓰자.

그리고 가장 중요한 것은 컨테이너의 특징인데, 컨테이너는 언제라도 사라질 수 있기 때문에 컨테이너 내부에 데이터를 저장하는 것은 아주 위험한 일이다. 그래서 볼륨을 이용하여 계속 저장되어야 하는 데이터를 컨테이너 외부에 저장해야 할 수있다.

## Create and manage volumes

볼륨은 컨텍이너의 실행과 별개로 생성할 수 있다. 

```bash
$ docker volume create my-vol
```

`-o` or `--opt` 옵션을 사용하면 볼륨을 더욱 자세하게 설정할 수 있다. 

```bash
$ docker volume create --driver local \
    --opt type=btrfs \
    --opt device=/dev/sda2 \
    foo
```





자신의 도커에 설정된 볼륨 정보를 보려면 다음 명령어를 입력한다. 

```bash
$ docker volume ls

local               my-vol
```

볼륨의 상세 정보를 보려면 다음 명령어를 사용한다. 이 명령어를 통하여 해당 볼륨이 실제 내 pc의 어느 디렉토리와 매핑됐는지 알 수 있다.

```bash
$ docker volume inspect my-vol
[
    {
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/my-vol/_data",
        "Name": "my-vol",
        "Options": {},
        "Scope": "local"
    }
]
```

볼륨을 삭제하려면 다음 명령어를 사용한다. 

```bash
$ docker volume rm my-vol
```

볼륨을 삭제하면 볼륨과 매핑된 실제 폴더도 삭제 된다. 