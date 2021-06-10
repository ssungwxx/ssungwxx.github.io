---
title: "2-4 Kube Controller Manager"
date: 2021-06-11T01:17:14+09:00
draft: true
---

# Kube controller manager

쿠버네티스에는 사무실이나 부서 역할을 담당하는 다양한 컨트롤러들이 존재한다. 컨트롤러는 종류에 따라 배들을 모니터링 하기도하고 컨테이너를 확인하고 돌보기도한다.

이러한 컨트롤러들은 역시나 모두 **_kube api server_**를 통해 통신한다.

Namespace controller, Deployment Controller, Endpoint controller 등 다양한 컨트롤러들이 있는데 이러한 컨트롤러들을 모두 관리하는 것이 kube controller manager이다.

Kube controller manager는 직접 다운로드 받아 배포할 수도 잇지만, kubeadm을 통해 쿠버네티스를 설정하면 `kube-system` 네임스페이스에 `kube-controller-manager-master` 이라는 이름으로 배포된다.
