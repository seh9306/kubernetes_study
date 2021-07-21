### 노드에 대해 상세한 정보 얻기

```
kubectl get nodes -o wide 
```

### 쿠버네티스 CLI 명령에 대한 API 정보, 줄임말 정보가 나옴

```
kubectl api-resources
```

### 쿠버네티스 노드(물리장치)에 대한 자세한 내용

```
kubectl describe node [노드이름]
```

### 쿠버네티스 파드에 배쉬로 접속하는 방법

```
kubectl exec [파드이름] -it -- /bin/bash
```
