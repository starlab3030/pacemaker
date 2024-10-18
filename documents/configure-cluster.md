# 고가용성 클러스터 구성 및 제거

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

## 2. 고가용성 클러스터 생성

2대의 DB 노드로 구성된 클러스터를 생성합니다.

### 2.1 클러스터의 모든 노드에서 *hacluster* 사용자를 인증

```bash
pcs host auth aap-db1.thinkmore.net aap-db2.thinkmore.net
```

실행 결과
```
[root@aap-db1 ~]# pcs host auth aap-db1.thinkmore.net aap-db2.thinkmore.net
Username: hacluster
Password:
aap-db1.thinkmore.net: Authorized
aap-db2.thinkmore.net: Authorized

[root@aap-db2 ~]#
```
<br>

### 2.2 클러스터 생성

#### 2.2.1 기본 클러스터 생성

```bash
pcs cluster setup db_cluster --start aap-db1.thinkmore.net aap-db2.thinkmore.net
```

실행 결과
```
[root@aap-db1 ~]# pcs cluster setup db_cluster --start aap-db1.thinkmore.net aap-db2.thinkmore.net
No addresses specified for host 'aap-db1.thinkmore.net', using 'aap-db1.thinkmore.net'
No addresses specified for host 'aap-db2.thinkmore.net', using 'aap-db2.thinkmore.net'
Destroying cluster on hosts: 'aap-db1.thinkmore.net', 'aap-db2.thinkmore.net'...
aap-db1.thinkmore.net: Successfully destroyed cluster
aap-db2.thinkmore.net: Successfully destroyed cluster
Requesting remove 'pcsd settings' from 'aap-db1.thinkmore.net', 'aap-db2.thinkmore.net'
aap-db2.thinkmore.net: successful removal of the file 'pcsd settings'
aap-db1.thinkmore.net: successful removal of the file 'pcsd settings'
Sending 'corosync authkey', 'pacemaker authkey' to 'aap-db1.thinkmore.net', 'aap-db2.thinkmore.net'
aap-db2.thinkmore.net: successful distribution of the file 'corosync authkey'
aap-db2.thinkmore.net: successful distribution of the file 'pacemaker authkey'
aap-db1.thinkmore.net: successful distribution of the file 'corosync authkey'
aap-db1.thinkmore.net: successful distribution of the file 'pacemaker authkey'
Sending 'corosync.conf' to 'aap-db1.thinkmore.net', 'aap-db2.thinkmore.net'
aap-db2.thinkmore.net: successful distribution of the file 'corosync.conf'
aap-db1.thinkmore.net: successful distribution of the file 'corosync.conf'
Cluster has been successfully set up.
Starting cluster on hosts: 'aap-db1.thinkmore.net', 'aap-db2.thinkmore.net'...

[root@aap-db1 ~]# pcs status
Cluster name: db_cluster

WARNINGS:
No stonith devices and stonith-enabled is not false

Cluster Summary:
  * Stack: corosync (Pacemaker is running)
  * Current DC: aap-db1.thinkmore.net (version 2.1.7-5.2.el9_4-0f7f88312) - partition with quorum
  * Last updated: Wed Oct 16 23:01:55 2024 on aap-db2.thinkmore.net
  * Last change:  Wed Oct 16 23:01:14 2024 by hacluster via hacluster on aap-db1.thinkmore.net
  * 2 nodes configured
  * 0 resource instances configured

Node List:
  * Online: [ aap-db1.thinkmore.net aap-db2.thinkmore.net ]

Full List of Resources:
  * No resources

Daemon Status:
  corosync: active/disabled
  pacemaker: active/disabled
  pcsd: active/enabled
[root@aap-db2 ~]# pcs cluster status
Cluster Status:
 Cluster Summary:
   * Stack: corosync (Pacemaker is running)
   * Current DC: aap-db1.thinkmore.net (version 2.1.7-5.2.el9_4-0f7f88312) - partition with quorum
   * Last updated: Wed Oct 16 23:02:18 2024 on aap-db2.thinkmore.net
   * Last change:  Wed Oct 16 23:01:14 2024 by hacluster via hacluster on aap-db1.thinkmore.net
   * 2 nodes configured
   * 0 resource instances configured
 Node List:
   * Online: [ aap-db1.thinkmore.net aap-db2.thinkmore.net ]

PCSD Status:
  aap-db2.thinkmore.net: Online
  aap-db1.thinkmore.net: Online

[root@aap-db1 ~]#
```

#### 2.2.2 다중 링크로 클러스터 구성

