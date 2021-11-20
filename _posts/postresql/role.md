## Role 이란

자동으로 설정되는 것이 또 하나 있는데 role이다. role은 postgresql에서 데이터베이스 access 권한을 관리하기 위해 사용하는 컨셉이다. postgresql은 최초 설치시에 가지는 기본 role이 있는데 이 role이 항상 superuser의 역할을 한다. 이 role은 데이터베이스를 초기화하는 운영체제 user의 이름과 동일한 이름으로 설정된다. 즉, postgres계정으로 데이터베이스를 초기화 했으니 postgres role이 superuser role로 생성되어있는 것이다. 관습적으로 `postgres`가 super role의 이름이 된다. 그리고 많은 애플리케이션에서 별도로 role을 명시하지 않으면 운영체제의 유저명을 사용하도록 되어 있기 때문에, 운영체제의 유저명과 role명을 일치시키는게 좋다.

 운영체제 계정으로도 postgres가 있고 postgresql 서버 안에도 postgres role이 설치되어 있는데 이 둘은 다른 것이니 혼동하지 말자. 그런데 더욱 혼란스러운 것은 운영체제의 postgres 계정으로 postgre 명령어 (psql, createdb 등)을 사용할 수 있다. 이것은 postgres role이 있기 때문에 실행이 가능한 것 같다. 만약 다른 유저로 postgresql 명령어를 사용한다면 해당 유저와 관련한 role이 없다며 명령어가 실행되지 않는다. 그렇다면 운영체제의 postgre계정과 postgre role이 연결된 것인가? 서버의 role과 운영체제의 계정은 어떤 관계일까? 이 부분은 좀 더 연구가 필요한 것 같다.

> 알아야 할 것: 리눅스의 postgres 계정과 DB서버의 postgres role과의 관계

## Role Attributes

Role은 권한과 클라이언트 인증 시스템을 정의하는 많은 애트리뷰트를 가지고 있다.

### login privilege

이 애트리뷰트가 있는 role만 데이터베이스에 로그인이 가능하다.

```sql
create role name LOGIN;
create user name;
```

위와 같이 2가지 방법으로 role을 생성할 수 있는데 user 명령어는 기본으로 login애트리뷰트를 생성한다.

Role 애트리뷰트는 `ALTER ROLE` 명령어를 통해 creation 이후에 변경가능하다.

```sql
ALTER ROLE name LOGIN;
```

```sql
ALTER ROLE name [ [ WITH ] option [ ... ] ]

where option can be:
    
      SUPERUSER | NOSUPERUSER
    | CREATEDB | NOCREATEDB
    | CREATEROLE | NOCREATEROLE
    | CREATEUSER | NOCREATEUSER
    | INHERIT | NOINHERIT
    | LOGIN | NOLOGIN
    | CONNECTION LIMIT connlimit
    | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'
    | VALID UNTIL 'timestamp' 

ALTER ROLE name RENAME TO newname

ALTER ROLE name SET configuration_parameter { TO | = } { value | DEFAULT }
ALTER ROLE name SET configuration_parameter FROM CURRENT
ALTER ROLE name RESET configuration_parameter
ALTER ROLE name RESET ALL
```

