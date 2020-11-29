## 개요

plex media server를 도커 컨테이너로 설치하는 방법이다.

## 이미지

공식사이트에서 제공하는 이미지도 있지만 linuxserver에서 제공하는 이미지를 사용한다. ([링크](https://hub.docker.com/r/linuxserver/plex)) 

plex를 다양한 장비를 지원하기 때문에 우리가 사용하는 이미도 장비에 따라 여러버전이 존재한다. 

linuxserver에서 제공하는 장비별 이미지는 다음과 같다.

| Architecture | Tag            |
| ------------ | -------------- |
| x86-64       | amd64-latest   |
| arm64        | arm64v8-latest |
| armhf        | arm32v7-latest |

## 사용하기 

### 도커 컴포즈를 이용한 방법

다음 컴포즈 설정파일을 이용한다. 

```yaml
---
version: "2.1"
services:
  plex:
    image: ghcr.io/linuxserver/plex
    container_name: plex
    network_mode: host
    environment:
      - PUID=1000
      - PGID=1000
      - VERSION=docker
      - UMASK_SET=022 #optional
      - PLEX_CLAIM= #optional
    volumes:
      - /path/to/library:/config
      - /path/to/tvseries:/tv
      - /path/to/movies:/movies
    restart: unless-stopped
```

## 파라미터

다음과 같은 파라미터를 지원한다. 

| Parameter           | Function                                                     |
| ------------------- | ------------------------------------------------------------ |
| `--net=host`        | 도커 컨테이너의 네트워크를 호스트 방식으로 설정한다.         |
| `-e PUID=1000`      | for UserID - see below for explanation                       |
| `-e PGID=1000`      | for GroupID - see below for explanation                      |
| `-e VERSION=docker` | plex를 update 할지를 설정한다. - see Application Setup section. |
| `-e UMASK_SET=022`  | 플렉스가 생성하는 파일과 폴더의 권한을 설정한다.             |
| `-e PLEX_CLAIM=`    | (선택사항) https://plex.tv/claim으로 부터 클레임 토큰을 포함할 수 있다. 클레임 토큰은 4분안에 만료되는 것에 유의하자. |
| `-v /config`        | 플렉스 라이브러리 위치. *This can grow very large, 50gb+ is likely for a large collection.* |
| `-v /tv`            | Media goes here. Add as many as needed e.g. `/movies`, `/tv`, etc. |
| `-v /movies`        | Media goes here. Add as many as needed e.g. `/movies`, `/tv`, etc. |

## 파일로 부터 환경변수 

`FILE__` 접두어를 붙이고 특정 파일을 연결하면 접두어 다음에 오는 단어를 환경변수로 하는 값을 컨테이너에 전달할 수 있다. 

예를 들어:

```yaml
-e FILE__PASSWORD=/run/secrets/mysecretpassword
```

`PASSWORD` 라는 환경변수를 설정하는데 이 값은 `/run/secrets/mysecretpassword` 파일의 값으로 부터 읽어 온다.

## UMASK

기본 설정은 umask 022 이다. 

## 선택적인 파라미터

컨테이너의 네트워크를 브릿지 모드로 할 경우 포트포워딩 설정이 필요하다. 

```bash
  -p 32400:32400 \
  -p 1900:1900/udp \
  -p 3005:3005 \
  -p 5353:5353/udp \
  -p 8324:8324 \
  -p 32410:32410/udp \
  -p 32412:32412/udp \
  -p 32413:32413/udp \
  -p 32414:32414/udp \
  -p 32469:32469
```

## 사용자 / 그룹

볼륨을 사용할 경우 OS와 컨테이너간의 권한 문제가 발생할 수 있다. 이 문제를 해결하기 위해 사용자와 그룹을 설정할 수 있도록 하였다.

호스트의 디렉토리의 소유자가 컨테이너에 전해지는 PUID, GUID와 같도록 하자. 

다음은 호스트의 uid와 gid를 알아내는 방법이다.

```bash
$ id username 
uid=1000(dockeruser) gid=1000(dockergroup) groups=1000(dockergroup)
```

## 애플리케이션 설치

웹UI는 `$YOUR_IP:32400/web` 으로 접속할 수 있다. 