```bash
pcs cluster setup db_cluster aap-db1.thinkmore.net addr=192.168.253.48 addr=192.168.0.48 aap-db2.thinkmore.net addr=192.168.253.49 addr=192.168.0.49
pcs cluster start
pcs cluster status
```

실행 결과
```
[root@aap-db1 ~]# pcs cluster setup db_cluster aap-db1.thinkmore.net addr=192.168.253.48 addr=192.168.0.48 aap-db2.thinkmore.net addr=192.168.253.49 addr=192.168.0.49
Destroying cluster on hosts: 'aap-db1.thinkmore.net', 'aap-db2.thinkmore.net'...
aap-db2.thinkmore.net: Successfully destroyed cluster
aap-db1.thinkmore.net: Successfully destroyed cluster
Requesting remove 'pcsd settings' from 'aap-db1.thinkmore.net', 'aap-db2.thinkmore.net'
aap-db1.thinkmore.net: successful removal of the file 'pcsd settings'
aap-db2.thinkmore.net: successful removal of the file 'pcsd settings'
Sending 'corosync authkey', 'pacemaker authkey' to 'aap-db1.thinkmore.net', 'aap-db2.thinkmore.net'
aap-db1.thinkmore.net: successful distribution of the file 'corosync authkey'
aap-db1.thinkmore.net: successful distribution of the file 'pacemaker authkey'
aap-db2.thinkmore.net: successful distribution of the file 'corosync authkey'
aap-db2.thinkmore.net: successful distribution of the file 'pacemaker authkey'
Sending 'corosync.conf' to 'aap-db1.thinkmore.net', 'aap-db2.thinkmore.net'
aap-db1.thinkmore.net: successful distribution of the file 'corosync.conf'
aap-db2.thinkmore.net: successful distribution of the file 'corosync.conf'
Cluster has been successfully set up.

[root@aap-db1 ~]# pcs cluster start
Starting Cluster...

[root@aap-db1 ~]# pcs cluster status
Cluster Status:
 Cluster Summary:
   * Stack: unknown (Pacemaker is running)
   * Current DC: NONE
   * Last updated: Wed Oct 16 23:24:32 2024 on aap-db1.thinkmore.net
   * Last change:  Wed Oct 16 23:24:30 2024 by hacluster via hacluster on aap-db1.thinkmore.net
   * 2 nodes configured
   * 0 resource instances configured
 Node List:
   * Node aap-db1.thinkmore.net: UNCLEAN (offline)
   * Node aap-db2.thinkmore.net: UNCLEAN (offline)

PCSD Status:
  aap-db1.thinkmore.net: Online
  aap-db2.thinkmore.net: Online

[root@aap-db1 ~]#
```
<br>
<br>

## 3. 클러스터 특성 설정

### 3.1 펜싱 설정 (stonith)

```bash
pcs property config stonith-enabled
pcs property set stonith-enabled=false
pcs property config stonith-enabled
```

실행 결과
```
[root@aap-db1 ~]# pcs property config stonith-enabled
Cluster Properties: cib-bootstrap-options
  stonith-enabled=true (default)

[root@aap-db1 ~]# pcs property set stonith-enabled=false

[root@aap-db1 ~]# pcs property config stonith-enabled
Cluster Properties: cib-bootstrap-options
  stonith-enabled=false

[root@aap-db1 ~]#
```
* 고가용성 클러스터에는 펜싱이 필요
* 운영 환경이 아닌 테스트 동의 경우에는 펜싱 비활성화
<br>
<br>

## 4. 고가용성 클러스터 제거

### 4.1 클러스터 중지

```bash
pcs cluster stop
```

실행 결과
```
[root@aap-db2 ~]# pcs cluster stop
Stopping Cluster (pacemaker)...
Stopping Cluster (corosync)...

[root@aap-db2 ~]#
```
<br>

### 4.2 클러스터 삭제

모든 클러스터 멤버 노드에서 실행
```bash
pcs cluster destroy
```

실행 결과
```
[root@aap-db1 ~]# pcs cluster destroy
Warning: It is recommended to run 'pcs cluster stop' before destroying the cluster.
WARNING: This would kill all cluster processes and then PERMANENTLY remove cluster state and configuration
Type 'yes' or 'y' to proceed, anything else to cancel: y
Shutting down pacemaker/corosync services...
Killing any remaining services...
Removing all cluster configuration files...

[root@aap-db1 ~]#
```
* 클러스터 멤버인 aap-db1.thinkmore.net 및 aap-db2.thinkmore.net에서 실행
<br>
<br>

------
[차례](../README.md)