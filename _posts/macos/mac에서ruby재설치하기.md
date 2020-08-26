## mac에서 ruby버전 관리하기

mac에는 이미 ruby가 설치되어 있지만 예전 버전이 설치되어 있고 update도 되지 않는다. 그래서 최신버전의 ruby를 homebrew를 통해서 설치하도록 한다. 

먼저, rbenv를 설치한다. ruby에서는 rbenv를 사용하고 ruby를 설치할 것을 권고한다. (이뉴는 잘 모르겠다)

다음 명령어로 rbenv를 설치하자. 

```bash
brew install rbenv
```

이렇게 하면 의존성이 있는 autoconf, pkg-config, ruby-build도 함께 설치된다. ruby-build가 설치가 완료되면 다음과 같이 의미심장한 메세지가 나타난다. 

```bash
ruby-build installs a non-Homebrew OpenSSL for each Ruby version installed and these are never upgraded.

To link Rubies to Homebrew's OpenSSL 1.1 (which is upgraded) add the following
to your ~/.zshrc:
  export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)"
```

### .bash_profile 수정

인터넷에 이렇게 하라고 나와있는데 정확한 것은 자신이 사용하는 쉘에 따라 달라진다. 나는 zsh를 사용하기 때문에 .zshrc파일을 수정한다. 파일에 아래 명령어를 추가한다. 

```
eval "$(rbenv init -)"
```

### 사용가능한 버전 찾기 

다음 명령어를 실행하면 설치가능한 ruby버전이 보인다. 

```
rbenv install -l
```

### 원하는 버전 설치하기 

아래 명령어로 설치하자. 설치하는 시간이 꽤 걸린다.

```
rbenv install 2.7.1
```

설치완료 후 아래 명령어를 실행한다. 

```
rbenv rehash
```

### ruby 버전 변경

아래 명령어로 원하는 버전을 지정할 수 있다. 

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