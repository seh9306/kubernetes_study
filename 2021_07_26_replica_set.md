ReplicaSet은 ReplicationController보다 다양한 라벨 셀렉터가 사용 가능하다.


### RC 정의 
```
apiVersion: v1
kind: ReplicationController
metadata:
    name: rc-nginx
spec:
    replicas: 3
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

### RS 정의 

```
apiVersion: apps/v1 # 버전이 달라짐
kind: ReplicaSet
metadata:
    name: rs-nginx
spec:
    replicas: 3
    selector:
        matchLabels: # 추가되는 부분 
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

MatchExpressions:
  - {key: tier, opertaor: In, values: [cache]}
  - {key: enviroment, operator: NotIn, values: [dev]} 

이런식으로 match 표현식을 사용할 수 있다.
