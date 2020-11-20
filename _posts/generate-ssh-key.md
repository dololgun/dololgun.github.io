# SSH 비 대칭키 생성하기 

## 윈도우

윈도우는 openssl을 설치해야 한다.  [링크](https://zetawiki.com/wiki/%EC%9C%88%EB%8F%84%EC%9A%B0_openssl_%EC%84%A4%EC%B9%98)

### 개인키 생성
openssl이 설치된 폴더의 bin폴더로 들어간다. 
아래 명령어를 입력한다.

```
openssl genrsa -out private.key 1024
```

### 공개키 생성
개인키를 가지고 공개키를 만든다. 
```
openssl rsa -in private.key -out public.key -pubout -outform derwriting
```

## mac
아래 명령어로 rsa키가 있는지 확인
```
$ cat ~/.ssh/id_rsa.pub
```

없다면 ssh 키 쌍을 만들자 
```
ssh-keygen
```

# 공개키 저장

## 리눅스

아래 파일을 생성하고 공개키를 저장한다. 
최초 생성시 파일의 권한이 755로 되어 있을 것이다. 
파일의 권한이 rw-------가 아니면 refuse 되니 파일의 권한을 변경하자.

```bash
echo "공개키" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```