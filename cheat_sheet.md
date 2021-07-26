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

### 네임스페이스 생성하기

```
kubectl create namespace [네임스페이스이름]
```

### 네임스페이스 스위칭하기 ( 새로운 컨텍스트 생성 )

컨텍스트, 클러스터, 유저 이름은 알아서 지정하면 됨
```
kubectl config set-context [컨텍스트이름] --cluster=[클러스터이름] --user=[유저이름] --namespace=[네임스페이스이름]
```

컨텍스트 변경하기
``` 
kubectl config use-context [컨텍스트이름]
```

현재 컨텍스트 확인하기
```
kubectl config current-context 
```

### 네임스페이스 스위칭하기 ( 기존 컨텍스트 변경 )

```
kubectl config set-context --current --namespace=[네임스페이스이름]
# 확인하기
kubectl config view --minify | grep namespace:
```

### api 확인하기

```
kubectl explain pod
kubectl explain service 
```

### linux watch 명령어를 이용해서 pod 감시하기

```
watch kubectl get pods -o wide
```

### 멀티 파드에서 특정 컨테이너로 접근

```
kubectl exec [파드이름] -c [컨테이너이름] -it -- /bin/bash
```

### 롤링 업데이트에 대한 히스토리 보기

디플로이에 대한 히스토리를 보기 위해서는 디플로이 생성 시 --record 옵션을 줘야함. 

```
kubectl rollout history deployment [디플로이이름]
```

### 롤링 업데이트 시 일시정지, 재시작

```
kubectl rollout pause deployment [디플로이이름]
kubectl rollout resume deployment [디플로이이름]
```

### 롤링 업데이트 롤백

```
kubectl rollout undo deploy [디플로이이름]
```

### 토큰 관련
리스트
```
kubeadm token list
```
새로운 토큰 생성

```
kubeadm token create --ttl 1h
```

kubeadm init 했을 때 나오는 그 토큰(--token {위에 나온 결과})! 생성 후 1일이면 만료되기 때문에 클러스터에 새롭게 조인할 때 새로운 토큰을 생성해야한다. 

### 컨트롤러 삭제 시 pod를 유지하는 명령

```
kubectl delete [컨트롤러타입] [컨트롤러이름] --cascade=false
kubectl delete rs rs-nginx --cascade=false
```

