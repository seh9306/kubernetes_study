## init container Pod

init container를 가지고 있는 Pod는 반드시 init container가 실행된 이후에 main container가 실행된다.

main container를 실행하기 전에 필요로 하는 컨테이너들을 init container로 만듬. 예를 들면 DB 컨테이너와 해당 DB를 쓰는 어플리케이션? 같은 경우

### 쿠버네티스 init container example
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup myservice.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for myservice; sleep 2; done"]
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup mydb.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for mydb; sleep 2; done"]
```

### 위 init container 를 발동시키기 위한 서비스 

```
apiVersion: v1
kind: Service
metatdata:
    name: myservice
spec:
    ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```


```
apiVersion: v1
kind: Service
metadata:
  name: mydb
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9377
```

---

Pod 을 생성하면 infra container를 같이 생성해준다. 

```
06a3a7d1830a   295c7be07902             "nginx -g 'daemon of…"   9 minutes ago   Up 9 minutes             k8s_web_app-deploy-67bb978b99-cqfpk_default_fc994b01-4dcb-4e7c-ba15-5c58fc1720d0_0
6f6fde610e61   295c7be07902             "nginx -g 'daemon of…"   9 minutes ago   Up 9 minutes             k8s_web_app-deploy-67bb978b99-5bzmp_default_69708ebc-60ab-4a12-83a9-4a3cc8a6a464_0
862302bf06f8   k8s.gcr.io/pause:3.4.1   "/pause"                 9 minutes ago   Up 9 minutes             k8s_POD_app-deploy-67bb978b99-cqfpk_default_fc994b01-4dcb-4e7c-ba15-5c58fc1720d0_0
7db6cd93332e   k8s.gcr.io/pause:3.4.1   "/pause"                 9 minutes ago   Up 9 minutes             k8s_POD_app-deploy-67bb978b99-5bzmp_default_69708ebc-60ab-4a12-83a9-4a3cc8a6a464_0
```

근데 그래서 뭐하는건데(?)

