# Redis를 서비스로 실행하기 

Centos를 기준으로 설명한다. 

전제조건은 다음과 같다. 

redis 설치파일 및 빌드된 폴더 : /home/redis/redis-5.0.7
redis-server 경로 : /usr/local/bin/redis-server
redis-cli 경로 : /usr/local/bin/redis-cli

이와 같은 상황에서도 redis-server는 실행이 가능하지만 보다 전문적인 사용 환경을 구축하기 위해서는 적합하지 않은 설정이다. 

### 1. redis의 설정 파일 지정

redis 서버가 실행되는 port 이름으로 설정파일을 구성하자.
```
cp /home/redis/redis-5.0.7/redis.conf /etc/reids/6379.conf
```

### 2. redis data파일 저장 위치 생성

redis가 in-memory database이지만 생성된 data를 특정 위치에 저장이 가능하다. 

```
mkdir /var/redis/6379/
```

### 3. redis 설정 파일 수정

##### bind
1번 과정에서 복사한 redis 설정 파일을 수정하자. 

기본으로 내부에서만 접속 가능하도록 되어 있지만 외부에서 접속 가능하도록 bind 설정을 삭제하자. 
```
# bind 127.0.0.1
```
> 주의 : bind 설정을 사용하지 않으면 불특정 다수가 redis에 접속할 수 있기 때문에 redis는 자동으로 protected-mode로 설정된다. 

##### daemonize
daemonize는 서비스로 redis를 실행하기 위해서 꼭 yes로 설정해야 한다. 
daemonize=yes

##### supervised
무슨 역할인지 잘 모르겠지만 일단 수정할 필요없다.

##### loglevel
로그를 남기는 수준을 결정한다. 
```
loglevel debug
```
##### logfile
로그가 생성되는 파일의 위치를 지정한다
```
logfile /var/log/redis_6379.log
```
##### dir
data가 저장되는 위치를 지정한다.
```
dir /var/redis/6379
```
##### requirepass 
클라이언트에 인증을 위한 비밀번호를 설정한다. detected-mode인 경우 필수가 된다.
```
requirepass 4532+
```