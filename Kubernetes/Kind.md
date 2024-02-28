local에 kubernetes를 설치하기 위한 툴로 많이 사용
``

```bash
> kind create cluster --config kind-config.yaml
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
