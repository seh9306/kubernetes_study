오늘은 쿠버네티스를 좀 제대로 다시 해보려고 집 PC에 가상머신을 띄워서 설치를 시도해봤다.

쿠버네티스 클러스터를 구성하기 위해 가상머신을 사용하려고 했는데 모니터링 시스템 테스트하려고 설치해놨던 우분투와 그 우분투를 클론해서 빠르게 셋팅을 진행했다.

근데 이게 문제가 됬는데, OS에 설정된 호스트 네임으로 쿠버네티스 노드의 이름을 설정하는데 이게 똑같으니까 이미 등록된 노드라고 뜨는데 이걸 확인 못하고 망할 삽질을 계속해서 시간을 날렸다.

그리고 또 몰랐던게 마스터를 초기화하기 위해 `kubeadm init -pod-network-cidr=10.244.0.0/16`을 진행하면 ( 뒤 옵션은 flannel 을 셋팅하기 위해 주는 옵션이다. 정해진거라 바꾸지 말라함. )  

```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.183.132:6443 --token 54wcpz.9gssditkoc1neyfu \
	--discovery-token-ca-cert-hash sha256:0a269c11df7123ce3e347c34f1c15b91f77a95b8707bd816faed2f8cf748b053 
```

이런식으로 나와서, 워커 노드로 쓸 머신에서 kubeadm join ~ 을 하면 클러스터로 조인이 되는데.. 

이게 조인을 한 번 실패하면 그 다음부터 제대로 설정을 해놔도 

```
...
[ERROR FileAvailable--etc-kubernetes-pki-ca.crt]: /etc/kubernetes/pki/ca.crt already exists
...
```

자꾸 이렇게 뜨는데 kubeadm reset을 하고 다시 진행하니까 됨. 안그러면 계속 저렇게 뜸.. 뭐 포트를 사용하고 있다 이런것도 같이 계속 뜨고 어쨋든 리셋을 하고 제대로 시도하면 된다.

그리고 마스터 노드에서 kubectl이 동작을 안하면 권한 문제인데, 위에 control-plane 완료 설명을 잘 읽어보면 일반 계정과 관리자 계정 별로 권한 설정이 나온다.

일반 계정은 해당 계정에 .kube 디렉토리 밑에 설정을 확인해서 접근권한을 확인한다.

어쨋든 워커 노드에서 연결을 잘 해봤다. 

```
[preflight] Running pre-flight checks
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.
```

연결이 잘되면 위에처럼 나오는데 또 제대로 안 읽고 kubectl get nodes를 그 머신에서 해보다가.. 잘 보면 control-plane에서 하라고 되어있다 ㅋㅋ

마스터 노드로 들어가서 kubectl get nodes 하면 잘 연결되어있다..!

```
NAME     STATUS     ROLES                  AGE   VERSION
node-2   NotReady   <none>                 91m   v1.21.3
ubuntu   Ready      control-plane,master   3h    v1.21.3
```

node-1도 있는데 이것저것 막 설정하다보니 엉망진창 꼬여서버려서 node-2를 새로 만들었다! 안될땐 다시 하는게 최고
