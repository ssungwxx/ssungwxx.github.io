---
title: "2-2 ETCD"
date: 2021-06-06T15:42:19+09:00
draft: true
tags: ["kubernetes", "ETCD"]
---
# ETCD란?
해당 강의에서는 

> ETCD is distributed reliable key-value store that is Simple, Secure&Fast.

라고 소개되었다. 현재 ETCD 홈페이지에는

> A distributed, reliable key-value store for the most critical data of a distributed system

라고 소개되었다.

## Key-value Store
Key-value Store는 ***key*** 와 ***value*** 형태로 저장되는 데이터 베이스이다.
키의 중복은 허용되지 않으며 일반적인 상황의 데이터베이스로는 적합하지않다. 대신 Configuraion과 같은 데이터의 일부를 저장하기에는 적합하다. 왜냐하면 쓰기와 읽기 속도가 빠르기 때문이다.

# ETCD in Kubernetes
쿠버네티스내의 Cluseter들은 nodes, pods, configs, secrests, accounts, roles 그리고 그들을 연결해주는 정보 등 다양한 정보들이 필요한데 이러한 정보들은 ETCD에 저장된다.

해당 강좌에서는 ETCD의 구성 방법에는 두가지가 있다고한다.
- 직접 설치하고 연결하는 방법
- kubeadm으로 자동으로 설치되는 경우

## From scractch
만약 직접 설치한다면 ETCD 바이너리 파일을 다운받고 실행시킨다. 그리고 master node에서 직접 지정을 해주어야한다.
```cmd
--advertis-client-urls https://${INTERNAL_AP}:2379
```
이와같은 옵션을 통해서 직접 지정해줄 수 있다.

## Kubeadm
기본저긍로는 Kubeadm을 통해 자동으로 생성된다. 생성된 pod는 kube-system 네임스페이스에 ***etcd-master*** 라는 이름으로 존재한다.

### HA Environment
ETCD를 HA(High availability)모드로 사용할 경우 여러개의 ETCD 인스턴스들을 Master node에 가지게 될 것이다. 이러한 경우 ETCD는 각 서비스가 올바른 정보를 가지고 있는지 확인할 수 있어야한다. 이는 ```initial-cluster- controller``` 옵션을 통해 설정할 수 있다.

위에 관한 더 자세한 내용은 다음에 배울 챕터에서 다룰 예정이다.