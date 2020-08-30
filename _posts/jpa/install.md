# Jpa 설치

##  Maven 설정
아래 2개 Maven Dependency를 추가하면 JPA구현체인 hibernate를 사용할 수 있다. 

```{xml}
	<properties>
		<!-- 기본 설정 -->
		<java.version>1.8</java.version>
		<!-- 프로젝트 코드 인코딩 설정 -->
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

		<!-- JPA, 하이버네이트 버전 -->
		<hibernate.version>5.4.10.Final</hibernate.version>
		<!-- 데이터베이스 버전 -->
		<h2db.version>1.4.200</h2db.version>
	</properties>

	<dependencies>
		<!-- JPA, 하이버네이트 -->
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-entitymanager</artifactId>
			<version>${hibernate.version}</version>
		</dependency>
		<!-- H2 데이터베이스 -->
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<version>${h2db.version}</version>
		</dependency>
	</dependencies>
```

## H2 DB설치

http://www.h2database.com/html/main.html 에 접속한다. 
Window전용 설치버전과 모든 플랫폼 용 무설치 버전이 있다. 
무설치 버전을 받은 후 특정 디렉터리에 압축을 푼다. 

#### 서버모드 실행
압축을 푼 디렉터리에 bin폴더에 들어가서 h2.sh(윈도우는 h2.bat)를 실행한다. 
자동으로 시작되는 웹페이지에서 JDBC URL을 `jdbc:h2:tcp://localhost/~/test`로 수정하면 서버모드로 연결이 시작된다. 