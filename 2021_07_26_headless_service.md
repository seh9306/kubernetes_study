ClusterIp가 없는 서비스로 단일 진입점이 필요 없을 때 사용한다.

pod-ip-addr.namespace.pod.cluster.local 로 접근한다. 

```yaml
apiVersion: v1
kind: Service
metadata:
    name: headless-service
spec:
    type: ClusterIP
    clusterIP: None
    selector:
        app: webui
    ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```
