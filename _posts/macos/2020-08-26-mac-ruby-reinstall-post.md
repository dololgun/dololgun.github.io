---
title: MacOS ruby 버전 관리
date: '2020-08-26 21:00:00 +0900'
categories: macos
published: true
---

## mac에서 ruby버전 관리하기

mac에는 이미 ruby가 설치되어 있지만 오래된 버전이 설치되어 있고 update도 되지 않는다. ruby의 최신기능을 사용하고 싶다면 최신버전으로 업그레이드하는 것이 좋다.

ruby를 업그레이드를 하는 방법은 여러가지가 있는데 mac에서는 homebrew홈브류)를 이용하여 편하게 설치할 수 있다.

ruby 공식 사이트에서는 rbenv를 사용하여 ruby를 설치할 것을 권고한다.(이뉴는 잘 모르겠다)

다음 명령어로 rbenv를 설치하자. 

```bash
$ brew install rbenv
```

rbenv에 의존성이 있는 autoconf, pkg-config, ruby-build도 함께 설치된다. ruby-build가 설치가 완료되면 다음과 같은 애매한 메세지가 나타난다. ruby-build가 사용하는 OpenSSL이 홈브류가 사용하는 OpenSSL과 연동되지 않아 업그레이드가 진행되지 않으니 로그인 프로파일(.zshrc파일)을 수정하여 홈브류와 연결하라는 것이다.

```bash
ruby-build installs a non-Homebrew OpenSSL for each Ruby version installed and these are never upgraded.

To link Rubies to Homebrew's OpenSSL 1.1 (which is upgraded) add the following
to your ~/.zshrc:
  export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)"
```

### .bash_profile 수정

rbenv가 설치되면 자신의 로그인 프로파일을 수정해야 한다. 리눅스나 유닉스에 친숙하지 않은 사용자라면 어떤 파일을 수정해야 하는지 모를수가 있는데, 수정해야 할 파일은 자신이 사용하는 쉘에 따라 달라진다. 왜냐하면 쉘마다 시작할 때 읽어들이는 스크립트 파일이 다르기 때문이다. 내 mac은 zsh 쉘을 사용하므로 .zshrc파일을 수정한다. 해당 파일 열어 아래 명령어를 추가한다. 

```
eval "$(rbenv init -)"
```

### 사용가능한 버전 찾기 

이제 rbenv를 사용할 수 있다. 다음 명령어를 실행하면 설치가능한 ruby버전이 보인다. 

```
rbenv install -l
```

### 원하는 버전 설치하기 

다음 명령어로 ruby를 설치하자.(설치하는 시간이 꽤 걸린다)

```
rbenv install 2.7.1
```

설치완료 후 아래 명령어를 실행한다.(이유는 정확히 모르겠다) 

```
rbenv rehash
```

### ruby 버전 변경

아래 명령어로 사용하고자하는 ruby버전을 지정할 수 있다. 

```
rbenv global 2.7.1
```

### 확인

다음 명령어로 ruby 버전이 변경된 것을 확인하자.

```
ruby --version
```


## 참고

https://jetalog.net/85

https://devlog.github.io/2015/11/30/mac-rbenv-ruby.html

[https://withrails.com/2015/11/25/rbenv-%EC%9E%91%EB%8F%99%EC%9B%90%EB%A6%AC/](https://withrails.com/2015/11/25/rbenv-작동원리/)
