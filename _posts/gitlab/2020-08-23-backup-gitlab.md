---
title: "gitlab 백업하기"
date: 2020-08-23 16:00:00 +0900
categories: gitlab
---

## gitlab backup

```bash
sudo gitlab-rake gitlab:backup:create
```

백업을 하면 `/var/opt/gitlab/backups/`에 백업파일이 생성된다. 

아래 메세지를 확인하여 몇몇 파일은 수동으로 백업해야 한다.

```bash
Warning: Your gitlab.rb and gitlab-secrets.json files contain sensitive data
and are not included in this backup. You will need these files to restore a backup.
Please back them up manually.
```

> 수동 백업해야 할 설정 파일은 `/etc/gitlab/`에 있다. 

