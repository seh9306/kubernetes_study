데몬셋은 전체 노드에서 pod가 한 개 씩 실행되도록 보장 -> 한 개 만? 최소 한 개? -> 한 개 만임 왜냐면 데몬셋 설정 파일에서 replicas를 지정하면 에러가 발생함

CNI는 데몬 셋이다. 

쿠버네티스에서 컨트롤러는 파드 실행 요청을 하는 존재(?) 레플리카 셋 <-> 데몬 셋 

### daemonset-exam.yaml
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
    name: daemonset-nginx
spec:
    selector:
        matchLabels:
            app: webui
    template:
        metadata:
            name: nginx-pod
            labels:
                app: webui
        spec:
            containers:
            - name: nginx-container
              image: nginx:1.14
```

두 셋이 굉장히 유사함

### replicaset-exam.yaml
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: daemonset-nginx
spec:
    replicas: 3 # 레플리카 셋에서만 설정 가능
    selector:
        matchLabels:
            app: webui
    template:
        metadata:
            name: nginx-pod
            labels:
                app: webui
        spec:
            containers:
            - name: nginx-container
              image: nginx:1.14
```

ds는 rs와 다르게 아래 명령을 통해 이미지를 변경하면 롤링업데이트가 진행된다.

```
kubectl edit ds daemonset-nginx
```

롤백 명령어

```
kubectl rollout undo daemonset daemonset-nginx
```
