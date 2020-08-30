# redis install

소스파일 다운로드 

```
wget "http://download.redis.io/releases/redis-5.0.7.tar.gz"
```

압축을 푼다. 


```
tar -xvzf redis~~~
```
make를 수행하여 소스를 컴파일한다.
```
make
```

`make test` 명령어로 정확히 설치가 됐는지 확인가능하다. (단, 이를 위해 tcl이 설치되어 있어야 한다.`yum install tcl`명령어로 tcl을 설치해도 된다. 

redis-server와 redis-cli는 보편적으로 사용되는 linux설치 폴더에 저장하는 것을 추천한다.

아니면 그냥 `make intall` 명령어를 입력하자.


/usr/local/bin/

## Redis 시작
Redis server를 시작하는 가장 단순한 방법은 단순히 아래 명령어를 실행하는 것이다. 

```
./redis-server
```

위의 명령어는 아무런 명시적인 설정없이 redis server를 시작한다. 그러므로 모든 설정은 기본설정을 따르다. 단순 테스튼나 개발환경이라면 기본설정을 사용하고 운영환경은 반드시 설정파일을 이용하여 상세한 설정을 해야 한다. 

설정과 함께 redis-server를 기동하기 위해서 첫번째 파라미터로 설정파일의 절대경로 입력한다. 예를 들어 다음과 같다. 

```
/usr/local/bin/redis-server /etc/redis.conf
```

설정파일의 샘플은 배포되는 파일의 root디렉토리에 있다. 

## Redis 동작 확인하기 
Redis와 통신하기 위해서는 Redis가 명시한 프코톨콜을 통해 TCP 소켓을 이용해야 한다. 이런 기능은 redis클라이언트 라이브러리에 구현되어 있고 이것을 사용해야 한다. 

하지만 빠르고 간편하게 redis와 통신하기 위한 redis-cli가 제공된다. 

아래 명령어로 redis-cli를 실행하자. 

```
redis-cli
```

그리고 입력창에서 다음과 같은 명령어를 입력해보자. 

```
ping
```

`PONG`이 출력되면 서버가 정상 실행되는 것이다. 

## redis 데이터 타입