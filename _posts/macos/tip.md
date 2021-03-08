## Mac OS 자잘한 팁

Mac을 사용하면서 알아두면 편리한 자잘한 팁을 지속적으로 추가한다.

### 윈도우 캡쳐

`COMMAND + SHIFT + 4` 이후에 `space` 를 눌르면 윈도우를 선택할 수 있다.

### 특수문자 사용

아래 단축키를 사용하면 특수문자를 사용할 수 있는 윈도우가 나타난다.  

```
command + control + space
```

### Command Line Tools 설치

터미널을 이용해 명령어 라인 도구를 설치한다. Xcode를 설치하면 자동으로 설치되지만 Xcode는 용량을 많이 차지하고 무겁기 때문에 굳이 사용할 필요가 없다면 이 방법을 이용하여 커맨드라인툴만 설치할 수 있다. 터미널 화면에서 아래 명령어를 수행한다.

```bash
xcode-select --install
```

### 터미널에서 `ll`명령어 사용하기

리눅스 bash에서 기본으로 사용할 수 있는 `ll`명령어를 Mac bash에서는 사용할 수 없다. 환경설정 파일에 ll을 alias로 등록하여 명령어처럼 사용하도록 하자. 

아래와 같이 사용자 홈디렉터리(사용자별 관리)에 `.bash_profile` 파일을 생성하고 아래 두번째 단락의 alias를 입력한다. 

```bash
vi ~/.bash_profile
alias ll='ls -l'
```

그리고 `source ~/.bas`h_profile 명령어를 입력하여 설정한 내용이 즉시 적용되도록 한다.

> **[주의]** 자신이 사용하는 기본 쉘이 무엇인지 알고 그에 맞는 환경설정파일을 수정해야한다. 위의 예제에서 사용된 .bash_profile파일은 bash쉘이 사용하는 로그인 환경설정이다.

### 기본 쉘 변경하기

맥은 기본으로 bash쉘이 지정되어 있다. 최근에는 zsh가 많은 인기를 얻고 있다. 맥의 기본 쉘을 zsh로 변경하기 위해 다음과 같이 진행한다. 

zsh의 위치를 찾는다. 

```
which zsh
```

다음 명령어를 이용하여 기본 쉘을 경로를 변경한다. 

```
chsh -s /usr/local/bin/zsh
```

위의 예제에서 zsh의 경로가 /usr/local/bin/zsh인데 이 경로가 기본 쉘로 적용 가능한지 확인이 필요하다. 만약 기본 쉘로 적용가능하지 않다면 chsh 명령어 실행 시 오류가 발생한다. 아래 파일을 확인하여 zsh의 경로가 없다면 등록하도록 하자.

```
vi /etc/shells
```

아래와 같이 경로가 없다면 추가하자.

```
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
/usr/local/bin/zsh
```

### 재부팅 하기

```
% ssh -l <admin> <computer ip>
% sudo shutdown -r hhmm
```

