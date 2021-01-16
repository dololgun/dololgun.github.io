---
title: "Mac OS에 하나 이상의 다른 버전의 JDK 설치하기"
date: 2021-01-16 09:00:00 +0900
categories: macos
---

이미 JDK가 설치되어 있는 환경에서 또 다른 JDK의 설치가 필요한 경우 고려해야 할 사항을 정리해보았다. MacOS에 기반하고 있으며 리눅스나 윈도우는 비슷하지만 다른 방법으로 관리한다.

### 자바 버전 및 위치 확인하기

아래와 같은 명령어를 사용하면 지금 로그인한 사용자가 실행할 수 있는 java버전이 나타난다. 또한, which명령어를 사용하여 java 바이너리의 위치를 확인할 수 있다. 만약, java 명령어를 인식하지 못한다면 java가 설치되어 있지 않거나 path 설정이 되어 있지 않다고 생각하면 된다.

```bash
$ java -version
openjdk version "1.8.0_265"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_265-b01)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.265-b01, mixed mode)

$ which java
/usr/bin/java
```

아래와 같이 path에 java가 있는 경로가 설정되어 있으니 전체경로 없이 java라는 명령어만으로도 정확히 해당 명령어를 실행할 수 있다. 추가로 내 계정에 설정되어 있는 path가 무엇인지 확인하기 위해서는 path변수의 값을 확인하면 된다.

```bash
$ echo $path
/Users/geonho/.rbenv/shims /usr/local/bin /usr/bin /bin /usr/sbin /sbin /Applications/VMware Fusion.app/Contents/Public
```

이제, /usr/bin 경로로 이동해서 java 파일을 확인하자. java와 관련된 바이너리가 보이는데 흥미롭게 모두 심볼릭링크로 설정되어 있다. 이 링크가 가리키는 방향은 `/System/Library/Framworks` 폴더다. 운영체제마다 jdk를 관리하는 방법이 다른데, 맥은 /System/Library/Frameworks/JavaVM.framework 에서 버전별로 관리하는 느낌이다(단순 추정이다). 마치 리눅스에서 alternatives를 사용하는 것과 마찬가지로.

```bash
jar -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/jar
jarsigner -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/jarsigner
java -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/java
javac -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/javac
javadoc -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/javadoc
javah -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/javah
javap -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/javap
javapackager -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/javapackager
javaws -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/javaws
```

좀 더 정확히 알아보기 위해 `/System/Library/Frameworks/JavaVM.framework/Versions/A` 폴더를 가면 자바와 관련된 라이브러리가 보인다. 그런데, 실제 자바라이브러리가 설치된 경로는 여기가 아니다. 진짜 경로는 다른 방법으로 구해야 한다. 다음 경로의 있는 명령어를 사용하자. 실제 자바가 설치되어 있는 경로는 바로 다음과 같다. 

```bash
$ /usr/libexec/java_home
/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
```

해당 폴더로 이동해보면 우리가 자주 접했던 자바의 홈 디렉토리의 구조를 볼 수 있다. 

```
-r--r--r--   1 root  wheel      1522  7 29 00:20 ASSEMBLY_EXCEPTION
-r--r--r--   1 root  wheel     19274  7 29 00:20 LICENSE
-r--r--r--   1 root  wheel    152996  7 29 00:20 THIRD_PARTY_README
drwxr-xr-x  45 root  wheel      1440  7 29 00:21 bin
drwxr-xr-x   3 root  wheel        96  7 29 01:39 bundle
drwxr-xr-x   9 root  wheel       288  7 29 00:20 include
drwxr-xr-x   7 root  wheel       224  7 29 00:20 jre
drwxr-xr-x   9 root  wheel       288  7 29 00:20 lib
drwxr-xr-x   5 root  wheel       160  7 29 00:20 man
-rw-r--r--   1 root  wheel        87  7 29 00:20 release
drwxr-xr-x  12 root  wheel       384  7 29 00:20 sample
-rw-r--r--   1 root  wheel  52658253  7 29 00:20 src.zip
```

왜 이런거까지 알아야 하는지 이해할 수 없을지도 모르겠지만, 하나의 운영체제의 다른 버전의 자바라이브러리를 한 개 이상 설치하고 자유롭게 버전을 변경하기 위해서는 알면 더 좋다. 사실 몰라도 강제로 라이브러리 위치를 잡아주면 되지만 라이브러리 관리가 점점 복잡해진다.

### JDK 추가 설치

상황이 이렇다는 것을 알아두고, 추가로 다른 jdk를 설치해보자. 필자는 weblogic 관련된 설치를 하기 위해 oracle jdk를 home brew를 이용하여 설치해 보았다.

```bash
$ brew search oracle
==> Casks
color-oracle                                       navicat-for-oracle                                 oracle-jdk                                         oracle-jdk-javadoc
```

