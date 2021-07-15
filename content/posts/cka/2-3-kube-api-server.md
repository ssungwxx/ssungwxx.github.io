---
title: "2-3 Kube Api Server"
date: 2021-06-08T13:22:59+09:00
---

# Kube-api server

앞쪽에서 설명했듯이 Kube api server는 쿠버네티스의 컴포넌트들을 관리하는 역할을 한다. 사용자가 `kubectl`을 통해 CLI 명령어를 입력하면 kube api server가 해당 명령어를 받게된다. 받은 다음 먼저, 해당 명령어가 권한이 있는지 그리고 유효한 명령인지를 확인한다. 그리고 유효하다면 ETCD 클러스터에서 해당 데이터를 가져오고 사용자가에게 응답해준다.

다른 예제를 통해 알아보자. 사용자가 pod를 생성하는 작업을 실행하면 아래의 순서대로 동작한다.

1. Authenticate User
2. Validate Request
3. Retrieve data
4. Update ETCD
5. Scheduler
6. Kubelet
