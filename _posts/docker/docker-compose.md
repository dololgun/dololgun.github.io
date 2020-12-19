# 도커 컴포즈 사용하기

## 개요

컴포즈는 다수의 컨테이너를 실행을 미리 정의하기 위한 툴이다. YAML파일을 이용하여 컨테이너를 실행하는데 필요한 정보를 미리 설정할 수 있고 재활용할 수 있는 장점이 있다. 

컴포즈를 사용하는 것은 기본적으로 3단계를 따른다 : 

1. dockerfile로 컨테이너를 정의한다. 
2. docker-compose.yml 파일을 작성하여 컨테이너 실행을 정의한다.
3. `docker-compose up` 명령어를 사용하여 정의된 컨테이너를 실행한다. 

docker-copose.yml 의 예시는 다음과 같다. 

```yaml
version: "3.8"
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

컴포즈 파일은 컨테이너의 라이프사이클 관리를 위한 명령어를 가지고 있다. 

* start, stop, rebuild 서비스
* 실행중인 서비스 상태 보기
* 실행중인 서비스 로그 스트림
* 일회성 명령어 실행

## 설치하기

### 필요사항

도커엔진, 의미있는 작업, 즉, 도커가 설치되어 있어야 함

* 컴포즈를 non-root 유저로 실행하기 위해 다음 [링크](https://docs.docker.com/engine/install/linux-postinstall/)를 참조하라.

### 리눅스 설치

curl 도구를 이용하여 도커 컴포즈를 받는다. 

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

> 참조 : 다른 버전을 받고 싶으면 위의 '1.27.4' 를 원하는 버전으로 변경

실행파일의 권한을 할당하자.

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

### 업그레이드

도커 컴포즈를 새로운 버전으로 업그레이드 하고 싶다면 그냥 위 설치 과정을 되풀이 하면 된다. 단, 아주 예전 버전을 사용하고 있다면 다음을 고려해야 한다.

도커 컴포즈 1.2이하 버전에서 업그레이드 하는 경우 기존 컨테이너들은 새로운 버전에서 동작하지 않는다. 

## 시작하기

### 단계1 : Setup

1. 디렉토리를 생성
2. 

### 단계2 : 도커파일 생성

### 단계3 : 컴포즈 파일 작성

### 단계4 : 컨테이너 실행 및 삭제

*  `docker-compose up`
*  `docker-compose down`

### 단계5 : 컴포즈. 파일 수정

### 단계6 : 컨테이너  다시 시작

### 단계7 : 어플리케이션 수정

### 단계8 : 다른 명령어 실험

서비스를 백그라운드 모드로 실행하고 싶다면 `-d` 옵션을 사용하자.

일회성 명령어를 사용하고 싶다면 `run 컨테이너명 명령어` 를 사용하자.

서비스를 중지하고 싶다면 `stop` 을 사용하자. 

컨테이너를 삭제하고 싶다면 `down`을 사용하자. `--volumes` 를 사용하면 볼륨의 데이터도 함께 삭제 된다. 





