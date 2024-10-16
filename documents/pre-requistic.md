# Pacemaker 설치

## 1. 클러스터 소프트웨어 설치

### 1.1 채널 활성화

클러스터 멤버의 각 노드에서 필요한 채널 활성화
```bash
subscription-manager repos --enable=rhel-9-for-x86_64-highavailability-rpms
```

실행 결과
```
[root@aap-db1 ~]# subscription-manager repos --enable=rhel-9-for-x86_64-highavailability-rpms
Repository 'rhel-9-for-x86_64-highavailability-rpms' is enabled for this system.

[root@aap-db1 ~]#
```
<br>

### 1.2 패키지 설치

#### 1.2.1 고가용성 패키지 설치

```bash
dnf install pcs pacemaker
```

실행 결과
```
[root@aap-db1 ~]# dnf install pcs pacemaker
...<snip>...

[root@aap-db1 ~]#
```

#### 1.2.2 펜싱 에이전트 확인

```bash
dnf search fence-agents-*
```

실행 결과
```
[root@aap-db1 ~]# dnf search fence-agents-*
Updating Subscription Management repositories.
Last metadata expiration check: 0:05:42 ago on Wed Oct 16 20:55:09 2024.
===================================================== Name Matched: fence-agents-* =====================================================
fence-agents-aliyun.x86_64 : Fence agent for Alibaba Cloud (Aliyun)
fence-agents-all.x86_64 : Set of unified programs capable of host isolation ("fencing")
fence-agents-amt-ws.noarch : Fence agent for Intel AMT (WS-Man) devices
fence-agents-apc.noarch : Fence agent for APC devices
fence-agents-apc-snmp.noarch : Fence agents for APC devices (SNMP)
fence-agents-aws.x86_64 : Fence agent for Amazon AWS
fence-agents-azure-arm.x86_64 : Fence agent for Azure Resource Manager
fence-agents-bladecenter.noarch : Fence agent for IBM BladeCenter
fence-agents-brocade.noarch : Fence agent for Brocade switches
fence-agents-cisco-mds.noarch : Fence agent for Cisco MDS 9000 series
fence-agents-cisco-ucs.noarch : Fence agent for Cisco UCS series
fence-agents-common.noarch : Common base for Fence Agents
fence-agents-compute.x86_64 : Fence agent for Nova compute nodes
fence-agents-drac5.noarch : Fence agent for Dell DRAC 5
fence-agents-eaton-snmp.noarch : Fence agent for Eaton network power switches
fence-agents-emerson.noarch : Fence agent for Emerson devices (SNMP)
fence-agents-eps.noarch : Fence agent for ePowerSwitch 8M+ power switches
fence-agents-gce.x86_64 : Fence agent for GCE (Google Cloud Engine)
fence-agents-heuristics-ping.noarch : Pseudo fence agent to affect other agents based on ping-heuristics
fence-agents-hpblade.noarch : Fence agent for HP BladeSystem devices
fence-agents-ibm-powervs.noarch : Fence agent for IBM PowerVS
fence-agents-ibm-vpc.noarch : Fence agent for IBM Cloud VPC
fence-agents-ibmblade.noarch : Fence agent for IBM BladeCenter
fence-agents-ifmib.noarch : Fence agent for devices with IF-MIB interfaces
fence-agents-ilo-moonshot.noarch : Fence agent for HP iLO Moonshot devices
fence-agents-ilo-mp.noarch : Fence agent for HP iLO MP devices
fence-agents-ilo-ssh.noarch : Fence agents for HP iLO devices over SSH
fence-agents-ilo2.noarch : Fence agents for HP iLO2 devices
fence-agents-intelmodular.noarch : Fence agent for devices with Intel Modular interfaces
fence-agents-ipdu.noarch : Fence agent for IBM iPDU network power switches
fence-agents-ipmilan.noarch : Fence agents for devices with IPMI interface
fence-agents-kdump.x86_64 : Fence agent for use with kdump crash recovery service
fence-agents-kubevirt.x86_64 : Fence agent for KubeVirt platform
fence-agents-mpath.noarch : Fence agent for reservations over Device Mapper Multipath
fence-agents-openstack.x86_64 : Fence agent for OpenStack's Nova service
fence-agents-redfish.x86_64 : Fence agent for Redfish
fence-agents-rhevm.noarch : Fence agent for RHEV-M
fence-agents-rsa.noarch : Fence agent for IBM RSA II
fence-agents-rsb.noarch : Fence agent for Fujitsu RSB
fence-agents-sbd.noarch : Fence agent for SBD (storage-based death)
fence-agents-scsi.noarch : Fence agent for SCSI persistent reservations
fence-agents-virsh.noarch : Fence agent for virtual machines based on libvirt
fence-agents-vmware-rest.noarch : Fence agent for VMWare with REST API
fence-agents-vmware-soap.noarch : Fence agent for VMWare with SOAP API v4.1+
fence-agents-wti.noarch : Fence agent for WTI Network power switches

[root@aap-db1 ~]#
```
<br>

#### 1.2.3 펜싱 에이전트 설치

선택한 에이전트 설치 (예: scsi)
```bash
dnf install -y fence-agents-scsi
```

실행 결과
```
[root@aap-db1 ~]# dnf install -y fence-agents-scsi
...<snip>...

[root@aap-db1 ~]#
```

모든 에이전트 설치
```bash
dnf install -y fence-agents-all
```

