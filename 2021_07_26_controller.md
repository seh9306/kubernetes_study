## 컨트롤러

### Controller API 7가지

- ReplicationController
- ReplicaSet
- Deployment
- DaemonSet
- StatefulSet
- Job
- CronJob

컨트롤러는 Pod의 개수를 보장해주는 역할을 한다.

```
kubectl create deployment [배포 이름] --image=nginx --replicas=3
```

배포 컨트롤러를 만들어서 nginx 3개의 파드를 유지하도록 생성함.

## ReplicationController

쿠버네티스 v1으로 가장 기본적인 컨트롤러

## ReplicaSet

ReplicationController에서 변형된 컨트롤러. ReplicaSet의 부모역할을 Deployment가 한다. 

## Stateful Sets

상태를 유지해주는 컨트롤러
