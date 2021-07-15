---
title: "2-6 Kubelet"
date: 2021-06-24T21:24:07+09:00
---

# Kubelet

`Kubelet`은 선박들의 함장과 같은 역할을한다. 마스터 노드의 스케쥴러와 통신하며 어떻게 컨테이너들을 실을지 결정하고 배의 상태(Pod들의 상태)를 주기적으로 알려준다.

Kube-scheduler에게서 Node 등록에 관한 명령을 받게되면 kubelet은 POD를 생성하고 Conatainer engine(보통 Docker)에 이미지를 실행하고 인스턴스를 생성한다.

그리고 Kubelet은 컨테이너의 상태를 모니터링하고 그에 관한 내용을 kube-api로 보내준다.
