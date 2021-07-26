Pod의 개수를 보장하며 파드 집합의 실행을 안정적으로 유지

```
apiVersion: v1
kind: ReplicationController
metadata:
    name: [RC이름]
spec:
    replicas: [Pod 개수]
    selector:
        key: value
    template:
        [Pod 템플릿]
```

직접 만들어보자.

```
apiVersion: v1
kind: ReplicationController
metadata:
    name: rc-test
spec:
    replicas: 2
    selector:
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

아래와 같은 결과가 나온다.

```
NAME            READY   STATUS    RESTARTS   AGE     IP            NODE     NOMINATED NODE   READINESS GATES
rc-test-gj97b   1/1     Running   0          4m10s   10.244.2.56   node-2   <none>           <none>
rc-test-ms5qz   1/1     Running   0          25m     10.244.2.53   node-2   <none>           <none>
```

여기서 ReplicationController는 app: webui 라벨을 갖고 있는 파드가 2개가 유지되도록 동작한다.

테스트를 해보기 위해서 간단한 Pod 파일을 만들어서 테스트 해보자.

```
kubectl run rc-test-pod --image=redis --dry-run=client --labels=app=webui -o yaml > redis.yaml
```

아래와 같이 파드에 대한 파일이 생성된다.

```
$ cat redis.yaml

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    app: webui
  name: rc-test-pod
spec:
  containers:
  - image: redis
    name: rc-test-pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

해당 파드를 생성해보자.

```
kubectl create -f redis.yaml
```

```
NAME            READY   STATUS        RESTARTS   AGE     IP            NODE     NOMINATED NODE   READINESS GATES
rc-test-gj97b   1/1     Running       0          8m52s   10.244.2.56   node-2   <none>           <none>
rc-test-ms5qz   1/1     Running       0          30m     10.244.2.53   node-2   <none>           <none>
rc-test-pod     0/1     Terminating   0          1s      <none>        node-2   <none>           <none>
```

생성도 안되고 바로 Terminating 된다.

RC의 설정에서 Pod의 템플릿이 변경된다고 하더라도, 즉시 반영되지 않는다. RC는 오로지 라벨을 보고 파드의 개수가 유지되고 있다면 해당 템플릿으로 파드를 업데이트하지 않는다. 

---

```
kubectl edit rc rc-test
```
에서 replicas를 3으로 변경

혹은 

```
kubectl scale rc rc-test --replicas=3
```

vi 편집기를 통해 replicas 를 수정하면 Pod 개수를 조절하는 것을 확인할 수 있다. 

그럼 이제 Pod 템플릿을 변경해도 업데이트가 되지 않는 것을 테스트해보자.

```
kubectl edit rc rc-test 
```

Pod 템플릿에서 nginx 의 버전을 1.15로 변경

```
kubectl describe pod rc-test-gj97b 
```

버전이 변경되지 않음을 확인.

하지만, 강제로 pod를 지우고 rc가 다시 파드를 생성하게되면

```
kubectl delete pod --all 
```

변경된 버전으로 파드가 생성된다.

