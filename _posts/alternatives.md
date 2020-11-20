# alternatives

centos에 설치된 패키지들의 버전 관리를 위한 프로그램이다. alternatives는 OS에 기본으로 설정되어 있는 명령을 새로 설치한 경로로 변경할 때 사용한다. 미리여러 개를 등록 해두고 변경 할 수 있다.

## --install

```
alternatives --install /usr/bin/java java /usr/java/jdk1.8.0_25/bin/java 100
```