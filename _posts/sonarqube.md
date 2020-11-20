# 개요

소나큐브는 버그를 자동으로 찾기 위한 도구이다. 아래 그림과 같이 우리가 작성한 코드를 commit/merge 하면 형상관리 툴에 저장이되고 젠킨스와 같은 CI/CD도구가 코드를 컴파일/빌드하고 소나큐브에 검사를 의뢰하게 된다. 

![SonarQube Instance Components](../assets/images/post/sonar-qube/dev-cycle.png)

# 설치

소나큐브는 3가지로 구성되어 있다. 

* 스캐너
* 소나큐브 서버
* 데이터베이스 서버

![SonarQube Instance Components](../assets/images/post/sonar-qube/SQ-instance-components.png)

### 호스트와 위치

소나큐브 서버와 데이터 베이스는 별도의 호스트로 분리되는 것이 권장된다. 

### 데이터 베이스 설치

다음 3가지 데이터 베이스를 사용할 수 있다. 

* Microsoft SQL Server
* Oracle
* PostgreSQL

### ZIP파일로 소나큐브 설치하기

요구사항을 체크하자.

* Oracle JRE 11 or OpenJDK 11

다음 [링크](https://www.sonarqube.org/success-download-community-edition/)에서 파일을 받아 압축을 해제하자.(숫자로 시작하는 폴더명을 사용하지 말자)

소나큐브는 root계정을 실행할 수 없으니 적절한 계정을 생성하자.

*$SONARQUBE-HOME(홈폴더)* 는 위의 파일이 압축이 풀린 폴더를 가리킨다.

#### 데이터 베이스 접속

*$SONARQUBE-HOME/conf/sonar.properties*를 설정한다. 

이미 제공된 JDBC드라이버가 있으니 일부러 변경하지 말자. (오라클은 제외)

오라클은 알맞은 드라이버를 *$SONARQUBE-HOME/extensions/jdbc-driver/oracle* 에 복사하자.

#### 엘라스틱 서치를 위한 저장소 설정

기본으로 *$SONARQUBE-HOME/data* 로 설정되어 있지만, 별도의 빠른 I/O가 가능한 경로로 변경하는 것을 추천한다. 

*$SONARQUBE-HOME/conf/sonar.properties* 파일의 다음 항목을 변경한다. 

```
sonar.path.data=/var/sonarqube/data
sonar.path.temp=/var/sonarqube/temp
```

#### 웹 서버 시작

기본 포트는 9000이고 컨텍스트 패스는 `/` 이다. *$SONARQUBE-HOME/conf/sonar.properties* 를 수정하여 변경할 수 있다. 

```
sonar.web.host=192.0.0.1
sonar.web.port=80
sonar.web.context=/sonarqube
```

다음 스크립트를 실행하자. 

* On Linux: bin/linux-x86-64/sonar.sh start
* On macOS: bin/macosx-universal-64/sonar.sh start
* On Windows: bin/windows-x86-64/StartSonar.bat

기본 운영자 계정은 admin/admin이다.

#### Java 버전 적용하기

만약 서버에 여러 버전의 자바가 설치되어 있다면 알맞은 자바 버전을 설정해야 한다. 

*$SONARQUBE-HOME/conf/wrapper.conf* 파일을 수정한다. 

```
wrapper.java.command=/path/to/my/jdk/bin/java
```

### 도커 이미지로 설치하기

1. 도커 이미지의 특성상 데이터를 유실하지 않기 위해 다음 설정을 한다. 

   * `sonarqube_data` – 엘라스틱 서치 인덱스, H2 데이터베이스를 위한 데이터 파일
   * `sonarqube_logs` – 소나큐브 로그(접속, 웹 프로세스, CE프로세스, 엘라스틱서치)
   * `sonarqube_extensions` – 설치한 플러그인과 오라클 JDBC

   다음 명령어로 볼륨을 생성하자.

   ```bash
   $> docker volume create --name sonarqube_data
   $> docker volume create --name sonarqube_logs
   $> docker volume create --name sonarqube_extensions
   ```

> 주의: 위의 커맨드로 볼륨을 설정하는 것을 명심해라. bind mount를 사용하지 말라. 플러그인 관련된 문제가 발생할 수 있다.

2. 오라클을 제외하고 데이터베이스 드라이버는 이미 제공되고 있다. 만약에 오라클을 사용한다면 sonarqube_extensions 폴더에 oracle jdbc드라이버를 설치해야 한다. 이를 위해 다음 단계를 따라하자. 

   * 임베디드 h2 데이터베이스를 사용하는 소나큐브 도커 컨테이너를 시작

   ```bash
   $ docker run --rm \
       -p 9000:9000 \
       -v sonarqube_extensions:/opt/sonarqube/extensions \
       <image_name>
   ```

   * 일단 시작뙜으면 그냥 끈다.
   * `sonarqube_extensions/jdbc-driver/oracle` 에 오라클 JDBC 드라이버를 복사한다.

3. -e 환경변수를 사용하여 도커 컨테이너를 기동한다. 

```bash
$> docker run -d --name sonarqube \
    -p 9000:9000 \
    -e SONAR_JDBC_URL=... \
    -e SONAR_JDBC_USERNAME=... \
    -e SONAR_JDBC_PASSWORD=... \
    -v sonarqube_data:/opt/sonarqube/data \
    -v sonarqube_extensions:/opt/sonarqube/extensions \
    -v sonarqube_logs:/opt/sonarqube/logs \
    <image_name>
```

> 위의 환경 변수는 추후 사라질 것이다. 

#### 도커 컴포즈를 사용하는 경우

다음 yml파일을 사용하자. 

```yaml
version: "3"

services:
  sonarqube:
    image: sonarqube:8-community
    depends_on:
      - db
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs
      - sonarqube_temp:/opt/sonarqube/temp
    ports:
      - "9000:9000"
  db:
    image: postgres:12
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data

volumes:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:
  sonarqube_temp:
  postgresql:
  postgresql_data:
```

### 플러그인 설치

## 소스코드 분석하기



# 출처

https://docs.sonarqube.org/