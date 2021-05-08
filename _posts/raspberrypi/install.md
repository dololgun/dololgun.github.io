## 설치

### 이미지로 설치하기

https://www.raspberrypi.org/software/ 사이트에서 라즈베리파이 이미저를 받는다. 

마이크로SD카드를 준비하여 이미저로 OS를 설치한다.

### 최초 구동 시에 원격 접속 가능하게 하기

모니터가 없는 상황에서 유용하다.

```
boot
```

1. ssh 이름의 빈 파일 생성

2. wpa_supplicant.conf

```
country=US 
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev 
update_config=1 
network={ 
 ssid="WIFI 이름" 
 psk="WIFI 비밀번호" 
 scan_ssid=1 
}
```

### 접속 계정

```
id : pi
pw : raspberry
```

### ssh 명령어

```bash
// 재시작
sudo reboot
// 끄기
sudo halt
```

### xrdp 설치

원격데스크탑으로 사용할 수 있다.

```bash
sudo apt install xrdp
```

설치 후 재부팅한다. 이제부터 윈도우 원격 데스크탑으로 접속할 수 있다.

### openjdk 설치

```
java -version
```

```bash
sudo apt-get install openjdk-8-jdk
```

### cpu 정보 체크

```bash
cat /proc/cpuinfo
```

### pi4j 설치

인터넷이 연결된 상태에서

```bash
curl -sSL https://pi4j.com/install | sudo bash
```

1.4가 설치되는 듯 하다.

```
The Pi4J JAR files are located at:
/opt/pi4j/lib

Example Java programs are located at:
/opt/pi4j/examples

You can compile the examples using this script:
sudo /opt/pi4j/examples/build

Please see https://pi4j.com for more information.

When attempting to compile a Java program using the Pi4J libraries, make sure to include the Pi4J lib folder in the classpath:
javac -classpath .:classes:/opt/pi4j/lib/'*' ...
```

### wiringpi natvie library

pi4j가 설치되어 있다면

```bash
sudo pi4j --wiringpi
```

