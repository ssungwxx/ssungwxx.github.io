---
title: "2-5 Kube Scheduler"
date: 2021-06-11T01:26:33+09:00
draft: true
tags: ["kubernetes"]
---

# Kube Secheduler

앞서 이야기했듯이 Kuber scheduler는 노드에 pod에 스케쥴링하는 작업을 담당한다.
여기서 명심해야 할 것은 scheduler는 pod가 어느 노드로 갈지만 결정한다. pod를 생성하는 것과 같은 역할은 kubelet이 담당한다.

좀 더 자세히 말하자면, 다양한 컨테이너들이 노드위에 적재될 때, 각각 다른 사이즈가 존재할 것이다. 사용자는 당연히 좀 더 효율적인 분배를 원할 것이고 이러한 역할을 scheduler가 담당한다.

만약 일정양이 필요한 작업이 있고 노드가 여러개 존재한다면 scheduler는 2가지 작업으로 해당 컨테이너를 적절한 노드에 배치한다.

1. Filter nodes
2. Rank nodes

**_FilterNodes_**는 컨테이너가 필요한 자원보다 적은 자원만 수용가능한 노드들을 선별한다. 만약 CPU 10을 요구하는 컨테이거 잇따면 CPU 10미만의 노드들을 필터링한다.

**_RankNodes_**는 남은 노드들 중에서 더 최적화할 수 있는 노드에 최종적으로 컨테이너를 배치한다. 예를 들어 10만큼의 CPU가 필요한 컨테이너가 있고, CPU 12, 16을 가진 노드가 있다고 가정하자. 만약 해당 컨테이너들에 적재된다면 12인 Node는 2만큼의 여유분을 16인 노드는 6만큼의 여유분을 가지게 된다. 이러한 경우 6만큼 남는 노드의 Rank를 더 높다고 측정하여 해당 노드에 배치하게된다.

Memory 배치 전략에서 **_Best_** 랑은 조금 다른 방식의 랭킹이다. 해당 전략에서는 최대한 여유분이 없도록 한다면 쿠버네티스에서는 남은 노드에 더 큰 작업이 올 수 있도록 여유로운 노드에 배치를 한다.

## More Later...

- Resource Requirements and Limits
- Taints and Tolerations
- Node Selectors/Affinity
  와 같은 것들은 해당 섹션이 아닌 뒤에서 다룰 예정이다.