실행 결과
```
[root@aap-db1 ~]# dnf install -y fence-agents-all
...<snip>...

[root@aap-db1 ~]#
```

#### 1.2.4 사용 가능한 에이전트 목록

```bash
rpm -qa | grep fence
```

실행 결과
```
[root@aap-db1 ~]# rpm -qa | grep fence
libxshmfence-1.3-10.el9.x86_64
fence-agents-common-4.10.0-62.el9_4.5.noarch
fence-agents-scsi-4.10.0-62.el9_4.5.noarch
fence-agents-apc-snmp-4.10.0-62.el9_4.5.noarch
fence-agents-cisco-mds-4.10.0-62.el9_4.5.noarch
fence-agents-eaton-snmp-4.10.0-62.el9_4.5.noarch
fence-agents-ibmblade-4.10.0-62.el9_4.5.noarch
fence-agents-ifmib-4.10.0-62.el9_4.5.noarch
fence-agents-intelmodular-4.10.0-62.el9_4.5.noarch
fence-agents-ipdu-4.10.0-62.el9_4.5.noarch
fence-agents-apc-4.10.0-62.el9_4.5.noarch
fence-agents-bladecenter-4.10.0-62.el9_4.5.noarch
fence-agents-brocade-4.10.0-62.el9_4.5.noarch
fence-agents-drac5-4.10.0-62.el9_4.5.noarch
fence-agents-hpblade-4.10.0-62.el9_4.5.noarch
fence-agents-ilo-moonshot-4.10.0-62.el9_4.5.noarch
fence-agents-ilo-mp-4.10.0-62.el9_4.5.noarch
fence-agents-rsa-4.10.0-62.el9_4.5.noarch
fence-agents-rsb-4.10.0-62.el9_4.5.noarch
fence-agents-wti-4.10.0-62.el9_4.5.noarch
fence-agents-vmware-soap-4.10.0-62.el9_4.5.noarch
fence-agents-vmware-rest-4.10.0-62.el9_4.5.noarch
fence-agents-rhevm-4.10.0-62.el9_4.5.noarch
fence-agents-redfish-4.10.0-62.el9_4.5.x86_64
fence-agents-mpath-4.10.0-62.el9_4.5.noarch
fence-agents-kdump-4.10.0-62.el9_4.5.x86_64
fence-agents-ilo-ssh-4.10.0-62.el9_4.5.noarch
fence-agents-heuristics-ping-4.10.0-62.el9_4.5.noarch
fence-agents-eps-4.10.0-62.el9_4.5.noarch
fence-agents-emerson-4.10.0-62.el9_4.5.noarch
fence-agents-cisco-ucs-4.10.0-62.el9_4.5.noarch
fence-agents-sbd-4.10.0-62.el9_4.5.noarch
fence-virt-4.10.0-62.el9_4.5.x86_64
fence-agents-ilo2-4.10.0-62.el9_4.5.noarch
fence-agents-ipmilan-4.10.0-62.el9_4.5.noarch
fence-agents-amt-ws-4.10.0-62.el9_4.5.noarch
fence-agents-all-4.10.0-62.el9_4.5.x86_64

[root@aap-db1 ~]#
```
<br>
<br>

## 2. 클러스터 구성 사전 준비

### 2.1 방화벽 구성

```bash
firewall-cmd --permanent --add-service=high-availability
firewall-cmd --reload
firewall-cmd --list-all
```

실행 결과
```
[root@aap-db1 ~]# firewall-cmd --permanent --add-service=high-availability
success

[root@aap-db1 ~]# firewall-cmd --reload
success

[root@aap-db1 ~]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens3 ens8
  sources:
  services: cockpit dhcpv6-client high-availability ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

[root@aap-db1 ~]#
```
<br>

### 2.2 고가용성 클러스터 사용자 설정

#### 2.2.1 사용자 ID 확인

사용자 이름은 *hacluster*
```bash
grep hacluster /etc/passwd
```

실행 결과
```
[root@aap-db1 ~]# grep hacluster /etc/passwd
hacluster:x:189:189:cluster user:/var/lib/pacemaker:/sbin/nologin

[root@aap-db1 ~]#
```

#### 2.2.2 사용자 암호 설정

고가용성 사용자 *hacluster*의 암호 설정
```bash
echo 'redhat' | passwd --stdin hacluster
```

실행 결과
```
[root@aap-db1 ~]# echo 'redhat' | passwd --stdin hacluster
Changing password for user hacluster.
passwd: all authentication tokens updated successfully.

[root@aap-db1 ~]#
```
<br>
<br>

## 3. 리소스 모니터링 설정

### 3.1 pcp-zeroconf

PCP는 RHEL 시스템을 위한 리소스 모니터링 도구로, pcp-zeroconf 패키지를 설치하면 PCP가 클러스터를 방해하는 펜싱, 리소스 오류 및 기타 이벤트 조사의 이점을 위해 performance-monitoring 데이터를 실행하고 수집할 수 있습니다.
* /var/log/pcp/가 포함된 파일 시스템에서 PCP의 캡처된 데이터에 사용할 수 있는 충분한 공간이 필요
* 일반적으로 pcp-zeroconf 기본 설정을 사용할 때 10Gb이면 충분
<br>

### 3.2 패키지 설치

```bash
dnf install pcp-zeroconf
```

실행 결과
```
[root@aap-db1 ~]# dnf install pcp-zeroconf
...<snip>...

[root@aap-db1 ~]#
```
<br>
<br>