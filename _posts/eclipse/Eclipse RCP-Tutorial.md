### 1. RCP

자바 기반의 UI 어플리케이션을 이클립스 개발 플랫폼 기반에 만드는 것을 의미한다.

즉, 우리가 Java개발을 위해 활용하는 (흔히 이클립스라고 부른다) 툴도 이클립스 플랫폼을 기반으로 개발된 RCP라고 생각하면 된다.

다음은 그림은 RCP 애플리케이션을 구성하는 컴포넌트를 보여준다.

![이클립스플랫폼](https://www.vogella.com/tutorials/EclipseRCP/img/architecture20.png)

### 2. 설치

#### 2.1. Eclipse SDK 받기

이클립스의 install software 메뉴에서 다음 주소를 입력한다.

```
http://download.eclipse.org/releases/latest
```

아래와 같이 Eclipse e4 Tools Developer Resources를 설치한다.

![image-20210904103546239](../../assets/images/post/eclipse/image-20210904103546239.png)

> 현재 기준으로 최신버전은 4.20.0.v20210427-1802 이다.

#### 2.2. Install the e4 spies

이클립스 애플리케이션을 분석하는데 유용한 e4 spies를 설치한다.

```
http://download.eclipse.org/e4/snapshots/org.eclipse.e4.tools/latest/
```

다수의 플러그인이 나오는데 어떤 것을 설치할지 모르겠다면 일단, 전체 설치를 해보자.

### 3. 연습: 마법사로 RCP 애플리케이션 만들기

이클립스 rcp 애플리케이션을 생성해보도록하자.

file > new > other > plug-in project 선택.

아래와 같이 만든다.

![image-20210904222522791](../../assets/images/post/eclipse/image-20210904222522791.png)

아래아 같이 jdk는 1.8로 설정하고 `This plug-in will make contributions to the UI` 에 체크를 하고 **Crate a rich client application**을 **Yes** 로 한다. 그리고 Next를 선택한다.

![image-20210904222904164](../../assets/images/post/eclipse/image-20210904222904164.png)

아래와 같이 Eclipse RCP application 선택한다.

![image-20210904223056636](../../assets/images/post/eclipse/image-20210904223056636.png)

마지막 위자드에서 `Create sample content` 체크를 한다. 



## 출처

[Eclipse RCP (Rich Client Platform) - Tutorial (vogella.com)](https://www.vogella.com/tutorials/EclipseRCP/article.html)