### livenessProbe 헬스체크

#### HTTP
```
livenessProbe:
    httpGet:
        path: /
        port: 80
```
#### TCP
```
livenessProbe:
    tcpSocket:
        port: 80
```
#### exec
exec 명령을 전달하고 종료코드가 0이 아니면 컨테이너 재시작
```
livenessProbe:
    exec:
      command:
      - ls
      - /data/file
```

```
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
spec:
    containers:
    - name: nginx-container
      image: nginx:1.14
      ports:
      - containerPort: 80
        protocol: TCP
      livenessProbe:
          httpGet:
              path: /
              port: 80        
```

deployment는 replicaSet을 만들고 replicaSet은 파드들을 만든다. 

deploy로부터 만들어진 rs와 pod는 삭제되어도 다시 생성된다.

### Deployment에 대해서

Deployment는 롤링 업데이트(?) 말 그대로 배포를 관리하는 역할으로 보임

rs는 pod의 갯수 보장 컨트롤러???의 역할을 하는 것으로 보임

### 디플로이 롤링업데이트 하는 방법

```
kubectl set image deploy [디플로이이름] [컨테이너이름]=[도커이미지:버전]
```

디플로이 설정 파일에서 직접 이미지를 수정하고 apply 시켜서 업데이트할 수 있다.
```
kubectl apply -f [디플로이설정파일]
```
