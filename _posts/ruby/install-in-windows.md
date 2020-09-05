## 윈도우 루비 설치

이 글을 작성하는 시점의 루비의 최신버전은 2.7.1이다. 윈도우에 루비를 설치하기 위해 홈페이지에서 제공하는 인스톨러를 받는다. [링크](https://rubyinstaller.org/) 홈페이지의 다운로드 사이트에서는 호환성이 더 좋은 2.6.6버전을 설치하라고 한다.

윈도우64의 경우 `rubyinstaller-devkit-2.6.6-1-x64.exe`파일을 받게된다. 이 파일을 실행한다. 설치가 완료되면 다음과 같은 화면이 나타난다. 

![image-20200905234506078](install-in-windows.assets\image-20200905234506078.png)

정확히 뭐가 설치되는 건지 모르지만 일단 모든 것을 설치하는 3번을 선택하자.

설치가 완료된 후 커맨트 창을 열어 다음 명령어로 설치를 확인하자. 

```bat
C:\Users\jh>ruby -version
ruby 2.6.6p146 (2020-03-31 revision 67876) [x64-mingw32]
Traceback (most recent call last):
-e:1:in `<main>': undefined local variable or method `rsion' for main:Object (NameError)
```





## 참조

[공식 홈페이지](https://www.ruby-lang.org/en/downloads/)