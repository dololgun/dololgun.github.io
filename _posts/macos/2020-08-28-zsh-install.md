---
title: "[MacOS] zsh 설치 및 설정"
date: 2020-08-28 23:00:00 +0900
categories: macos
---

## zsh 설치하기

zsh설치하기 전에 홈브류가 설치되어 있어야 함.

### zsh 설치

홈브류를 이용하여 zsh, zsh-completions를 설치한다.

```bash
brew install zsh zsh-completions
```

### oh-my-zsh 설치

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### 기본 쉘 확인하기

다음 명령어로 운영체제의 기본쉘을 확인할 수 있다. 아래 예제는 기본 쉘로 bash가 설정되어 있는 것을 확인할 수 있다.

```bash
$ echo $SHELL
/bin/bash
```

기본 쉘은 다음 명령어를 통해 변경할 수 있다.

```bash
chsh -s /usr/local/bin/zsh
```

그런데, 기본 쉘을 변경 할 때 오류가 날 수 있는데 그 원인은 사용 가능한 쉘이 별도로 지정되어 있기 때문이다. 다음 경로의 파일을 열어보면 사용 가능한 쉘 목록을 볼 수 있다.

```
cat /etc/shells
```

만약 설치된 zsh의 경로가 목록에 없다면 마지막에 추가하도록 하자.

## zsh 테마 변경

~/.zshrc 파일의 다음 항목을 수정한다. 

```
ZSH_THEME="agnoster"
```

### 프롬프트 변경하기

여기서 프롬프트란 터미널 창에서 커서를 기준으로 왼쪽에 표시되는 정보를 의미한다. 

![image-20200828232522592](image-20200828232522592.png)

이름이 너무 길경우 아래와 같은 로직을 .zshrc파일에 추가하면 사용자 이름만 나오도록 할 수 있다.

{% raw %}

```shell
prompt_context() {
  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment black default "%(!.%{%F{yellow}%}.)$USER"
  fi
}
```

{% endraw %}

### newline 적용하기

명령어가 길어지면 칸을 넘어가서 잘 보이지가 않는다. 이를 방지하기 위해 테마의 설정을 변경한다. 다음 명령어로 텍스트에디터를 통해 테마 파일을 수정할 수 있다. 

```
open -a TextEdit ~/.oh-my-zsh/themes/agnoster.zsh-theme
```

테마파일의 다음 항모을 수정한다. 
```
build_prompt() {
  RETVAL=$?
  prompt_status
  prompt_virtualenv
  prompt_context
  prompt_dir
  prompt_git
  prompt_bzr
  prompt_hg
  prompt_newline //이부분을 추가 꼭 순서 지켜서
  prompt_end
}
```

