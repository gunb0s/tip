```
kind create cluster --config kind-example-config.yaml
```

```yaml
kind: Cluster

apiVersion: kind.x-k8s.io/v1alpha4

nodes:

- role: control-plane

- role: worker

- role: worker

- role: worker
```

### metrics-server 구성
```shell
$ helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/
$ helm upgrade --install metrics-server metrics-server/metrics-server -f metrics-server-values.yaml
```

```yaml
args:
	- --kubelet-insecure-tls
```
메트릭을 수집할 때 발생하는 인증서 오류를 비활성화...

### kubernetes-dashboard 구성
```bash
$ helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
$ helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```

```bash
$ kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
```

인증 토큰 생성
```yaml
apiVersion: v1

kind: ServiceAccount

metadata:

name: admin-user

namespace: kubernetes-dashboard

---

apiVersion: rbac.authorization.k8s.io/v1

kind: ClusterRoleBinding

metadata:

name: admin-user

roleRef:

apiGroup: rbac.authorization.k8s.io

kind: ClusterRole

name: cluster-admin

subjects:

- kind: ServiceAccount

name: admin-user

namespace: kubernetes-dashboard
```

```bash
$ kubectl -n kubernetes-dashboard create token admin-user
```
인증토큰을 입력했는데도 안된다?? -> 쿠키를 지워주기