---
title: "2-9 Pods With Yaml"
date: 2021-06-24T23:44:29+09:00
---

# Kubernetes yaml 형식

```yaml
apiVersion:
kind:
metadata:
spec:
```

일반적인 yaml형식은 위와같다.

- apiVersion: 사용하려는 오브젝트에 따라 다르게 기술된다. 대표적인 예시는 다음과 같다.

| kind       | Version |
| ---------- | ------- |
| Pod        | v1      |
| Service    | v1      |
| ReplicaSet | apps/v1 |
| Deployment | apps/v1 |

- kind: 생성할 오브젝트의 타입을 지정한다. 여기에서는 pod를 만들거기에 pod라 기술한다. 위 표에서 왼쪽 필드에 있는 오브젝트들이 여기에 해당한다.
- metadata: 위에서 소개된 2가지와는 다르게 string 타입이 아닌 dictionary타입이 기술된다.

  ```yaml
  metadata:
    name: myapp-pod
    labels:
      app: myapp
      type: front-end
  ```

- spec: dictionary 형태로 정의된다. 여기에는 컨테이너들에 관한 정보가 들어간다. 또한 이 컨테이너에관한 정보는 여러가지가 들어가서 multi-container pod를 구성할 수도 있다.

  ```yaml
  spec:
  containers:
      - name: nginx-container
      image: nginx
  ```

  name 앞쪽을 보면 **-**가 들어가 있는 것을 볼 수 있다. 이것은 해당 아이템이 리스트에서 첫 번째 요소라는 것을 의미한다.

## command

### 모든 Pod의 정보를 가져오기

```bash
kubectl get pods
```

### 특정 파드의 정보에 대해 조회하기

```bash
kubectl describe pod myapp-pod
```
