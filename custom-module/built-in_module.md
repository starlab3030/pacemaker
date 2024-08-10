# Built-In Modules based on RHEL9.4

### Open Cluster Framework (OCF)


<br>

### 운영체제 정보

```bash
$ uname -a
Linux bastion01 5.14.0-427.13.1.el9_4.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Apr 10 10:29:16 EDT 2024 x86_64 x86_64 x86_64 GNU/Linux

$ cat /etc/redhat-release 
Red Hat Enterprise Linux release 9.4 (Plow)
$
```
<br>

### 디렉터리 아키텍처

```bash
$ pwd
/usr/lib

$ tree -F ./ocf -L 2
./ocf
├── lib/
│   └── heartbeat/
└── resource.d/
    ├── heartbeat/
    ├── openstack/
    └── pacemaker/

6 directories, 0 files
$
```
<br>

### PaceMaker 라이브러리

```bash
$ pwd
/usr/lib/ocf/lib/heartbeat

$ ls -l ocf*
-rw-r--r--. 1 root root  1718 May  1 17:19 ocf-binaries
-rw-r--r--. 1 root root   675 May  1 17:19 ocf-directories
-rw-r--r--. 1 root root  4594 May  1 17:19 ocf-distro
-rw-r--r--. 1 root root 13169 May  1 17:19 ocf.py
-rw-r--r--. 1 root root  3623 May  1 17:19 ocf-rarun
-rw-r--r--. 1 root root  1869 May  1 17:19 ocf-returncodes
-rw-r--r--. 1 root root 26403 May  1 17:19 ocf-shellfuncs

$ 
```

#### OCF 리눅스 배포판

```bash
$ egrep "^_|^[A-Za-z]" ocf-distro 
_ETC_OS_RELEASE_FILE="/etc/os-release"
_USR_OS_RELEASE_FILE="/usr/lib/os-release"
_DEBIAN_VERSION_FILE="/etc/debian_version"
_REDHAT_RELEASE_FILE="/etc/redhat-release"
_SUSE_RELEASE_FILE="/etc/SuSE-release"
_process_os_release_id() {
_process_os_version_id() {
_get_val_from_os_release_file() {
_get_os_from_legacy_source() {
_get_version_from_legacy_source() {
get_release_id() {
get_os_version_id() {
is_debian_based() {
is_redhat_based() {
is_suse_based() {
get_os_ver() {
$
```

#### OCF 반환값

```bash
$ egrep -v "^$|^#" ocf-returncodes 
OCF_SUCCESS=0
OCF_ERR_GENERIC=1
OCF_ERR_ARGS=2
OCF_ERR_UNIMPLEMENTED=3
OCF_ERR_PERM=4
OCF_ERR_INSTALLED=5
OCF_ERR_CONFIGURED=6
OCF_NOT_RUNNING=7
OCF_RUNNING_MASTER=8
OCF_FAILED_MASTER=9
$
```