설치할 수 있는 oracle-jdk가 있는데 Casks를 이용하여 설치해야 한다. 다음과 같은 명령어로 설치를 하자. 

```bash
$ brew install --cask oracle-jdk

...중략...
installer: Package name is JDK 15.0.1
installer: Installing at base path /
installer: The install was successful.
🍺  oracle-jdk was successfully installed!
```

위와 같이 base path `/`에 설치되었다고 나온다. 그런데 `/`가면 추가로 설치된 파일이 보이지 않는다.

자바 버전을 다시 확인해보자.

```bash
$ java --version
java 15.0.1 2020-10-20
Java(TM) SE Runtime Environment (build 15.0.1+9-18)
Java HotSpot(TM) 64-Bit Server VM (build 15.0.1+9-18, mixed mode, sharing)
```

이런, java의 버전이 바뀌었다. 포스트의 처음에 했던 과정을 되풀이하여 java 바이너리가 어디에 있는지 확인해보자.

```bash
$ /usr/libexec/java_home
/Library/Java/JavaVirtualMachines/jdk-15.0.1.jdk/Contents/Home
```

확인을 해보니 자바 홈의 경로가 위와 같이 바뀌었다. `/Library/Java/JavaVirtualMachines` 로 이동하면 아래와 같이 2개의 자바라이브러리가 설치되어 있다.

```bash
drwxr-xr-x  3 root  wheel    96B  8 17 16:01 adoptopenjdk-8.jdk
drwxr-xr-x  3 root  wheel    96B  1 16 14:45 jdk-15.0.1.jdk
```

그럼 어떤 설정이 운영체제의 현재의 자바 버전이 바뀌도록 하였을까? 인터넷 검색을 한 결과 별도의 설정이 없다면 운영체제가 각 자바 버전의 `Info.plist` 파일을 확인하여 가장 최신 버전을 기본으로 사용한다고 한다. 예를 들어, jdk-15의 Info.plist(`/Library/Java/JavaVirtualMachines/jdk-15.0.1.jdk/Contents` 에 있다) 파일과  adoptopnejdk-8의 Info.plist 파일을 비교하여 가장 최신버전을 선택한다는 것이다. 

### 특정 JDK 비활성화 하기

최신버전이 jdk-15이니깐 이것의 Info.plist 파일을 info.plist.disable 파일로 변경해보자. 

```bash
$ sudo mv Info.plist Info.plist.disable
```

  자바 버전을 검색해보자.

```bash
$ java -version
openjdk version "1.8.0_265"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_265-b01)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.265-b01, mixed mode)
```

와우, 다시 예전의 자바버전으로 돌아왔다. 하지만, 이렇게 할 경우 운영체제 전 영역에서 jdk-15는 사용할 수 없다. 만약 서로 다른 유저가 서로 다른 자바 버전을 사용할 수 없다. 

유저별로 다른 자바 버전을 사용하는 방법도 존재한다. 우선 비활성화 시켰던 jdk-15를 다시 활성시키자.

```bash
$ sudo mv Info.plist.disable Info.plist

$ java -version
java 15.0.1 2020-10-20
Java(TM) SE Runtime Environment (build 15.0.1+9-18)
Java HotSpot(TM) 64-Bit Server VM (build 15.0.1+9-18, mixed mode, sharing)
```

 ### 유저별로 사용하는 JDK 설정하기

특정 유저에게 JDK를 지정하고자 하면 해당 `JAVA_HOME` 변수를 설정해야 한다. 이 변수가 java 플랫폼에서는 약속인것 같다. 이 변수가 설정이 안되어 있다면 이전 단계에서 언급한 운영체제의 설정을 따른다. 

유저의 홈디렉토리에 `.profile` 을 생성하고 다음 내용을 추가한다. 이미 해당 파일이 생성돼 있을 수도 있다.

```shell
# jdk 1.8버전을 사용하는 유저
export JAVA_HOME=`/usr/libexec/java_home -v 1.8`
```

위의 명령어는 JAVA_HOME 변수를 1.8버전으로 설정하는 것이다.

 .profile 을 선택한 이유는 쉘과 상관없이 이 유저에게는 이 설정을 적용하기 위해서다. 자세한 내용은 [쉘(Shell)의 초기화 스크립트 동작 방식 정리](../linux/2021-01-15-shell-profile.md) 를 참고하자.

다음 로그인 부터는 이 설정이 적용되고 현재에도 이 설정을 적용하고자 한다면 source 명령어를 사용한다.

```bash
$ source ~/.profile
```

그리고 다시 자바 버전을 확인하면 버전이 변경된 것을 확인할 수 있다.

```bash
$ java -version
openjdk version "1.8.0_265"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_265-b01)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.265-b01, mixed mode)
```

## 참조

https://apple.stackexchange.com/questions/188485/setting-java-correctly-on-mac