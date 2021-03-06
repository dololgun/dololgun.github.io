---
title: "체크스타일[checkstyle] - 2 : 설정하기"
date: 2020-09-06 09:00:00 +0900
categories: checkstyle
---

## 설정 파일 이해하기

이전 포스팅에서 언급한데로 체크스타일에서 가장 핵심이 되는 부분은 설정파일이다. 거두절미하고 선(sun)에서 제공한 체크스타일의 설정파일을 살펴보자. 설정파일이 너무 길어 반복되는 부분과 주석은 생략하였다.

```xml
<?xml version="1.0"?>
<!DOCTYPE module PUBLIC
          "-//Checkstyle//DTD Checkstyle Configuration 1.3//EN"
          "https://checkstyle.org/dtds/configuration_1_3.dtd">

<module name="Checker">
  <property name="severity" value="error"/>
  <property name="fileExtensions" value="java, properties, xml"/>

  <!-- Excludes all 'module-info.java' files              -->
  <!-- See https://checkstyle.org/config_filefilters.html -->
  <module name="BeforeExecutionExclusionFileFilter">
    <property name="fileNamePattern" value="module\-info\.java$"/>
  </module>

  <!-- Checks for Size Violations.                    -->
  <!-- See https://checkstyle.org/config_sizes.html -->
  <module name="FileLength"/>
  <module name="LineLength">
    <property name="fileExtensions" value="java"/>
  </module>

  <!-- Checks for whitespace                               -->
  <!-- See https://checkstyle.org/config_whitespace.html -->
  <module name="FileTabCharacter"/>

  <module name="TreeWalker">

    <!-- Checks for Javadoc comments.                     -->
    <!-- See https://checkstyle.org/config_javadoc.html -->
    <module name="InvalidJavadocPosition"/>
    <module name="JavadocMethod"/>
    <module name="JavadocType"/>
    <module name="JavadocVariable"/>
    <module name="JavadocStyle"/>
    <module name="MissingJavadocMethod"/>

    <!-- https://checkstyle.org/config_filters.html#SuppressionXpathFilter -->
    <module name="SuppressionXpathFilter">
      <property name="file" value="${org.checkstyle.sun.suppressionxpathfilter.config}"
                default="checkstyle-xpath-suppressions.xml" />
      <property name="optional" value="true"/>
    </module>
  </module>
</module>
```

설정파일의 포맷은 xml이다. 설정파일에서 가장 자주 등장하는 태그는 모듈(module)과 모듈내의 프로퍼티(property)이다. 공식 홈페이지의 정의된 내용을 번역하자면 체크스타일 설정의 개념은 다음과 같다. (오역 주의)

> 체크스타일 설정은 검사를 위해 자바소스에 적용할 모듈을 정의하는 것이다. 모듈은 트리구조로 되어 있고 트리의 루트모듈은 체커(Checker) 모듈이다.

체크스타일 라이브러리에는 다수의 모듈이 들어 있고 각 모듈은 자신만의 역할(기능)이 있다. 이 모듈을 설정파일에 추가하면(plug in) 모듈로서 기능을 할 것이고 그렇지 않다면 아무런 동작을 하지 않을 것이다. 

각 모듈은 고유한 프로퍼티(property)를 가지고 있다. 이 프로퍼티의 값을 변경하면 해당 프로퍼티를 가지고 있는 모듈의 동작도 변경된다. 
즉, 모듈은 넣고 뺄수 있으며 프로퍼티를 통해 설정을 할 수 있다는 것을 알수있다. 이런 조합으로 체크스타일의 설정파일이 완성된다. 우리는 체크스타일이 제공하는 모듈의 기능과 설정방법만 알면된다. 

공식홈페이지의 많은 부분이 모듈에 대한 설명과 설정 방법에 대해 설명하고 있다. 

> 왼쪽 사이드바에서 Documentation > Configuration > Checks 부분이 모듈에 대한 설명이다.

이 포스트에서 모든 모듈에 대해서 설명할 수 없고(큰 의미가 없다) 모든 모듈에 대해 알아야 할 필요도 없다고 생각한다. 그저 필요한 기능이 있으면 그 기능을 제공하는 모듈이 있는지 찾아보고 있다면 설정에 적용하면 그만이다. 

그래도 대표적인 모듈에는 어떤것이 있으며 프로퍼티는 무엇이고 어떻게 설정하는지 좀 더 알아보도록 하자.

## 모듈

체크스타일의 모듈은 자바 클래스로 구현되어있다. 모듈의 이름이 클래스의 이름이며 체크스타일이 실행 될 때 이 클래스가 인스턴스화되어 동작한다. 클래스이기 때문에 필드와 필드의 getter, setter도 가지고 있다. 프로퍼티를 설정하는 것도 결국 setter를 이용하여 클래스 필드의 값(value)을 변경하는 것이다. 

체크스타일은 모듈을 세분화 하여 필터와 체크로 구분하였다. 자세한 내용은 잠시 후 설명하겠다.

## 프로퍼티

설정파일(xml)에 저장되는 프로퍼티는 모두 문자열로 된 값이다. 문자열 값은 실제 체크를 수행하는 모듈에 바인딩 될 때 모듈에 정의된 타입으로 매핑된다. 예를 들어, 모듈에서  optional이란 boolean타입의 필드를 가지고 있다면 설정파일에는 다음과 같이 작성할 수 있다. 

```xml
<property name="optional" value="true"/>
```

배열도 사용할 수 있다. 배열을 구분하는 딜리미터는 `,`이다. 예를 들어 fileExtensions라는 String 배열이 있다면 다음과 같이 여러개의 값을 설정할 수 있다. 

