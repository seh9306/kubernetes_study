Deployment는 ReplicaSet을 관리해주는(?) API이다.

생각을 해보면 ReplicaSet이나 ReplicationController는 이미 Pod의 수가 충족될 경우 템플릿을 변경해도 즉시 업데이트가 발생하지 않는다.

직접 파드를 수동으로 지워줘야하는데 이 부분은 Deployment로 해결할 수 있다.

Deployment는 롤링 업데이트 시에 새로운 레플리카 셋으로 롤링 업데이트한다. 그럼 당연히 파드도 업데이트 된다.

정의를 보면 ReplicaSet과 거의 똑같다. 

```
apiVersion: apps/v1
kind: Deployment
metadata:
    name: deploy-nginx
spec:
    replicas: 3
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

배포를 생성하면, rs, pod 가 순차적으로 생성된다. 

생성된 파드의 이름을 보면 규칙이 있다.

```
NAME                            READY   STATUS    RESTARTS   AGE   IP            NODE     NOMINATED NODE   READINESS GATES   LABELS
deploy-nginx-6d4c4cc4b8-7qtg2   1/1     Running   0          57s   10.244.2.69   node-2   <none>           <none>            app=webui,pod-template-hash=6d4c4cc4b8
deploy-nginx-6d4c4cc4b8-bvxsq   1/1     Running   0          56s   10.244.2.68   node-2   <none>           <none>            app=webui,pod-template-hash=6d4c4cc4b8
deploy-nginx-6d4c4cc4b8-hbmlf   1/1     Running   0          56s   10.244.2.70   node-2   <none>           <none>            app=webui,pod-template-hash=6d4c4cc4b8
```

### 이름 규칙
- [해당 rs를 관리하는 배포이름]-[해당 Pod를 만들어낸 rs의 hash]-[Pod의 hash]

레플리카의 수를 변경하면 그에 맞게 rs가 동작하여 Pod의 수를 조절한다.
```
kubectl scale deploy deploy-nginx --replicas=2
```

아래 방법으로 deploy의 이미지의 버전이나 다른 이미지로 수정할 경우
```
kubectl edit deploy deploy-nginx
```
롤링 업데이트가 발생하며, 새로운 rs을 만들어낸다. rs의 템플릿을 수정할 경우에는 이러한 롤링 업데이트가 발생하지 않으며, 정상적인 상황에서 업데이트하기 위해서는 직접 파드를 다 지워야한다.


