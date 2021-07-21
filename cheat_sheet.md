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

### 쿠버네티스 파드 테스티 실행 하기 (dry-run)

```
kubectl run [파드이름] --image=[도커이미지] --port 80 --dry-run=client
```

### 파드를 create로 실행하기

테스트 하기 전에 yaml 파일 하나를 쉽게 만들어 본다.

```
kubectl run test --image=nginx --port 80 --dry-run=client -o yaml > test-pod.yaml
```

생성된 pod 설정 파일로 파드를 만들어본다. 위에서 run 한건 dry-run으로 실제 파드가 생성되는건 아니다. 테스트 실행(?) 정도로 보면 됨.

```
kubectl create -f test-pod.yaml 
```

위 옵션으로 설정 파일로도 파드 생성/실행이 가능하다. 
