# homelab-gitops

```bash
# Permitir storage do grafana
microk8s enable community --wait
microk8s enable ingress
microk8s enable traefik --wait
microk8s enable dns
microk8s enable metallb:192.168.15.201-192.168.15.201
microk8s enable hostpath-storage
sudo microk8s enable observability --loki --tempo
microk8s enable metrics-server

microk8s kubectl delete pod test-loki-curl --force --grace-period=0

microk8s kubectl run test-loki-curl --rm -i --tty --image=curlimages/curl -- http://kube-prom-stack-kube-prome-prometheus.observability
microk8s kubectl edit statefulset loki -n observability

microk8s kubectl edit statefulset loki -n observability
# http://loki.observability:3100
# image: grafana/loki:2.9.11
microk8s kubectl edit statefulset tempo -n observability
microk8s kubectl edit deployment tempo -n observability
# image: grafana/tempo:2.4.2
microk8s kubectl rollout restart deployment kube-prom-stack-grafana -n observability
microk8s kubectl get secret kube-prom-stack-grafana -n observability -o jsonpath="{.data.admin-password}" | base64 --decode; echo
microk8s kubectl edit configmap kube-prom-stack-kube-prome-grafana-datasource -n observability

```

# Setup longhorn 
- https://k3s.guide/docs/storage/setup-longhorn/

```bash
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/master/deploy/longhorn.yaml
kubectl delete -f https://raw.githubusercontent.com/longhorn/longhorn/master/deploy/longhorn.yaml

# abrir shell para pod
kubectl exec -it immich-647fd96f9d-lwp6j -n immich -- /bin/bash
# port forward
kubectl port-forward -n longhorn-system svc/longhorn-frontend 8080:80

# Adicionar dados ao k8s
sudo rsync -av /mnt/dados_vm/docker/immich/ /var/lib/rancher/k3s/storage/pvc-8dd6fc92-66f8-477a-b6b8-01fddeb20008_immich_immich-uploads/

rm -Rf /var/lib/rancher/k3s/storage/pvc-89a72a10-4181-4e09-a731-34c2f165d324_immich_immich-uploads

# Backup
sudo rsync -av /var/lib/rancher/k3s/storage/pvc-8dd6fc92-66f8-477a-b6b8-01fddeb20008_immich_immich-uploads/ /mnt/dados_vm/docker/immich/
```

# Comandos Helms

```bash
helm repo add kubernetes-homelab-helm-charts https://harish2k01.github.io/helm-charts/
helm repo update
helm show values kubernetes-homelab-helm-charts/homepage
helm show values pulse/pulse
helm search repo pulse/pulse --versions

helm repo list
helm search repo kubernetes-homelab-helm-charts
helm search repo kubernetes-homelab-helm-charts/adguard-home --versions

helm template kubernetes-homelab-helm-charts/adguard-home --generate-name
helm template meu-adguard kubernetes-homelab-helm-charts/adguard-home -f meu-values.yaml


```