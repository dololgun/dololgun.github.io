---
title: "체크스타일(checkstyle) - 1"
date: 2020-09-06 09:00:00 +0900
categories: checkstyle
---

## 개요

체크스타일은 프로그래머가 자바코드를 작성할 때 일반적으로 지켜야 하는 코딩 표준을 잘 준수할 수 있도록 지원하는 유틸이다. 대표적인 예로 카멜케이스 규칙을 들수 있다. 자바 코드를 작성할 때 변수명은 카멜케이스를 지키는 것을 원칙으로 하지만 실제로 이 규칙을 어겨도 컴파일 오류가 발생하지는 않는다. 다시 말해, 이 클립스와 같은 개발 툴에서 이 규칙을 잘 지키고 있는지 별도의 알림을 주지 않으며 이것을 확인하는 작업은 지루하고 어려운 일이다.

체크스타일은 위에서 언급한 코딩 표준을 확인하는 작업을 자동으로 수행한다. 확인이 필요한 다양한 항목을 지원하고 쉽게 변경할 수 있다. 또한, 코딩을 통해 우리가 직접 새로운 규칙을 생성하여 적용할 수 있다.

## 커스텀 체크

내가 체크스타일에 관심을 가지게 된 이유는 방금전에 언급한 **직접 새로운 규칙을 작성하여 적용**하는 부분이다. 체크스타일에서는 이것을 **커스텀 체크**라고 부른다. 일반적으로 프로젝트를 진행하게 되면 기본적인 자바 코딩 표준을 지키는 것 이외에도 프로젝트 자체에서 적용하는 다양한 규칙이 있다. 패키지 명칭에 대한 규칙, 클래스 명칭에 대한 규칙, 함수명에 대한 규칙, 코멘트 규칙 등등이 대표적인 예이다. 좀 더 구체적인 예를 들면, 서비스 레이어로 동작하는 서비스 클래스들은 클래스명의 마지막을 SvcImpl로 끝나도록 하고 이 클래스가 참조하는 인터페이스는 Svc로 끝난다. 또한, 이런 클래스들은 꼭 service라는 이름의 패키지내에 구현되어야 한다. Dao클래스의 명칭이 Dao로 끝나는 것도 비슷한 예이다. 이런 규칙은 체크스타일이 제공하는 기본 기능으로는 확인할 수 없고 직접 커스텀 체크를 작성하여 확인할 수 있다.  

## 기본 설치 및 활용

사실 설치라는 말은 필요없다. 왜냐하면 라이브러리를 받고 그것을 실행하면 그만이기 때문이다. 단, 라이브러리는 자바언어로 작성되었기 때문에 자바 라이브러리를 실행할 수 있는 환경은 구성되어야 한다.

어쨋건 체크스타일을 실행하기 위해 최소한 다음 항목이 필요하다.

1. 체크스타일 라이브러리
2. 체크스타일 설정파일
3. 체크 대상파일

### 체크스타일 라이브러리

홈페이지에 방문하여 라이브러리 파일을 받을 수 있다.([링크](https://checkstyle.org/#Download)) 받은 라이브러리는 `checkstyle-8.36-all.jar`라는 이름의 파일이다.

> 이 글을 작성하는 시점의 최신 버전은 8.36이다.

### 체크스타일 설정파일

"어떤 항목을 어떤 기준으로 체크할 것인가?"를 설정하는 파일이다. 사실상 체크스타일의 가장 중요한 파일이라고 할 수 있다. 체크 스타일이 기본으로 제공하는 체크항목이 매우 다양하기 때문에 이 설정파일만 잘 다루어도 체크스타일의 대부분을 알고 있는 것이다. 그만큼 설정 파일이 복잡하고 내용도 많다. 나는 처음 설정파일을 보고 어디서부터 어떻게 시작해야하는지 막막하기만 하였다. 

다행히도, 구글과 선(sun)에서 제공하는 기본 체크스타일 설정파일이 있다. 이들이 제시하는 설정파일로 체크스타일의 검사를 통과한다면 그 코드는 사실상 완벽한 코드라고 할 수 있다. 이 글에서는 선에서 제공하는 설정파일을 기준으로 체크스타일을 돌려보도록하겠다. [다운로드](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/sun_checks.xml) 

### 체크 대상파일

아무 자바소스라도 체크 대상이 될 수 있다.

### 커맨드라인 명령어 실행

기본적인 파일이 준비되었으니 아래와 같이 커맨드 라인 명령으로 체크스타일을 실행할 수 있다. 다시한번 말하지만 자바라이브러리를 실행할 수 있는 환경은 구성되어야 한다. 

```bash
java -cp checkstyle-8.36-all.jar com.puppycrawl.tools.checkstyle.Main -c sun_checks.xml FileUtil.java
```

자바 초보자를 위해 살짝 설명을 하겠다. -cp 옵션으로 클래스패스에 체크스타일 라이브러리를 로드하였고 com.puppycrawl.tools.checkstyle.Main클래스를 실행하였다. 아마 Main클래스안에 main매소드가 있을것이다. -c 옵션으로 체크스타일 설정파일을 로드하였고 마지막으로 체크대상이되는 파일은 FileUtil.java로 하였다.

실행 후 나타나는 메세지는 다음과 같았다. 

 ```bash
Starting audit...
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:1: Missing package-info.java file. [JavadocPackage]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:6:1: Utility classes should not have a public or default constructor. [HideUtilityClassConstructor]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:7: Line has trailing spaces. [RegexpSingleline]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:7:1: File contains tab characters (this is the first instance). [FileTabCharacter]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:14:12: Unused @param tag for 'depth'. [JavadocMethod]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:17: @return tag should be present and have description. [JavadocMethod]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:17: Line has trailing spaces. [RegexpSingleline]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:17: Line is longer than 80 characters (found 88). [LineLength]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:17:48: Parameter root should be final. [FinalParameters]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:17:59: Parameter target should be final. [FinalParameters]
[ERROR] /Users/geonho/Documents/checkstyle/FileUtil.java:21: Line has trailing spaces. [RegexpSingleline]
Audit done.
Checkstyle ends with 11 errors.
 ```

체크 대상이 된 소스는 다음과 같다. 

```java
package app.file;
import java.nio.file.Path;
public class FileUtil {
	/**
	 * target = "/test/test1/test2/test.txt" 이면
	 * root = "/test" 이면
	 * depth가 1이면 "/test/test1"을 리턴한다.
	 * @param root
	 * @param target
	 * @param depth
	 * @return
	 */
	public static Path getRelativeRootPath(Path root, Path target) {	
		Path relativePath = root.relativize(target);
		return root.resolve(relativePath.getName(0));
	}
}
```

이런 소스코드보다 에러메시지가 더 많다. 패키지 커멘트, 생성자, 파일의 마지막 빈줄, 라인 길이 80자, 자바독 커맨트, 매소드의 파라미터 등. 우리가 생각지도 못한 부분까지도 세심하게 또는 과하게 지적을 한다. 다시 말하지만 이런 결함은 모두 설정파일에 따라 달라질 수 있다. 

## 참조

[공식홈페이지](https://checkstyle.sourceforge.io/)