```xml
<property name="fileExtensions" value="java, properties, xml"/>
```

### 오류 수준 조절하기

Serverity 프로퍼티를 변경하면 오류수준으로 제어할 수 있다. Severity레벨은 4개를 설정할 수 있다. 

- ignore
- info
- warning
- error

```xml
<property name="severity" value="warn"/>
```

## 필터

필터는 위에서 언급한 모듈의 한 종류이다. 필터는 소스를 검사할지 하지 않을지를 세부적으로 설정할 수 있도록 한다. 우리가 가지고 있는 소스 디렉터리에서 특정 파일을 제외하거나 소스 파일의 특정 영역을 제외하고 싶을 때 필터를 사용한다. 설정파일에서 모듈의 이름이 Filter로 끝난다면 필터이다.

대표적인 필터(그냥 내가 써본 필터)를 소개하겠다.

### SuppressiongCommentFilter

이 필터를 사용하면 주석을 이용하여 특정 영역은 소스를 검사하지 않도록 할 수 있다. 다음 예를 보자. 

```java
// CHECKSTYLE:OFF

... 소스 ...

// CHECKSTYLE:ON
```

주석 CHECKSTYLE:OFF와 CHECKSTYLE:ON사이의 소스는 체크를 하지 않는다. 주석에 들어가는 키워드는 설정 파일의 offCommentFormat, onCommentFormat의 값을 수정하여 변경할 수 있다.

### SuppressionSingleFilter

필자가 가장 유용하게 사용한 필터이다. 이 필터는 특정 유형의 파일을 특정 모듈이 체크하지 않도록 설정하는 기능을 가지고 있다. 예를 들어, 서비스 레이서에서 동작하는 클래스와 Dao로 동작하는 클래스는 기능이 다른 만큼 체크를 해야 하는 범위도 다르다. A라는 체크 모듈이 Dao클래스에서 동작하지 않도록 아래와 같이 설정 할 수 있다.

```
<module name="SuppressionSingleFilter">
  <property name="checks" value="A"/>
  <property name="files" value="*Dao.java"/>
</module>
```

이 필터의 더 훌륭한 점은 다양한 조건에 따라 여러번 설정 할 수 있다는 것이다. 이번에는 B라는 체크를 서비스클래스에서 동작하지 않도록 하고 싶은 경우 아래와 같이 기존 설정에 추가하여 설정할 수 있다.

```
<module name="SuppressionSingleFilter">
  <property name="checks" value="A"/>
  <property name="files" value="*Dao.java"/>
</module>
<module name="SuppressionSingleFilter">
  <property name="checks" value="B"/>
  <property name="files" value="*Svc.java|*SvcImpl.java"/>
</module>
```

> 위의 설정은 Dao로 동작하는 클래스의 접미사는 Dao, 서비스로 동작하는 클래스의 접미사는 Svc로 주도록 표준을 정하였기 때문에 가능한 설정이다. 만약에 서비스클래스와 DAO클래스를 구별하기 위한 어떤한 이름짖기 규칙이 없다면. 위와 같은 설정은 불가능하다.

이 외에도 다양한 상황에 사용할 수 있는 필터가 많이 있고 공식 홈페이지에 자세하게 설명이 되어 있으니 참고하도록 하자.

## 체크(check)

모듈 이름이 필터로 끝나지 않는 모든 모듈은 체크모듈이다. 체크모듈은 말 그대로 소스를 체크하여 규칙에 부합하는지를 검사한다. 매우 다양한 체크가 제공되어 있으며 어떤 체크가 있는지 공식홈페이지를 찾아보도록 하자. 몇 가지 알아야 하는 체크 모듈은 살짝 이야기 한다. 

### Checker

앞에서 소개한 설정파일에서 루트에 선언되어 있는 모듈이다. 이 모듈이 소스의 내용을 검사하는 것은 아니다. 단, 이 모듈을 통해서 필터 모듈이 호출되고 바로 다음에 설명할 TreeWalker가 호출된다. 또한, 이 모듈의 프로퍼티를 수정하여 전역 설정을 할 수 있도록 한다. 예를 들어, severity는 체크규칙에 걸린 경우 심각도를 설정 할 수 있는데 이것은 전역적인 설정이며 이런 설정을 Checker에서 하도록 되어있다. 

### TreeWalker (트리워커)

위의 설정파일을 자세히 살펴보면 대부분의 체크모듈이 트리워커 내에 선언된 것을 볼 수 있다. (트리워커와 동등한 레벨에는 필터가 있고 이 모든 것을 포함하고 있는 것은 Checker이다) 내 경험에 의하면 하나의 자바소스를 검사한다고 할 때 트리워커가 소스의 특정 토큰(if, String과 같이 소스내에서 의미를 가질 수 있는 가장 작은 문자)마다 트리워커 내부에 설정되어 있는 체크모듈을 호출하는 것 같다. 커스텀체크를 만들지 않는다면 이런 역할정도만 알아도 체크스타일을 사용하는데 큰 문제는 없다.

## 마무리

지금까지 체크스타일의 설정에 대해서 알아보았다. 이전 포스트에서 소개한 체크스타일 실행 방법을 이용하여 설정 파일의 다양한 모듈을 적용해보면 체크스타일이 어떻게 동작하는지 충분히 파악할 수 있다고 생각한다. 다시 한번 말하지만 나도 모든 모듈의 동작에 대해서 아는 것이 아니다. 필요하면 그때 그때 찾아서 적용해 보도록 하자. 

다음 포스트에서는 메이븐을 이용하여 체크스타일을 더욱 편하게 적용할 수 있는 방법에 대해서 알아보고자 한다.


## 참조

https://checkstyle.sourceforge.io/