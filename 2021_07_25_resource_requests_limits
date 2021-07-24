## 리소스 요청과 제한

쿠버네티스는 파드 생성 시 리소스에 대한 요구사항을 작성할 수 있다.

최소 사양과 최대 사양을 설정할 수 있다.

최소 사양은 Resource reqeusts

최대 사양은 Resource limits

최소 사양 같은 경우는 생성될 파드 자체에 리소스 할당이 많이 필요한데 배치된 노드의 리소스가 부족할 경우를 대비해서 설정하면 유용하다

```
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod-resource
spec:
    containers:
    - name: nginx-container
      image: nginx:1.14
      ports:
      - containerPort: 80
        protocol: TCP
      resources:
          requests: # 최소 사양
              cpu: 200m
              memory: 250Mi
          limits: # 최대 사양
              cpu: 1
              memory: 500Mi
```

- 단위

```
1Mi = 1024 K
1M = 1000 K

1 core = 1000m
200m = 0.2 core
```

requests를 설정했을 때 그에 맞는 노드가 없을 경우 해당 파드는 Pending 상태로 유지된다.
