---
title: "2-1 Cluster Architecture"
date: 2021-06-06T14:40:52+09:00
---

# 쿠버네티스의 클러스터 구조

## 시작하기에 앞서

해당 챕터에서는 쿠버네티스의 아키텍쳐를 고수준부터 설명을 하고 앞으로의 강의에서 좀 더 구체적으로 내려가며 각각의 컴포넌트들에 대해서 알아볼 것이다.

먼저, 각 클러스터의 각 컴포넌트들의 기능과 역할에 대해 알아본다. 쿠버네티스의 아키텍쳐를 선박들에 비유하며 설명할 예정이다.

> Kuberntes 라는 명칭의 유래는 Helmsman(조타수)를 의미하는 그리스어에서 유래했다. 또한, Kubernetes는 Container Orchestration 이기 때문에 내부 클러스터들의 동작이 선박들이 움직이는 모습에 비유를 많이한다.

쿠버네티스에는 두가지의 선박들로 비유를 할 수 있다. 하나는 직접 바디를 건너 컨테이너를 옮기고 컨트롤 하는 Cargo ship(화물선), 다른 하나는 이러한 화물선들을 지켜보고 관리하는 Control ship이다.

## Cargo ship(Workder Node)

워커노드는 쿠버테니스 내부에서는 컨테이너를 싣는 역할을 한다. 실제로는 컨네이너를 호스팅하는 역할을 담당하는 것이다.

### Kubelet

각 선박에는 모든 행동들을 관리하는 역할을 하는 Captain이 존재한다. 쿠버네티스에서는 해당 역할을 **_Kubelet_**이 담당한다.

이는 수행받은 작업을 받아 수행하고 수행된 내용과 상태를 다시 Master Node에게 보낸다.

### Kube proxy

만약 웹서버가 동작할 때 각각 다른 컨테이너들간의 소통이 필요하다면 어떻게 해야할까? 예를 들어, 한 컨테이너에서 웹서버가 동작하는데 만약 다른 컨테이너, 노드에서 동작중인 DB에 접근하기 위해서는 어떻게 해야할까? 이러한 역할을 **_Kuber proxy_**가 담당한다.

이러한 Kube proxy는 각 컨테이너마다 존재하며 proxy들끼리 소통을 한다.

## Control ship(Master Node)

누군가는 컨테이너를 싣고 어떻게 운항할지에 대한 계획을 세워야한다. 계획뿐만 다른 선박과의 소통, 그리고 감시하는 역할은 **_Master Node_**에서 행한다.

### ETCD cluster

많은 컨테이너들이 매일 실리고 내려갈 것이다. 그렇다면 이러한 선박들에 대한 정보를 관리하는 역할이 필요할 것이다. 이러한 정보들은 고가용성 **_key-value_** 저장소로 알려진 **_ETCD_**가 수행한다.

### Kube scheduler

그렇다면 ETCD에 있는 정보들을 선박에 싣는 역할을 수행하는 것은 어디서 담당할까? 바로 **_Kube scheduler_**이다. Scheduler는 Worker Node에 어떠한 정보가 필요한지 확인을 하고 필요한 정보를 ETCD에서 전달해주는 역할을 담당한다.

### Controller manager

갑판에는 다른 사무실을 가지고 특별한 일을 처리하는 부서가 존재한다.

예를들어, 선박들이 혼잡해져서 컨트롤이 필요해거나 선박이 예상치못한 문제에 빠졋을 경우 해당 부서가 이를 담당하는데, 이들은 새로운 컨테이너를 만들고 사용가능하도록 할 수 았다. 이러한 사무실은 선박들간의 소통도 담당한다.

쿠버네티스에서는 이러한 컨트롤을 하기위한 컨트롤러들이 존재하는데 바로, **_Node controller_** 와 **_Replication controller_** 라고 불리고 각각 다른 영역을 담당한다.

Node Controller는 노드에 관한 부분을 담당한. 노드들을 새로만들거나 사용불가능해진 노드들을 파괴하기도한다.

Replication Controller는 Replication Group 내에 있는 컨테이너들이 실행되는 숫자를 조절한다.

### Kube APIServer

그렇다면 위와 같은 부서들이 소통하고 관리하는 역할을 누가 수행할까? 바로 한 단계 더 높은 단계에 있는 **_Kube APIServer_**가 수행한다. Kube APIServer는 외부 유저들과 소통하며 명령들을 수행하기도한다.

## Container runtime engine

DNS 서비스는 컨테이너 형태를 배포해준다. 이 컨테이너들을 실행가능하게 해주는 역할을 **_Container runtime engine_**이다.

대표적으로 **_Docker_**가 있다. 하지만 무조건 Docker를 사용해야하는 것은 아니다. 쿠버네티스는 **_ContainerD_** 나 **_Rocket_** 도 지원해준다. 또한, Kubernetes에서는 v1.20버전부터 컨테이너 런타임으로 Docker사용을 Deprecate했다. 자세한 내용은 아래의 링크에서 확인할 수 있다.

[Don't Panic: Kubernetes and Docker
](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/)
