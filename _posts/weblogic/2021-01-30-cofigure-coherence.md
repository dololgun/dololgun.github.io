---
title: "코히어런스(Coherence) 기본 설정하기"
date: 2021-01-30 09:00:00 +0900
categories: weblogic
---

코히어런스 캐시 서버의 기본적인 설정을 적용해본다.

## 설정 이해하기

이전 포스팅  [코히어런스 설치하기](2021-01-17-install-coherence.md)에서 살짝 언급했지만 코히어런스를 이해하는 것의 대부분은 설정하는 방법을 아는 것이다. 코히어런스의 핵심 라이브러리인 `coherence.jar` 파일안에 기본 설정 파일(xml)이 포함되어 있고 우리는 이 파일과 별도로 설정 파일을 생성하여 적용할 수 있다. 만약 아무런 설정을 하지 않는다면 기본 설정 파일로 코히어런스가 동작한다.

코히어런스는 다양한 설정파일을 가지고 있지만 그 중 꼭 알아야 하는 설정파일은 `Operation Configuration File`, `Cache Configuration File` 이다.  

### Operation Configuration File 

`coherence.jar` 파일안에 `tangosol-coherence.xml` 에 관련된 설정이 있다. 설정 파일의 이름에서 알 수 있듯이 코히어런스가 동작하는 모든 행위에 대한 설정을 한다. 

가이드에서는 이 설정 파일은 그냥 두고 변경해야 할 설정이 있다면 오버라이드 파일을 생성하여 적용하는 것을 추천한다. 즉, 변경할 설정이 있다면 오버라이드 파일을 만들고 코히어런스에게 이런 오버라이드 파일이 있으니 그 파일을 보라고 하는 것이다. 

코히어런스는 `tangosol-coherence-override.xml` 명칭의 파일이 클래스 패스에 있다면 알아서 오버라이드 파일로 인식하고 로드한다. 

그럼 오버라이드 파일을 한번 만들어보자.

* `$COHERENCE_HOME/config` 폴더를 만든다.
* 위의 폴더에 tangosol-coherence-override.xml을 만들고 아래와 같이 작성한다.

```xml
<?xml version='1.0'?>

<!--
This operational configuration override file is set up for use with Coherence in
a development mode.
-->
<coherence xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://xmlns.oracle.com/coherence/coherence-operational-config" xsi:schemaLocation="http://xmlns.oracle.com/coherence/coherence-operational-config coherence-operational-config.xsd">
    <cluster-config>
        <member-identity>
            <cluster-name system-property="coherence.cluster">leegeonho</cluster-name>
        </member-identity>
    </cluster-config>

    <configurable-cache-factory-config>
        <init-params>
            <init-param>
                <param-type>java.lang.String</param-type>
                <param-value system-property="coherence.cacheconfig">/Users/geonho/Oracle/Middleware/Oracle_Home/coherence/config/sample-cache-config.xml</param-value>
            </init-param>
        </init-params>
    </configurable-cache-factory-config>
</coherence>
```

* `$COHERENCE_HOME/bin/cache-server.sh` 파일의 가장 아래부분을 수정하여 클래스 패스 옵션(-cp 옵션)에 $COHERENCE_HOME/config 을 추가한다.

```sh
... 중략 ...

$JAVAEXEC -server -showversion $JAVA_OPTS $COHERENCE_OPTS -cp "$COHERENCE_HOME/lib/coherence.jar:$COHERENCE_HOME/config" com.tangosol.net.DefaultCacheServer "$@"

```

* cache-server.sh 를 실행하여 캐시 서버를 기동한다. 

캐시 서버가 기동 될 때 로그를 잘 보면 아래와 같이 tangosol-coherence-override.xml 이 로드 된 것을 확인할 수 있다. 

```bash
2021-01-30 22:00:55.907/1.389 Oracle Coherence 14.1.1.0.0 <Info> (thread=main, member=n/a): Loaded operational overrides from "file:/Users/geonho/Oracle/Middleware/Oracle_Home/coherence/config/tangosol-coherence-override.xml"
```

tangosol-coherence-override.xml 설정파일에 정의된 내용을 간략히 설명하도록 하겠다.

#### cluster-name

`cluster-name` 은 말그래도 클러스터 이름을 지정하는 것이다. 코히어런스는 무조건 클러스터를 구성하여 실행된다. 이 말은 우리가 클러스터를 지정하지 않는다면 코히어런스가 알아서 클러스터 이름을 지정한다. 위의 설정에서 클러스터 이름을 leegeonho로 지었으므로 코히어런스 실행 시 아래와 같은 로그를 확인할 수 있다. 

```bash
2021-01-30 22:01:00.777/6.259 Oracle Coherence GE 14.1.1.0.0 <Info> (thread=Cluster, member=n/a): Created a new cluster "leegeonho" with Member(Id=1, Timestamp=2021-01-30 22:00:57.442, Address=192.168.1.67:9900, MachineId=10845, Location=process:49361, Role=CoherenceServer, Edition=Grid Edition, Mode=Development, CpuCount=4, SocketCount=4)
```

#### configurable-cache-factory-config

기본 설정된 coherence-cache-config.xml 파일을 오버라이드 하기 위해 사용한다. 자세한 내용은 cache configuration file에서 설명한다. 

### Cache Configuration File

이 파일에서 코히어런스 캐시에 대한 설정을 할 수 있다. 기본 설정은 `coherence.jar` 파일안에 `coherence-cache-config.xml` 에 있다. 물론 이 파일이 아닌 다른 설정 파일도 사용할 수 있다. 이를 위해 `tangosol-coherence-override.xml` 파일에 `configurable-cache-factory-config` 항목에 파일의 경로를 입력한다.

 



## 참조

https://docs.oracle.com/en/middleware/fusion-middleware/coherence/12.2.1.4/develop-applications/understanding-configuration.html#GUID-C5335E66-6D7F-4C15-B7EC-F6D7D1494066