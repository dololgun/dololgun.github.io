도커엔진이 설치되면 docker0이라는 네트워크가 생성된다. 

호스트 머신에서 다음 명령어를 입력해보자. 

```bash
ip addr show docker0
```

다음과 같이 172.17.0.1/16을 사용하는 네트워크가 구성되어 있다.

```
5: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:25:b7:aa:fd brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:25ff:feb7:aafd/64 scope link
       valid_lft forever preferred_lft forever
```



도커는 기본적으로 브리지 모드로 실행된다. 

docker run --rm -it ubuntu:trusty bash 을 실행하자.

