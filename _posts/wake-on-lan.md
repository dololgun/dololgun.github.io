## WAKE ON LAN 적용하기

### 1. BIOS 설정

바이오스 WAKE ON LAN 설정을 활성화 한다. 이 옵션을 지칭하는 이름이 제조사의 바이오스 마다 다른다. 아래 목록은 이 설정에 대한 대표적인 이름이다.

* Wake on PME
* Wake on PCI Deivces
* Wake on Lan from S4/S5

내 메인보드는 Asrock 제품인데 `PCI Devices Power On` 이란 이름으로 되어 있었다.

### 2. 네트워크 어뎁터 설정

#### 전원 설정

컴퓨터 종료 시에 이 네트워크 어뎁터가 종료되지 않도록 설정

#### 고급

* Wake on Magic Packet 활성화

* Wake on Pattern Match 활성화

* 웨이크 온 랜 종료 활성화

### 3. 윈도우 전원 설정

`전원단추작동설정`에서 `빠른시작켜기(권장)` 기능이 활성화 되어 있다면 WOL이 동작하지 않기 때문에 `빠른시작켜기(권장)` 기능을 비활성화 한다.

## 출처

https://seujeum.tistory.com/13