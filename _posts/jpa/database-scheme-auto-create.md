## 데이터베이스 스키마 자동생성

persistence.xml 에서 스키마를 자동생성에 대한 설정을 할 수 있다. 

```
<property name="hibernate.hbm2ddl.auto" value="create" />
```

옵셜 설명

create : 기존 테이블 삭제하고 다시 생성
create-drop : create 옵션 + 어플리케이션이 종료할 때 drop한다.
update :DB와 비교하여 변경된 사항만 반영한다.
validate :DB와 비교하여 차이가 있으면 어플리케이션을 실행하지 않고 경고 메시지를 남긴다. 이 설정은 DDL을 수정하지 않는다. 
none : 자동 생성 기능을 사용하지 않는다. 