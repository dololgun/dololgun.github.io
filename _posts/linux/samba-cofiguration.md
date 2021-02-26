## samba configuration

### 설치

centos 기준, 다음 명령어로 설치한다.

```bash
$ sudo yum install samba samba-client
```

samba가 사용하는 프로세스 이름은 `smbd`이고, 사용하는 포트는 TCP 139, 445 이다. 

NetBIOS가 사용하는 프로세스 이름은 `nmbd`이고, 사용하는 포트는 UDP 137이다. 

### 방화벽 설정

굳이 포트명을 사용하지 않고 서비스 이름으로 방화벽을 해제할 수 있다.

```bash
$ sudo firewall-cmd --permanent --add-service=samba
$ sudo systemctl restart firewalld
```

### 그룹관리

samba를 위한 그룹을 생성하는 것이 좋다.

```bash
$ sudo groupadd samba
```

samba를 사용하는 모든 유저에 samba 그룹을 할당한다.

### 유저 관리

유저는 리눅스의 유저를 사용하지만 인증은 리눅스와 별개로 samba의 인증방식을 따른다. 

즉, 유저 생성은 리눅스의 유저를 생성하는 절차대로 하면 된다. 단, samba만을 사용하는 shell 에 접근할 필요가 없다.  

```bash
$ sudo useradd -M -d /samba/josh -s /usr/sbin/nologin -G sambashare josh
```

- `-M` -do not create the user’s home directory. We’ll manually create this directory.
- `-d /samba/josh` - set the user’s home directory to `/samba/josh`.
- `-s /usr/sbin/nologin` - disable shell access for this user.
- `-G sambashare` - add the user to the `sambashare` group.

생성한 유저의 smb 패스워드를 설정한다. 

```bash
$ sudo smbpasswd -a geonho
```

유저를 smb 서비스에 등록한다.

```bash
$ sudo smbpasswd -e geonho
```



### 폴더 관리

* `mkdir`명령어를 사용하여 공유하고자 하는 디렉터리를 생성한다. 
* 소유자를 samba

```bash
$ sudo mkdir /samba/josh
$ sudo chown josh:samba /samba/josh
```

디렉터리 퍼미션을 준다. 

```bash
$ sudo chmod 2770 /samba/josh
```

### smb.conf 설정

* samba 설정을 위해 `/etc/samba/smb.conf` 파일을 수정한다. 
* global 이 공통 설정 영역이다. 

```
[global]
        workgroup = SAMBA
        security = user

        passdb backend = tdbsam

        printing = cupsㅊㅇ .
        printcap name = cups
        load printers = yes
        cups options = raw

[homes]
        comment = Home Directories
        valid users = %S, %D%w%S
        browseable = No
        read only = No
        inherit acls = Yes

[printers]
        comment = All Printers
        path = /var/tmp
        printable = Yes
        create mask = 0600
        browseable = No

[print$]
        comment = Printer Drivers
        path = /var/lib/samba/drivers
        write list = @printadmin root
        force group = @printadmin
        create mask = 0664
        directory mask = 0775

```

### selinux 보안 설정

selinux를 사용하는 운영체제는 다음 설정을 해야 추가적으로 해야 한다. 

* selinux 보안 설정을 한다. `setsebool -P samba_enable_home_dirs on`
* selinux 보안 설정을 한다. `chcon -t samba_share_t /home/sds/share`