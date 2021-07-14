---
title: Pod
weight: 20
menu:
  notes:
    name: Pod
    identifier: cka-pod
    parent: cka-notes
    weight: 20
---

{{< note title = "Create simple nginx pod" >}}

```
kubectl run nginx --image=nginx
```

{{</ note>}}

{{<note title = "Find image of pod" >}}

```
kubectl describe pod new pods-bcvm4 | grep -i image
```

{{</ note>}}

{{<note title = "View detail of pod" >}}

```
kubectl get pods -o wide
```

{{</ note>}}

{{<note title = "Create simple pod with dry run to yaml" >}}

```
kubectl run redis --image redis --dry-run=client --o yaml > pod.yaml
```

{{</ note>}}
