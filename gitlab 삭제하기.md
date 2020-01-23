아래 명령어 실행

```shell
sudo gitlab-ctl uninstall
sudo gitlab-ctl cleanse
sudo gitlab-ctl remove-accounts
sudo dpkg -P gitlab-ce || sudo yum -y remove gitlab-ce
```



아래 폴더  remove

```shell
/opt/gitlab
/var/opt/gitlab
/etc/gitlab
/var/log/gitlab
/etc/yum.repos.d/gitlab관련파일
```



