# 고가용성 클러스터 구성

## 1. 클러스터 데몬 실행

```bash
systemctl start pcsd.service
systemctl enable pcsd.service
systemctl status pcsd.service
```

실행 결과
```
[root@aap-db1 ~]# systemctl start pcsd.service

[root@aap-db1 ~]# systemctl enable pcsd.service
Created symlink /etc/systemd/system/multi-user.target.wants/pcsd.service → /usr/lib/systemd/system/pcsd.service.

[root@aap-db1 ~]# systemctl status pcsd.service
● pcsd.service - PCS GUI and remote configuration interface
     Loaded: loaded (/usr/lib/systemd/system/pcsd.service; enabled; preset: disabled)
     Active: active (running) since Wed 2024-10-16 21:47:37 KST; 11s ago
       Docs: man:pcsd(8)
             man:pcs(8)
   Main PID: 11012 (pcsd)
      Tasks: 31 (limit: 48800)
     Memory: 346.0M
        CPU: 5.072s
     CGroup: /system.slice/pcsd.service
             ├─11012 /usr/bin/python3 -Es /usr/sbin/pcsd
             ├─11019 /usr/bin/python3 -E -s -c "from multiprocessing.resource_tracker import main;main(6)"
             ├─11020 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11021 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11031 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11032 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11033 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11034 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11035 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11036 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11037 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11038 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             ├─11039 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>
             └─11040 /usr/bin/python3 -E -s -c "from multiprocessing.forkserver import main; main(6, 8, ['__main__'], **{'sys_path': ['>

Oct 16 21:47:35 aap-db1.thinkmore.net systemd[1]: Starting PCS GUI and remote configuration interface...
Oct 16 21:47:37 aap-db1.thinkmore.net systemd[1]: Started PCS GUI and remote configuration interface.

[root@aap-db1 ~]#
```
<br>
<br>

## 2. 

## 3. 



```bash

```

실행 결과
```

```