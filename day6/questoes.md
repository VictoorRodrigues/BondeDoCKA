## DAY6

### Questão 1

O nosso gerente observou no dashboard do Lens que um dos nossos nodes não está bem. Temos algum problema com o nosso cluster e precisamos resolver agora.

### Resposta 1

```bash
kubectl get nodes
ssh node-com-problema
ps -ef |grep kubelet
docker ps
systemctl status kubelet
journalctl -u kubelet
whereis kubelet
vim /lib/systemd/system/kubelet.service.d/10-kubeadm.conf # ARRUMAR O PATH DO BINARIO DO KUBELET
# systemctl edit --full kubelet # ainda podemos usar esse comando ao invés de alterar o arquivo.
systemctl daemon-reload
systemctl restart kubelet
systemctl status kubelet
journalctl -u kubelet
```

### Questão 2

Temos um secret com o nome e senha de um usuário que nossa aplicação irá utilizar, precisamos colocar esse secret em um pod.
Detalhe, esse secret deve se tornar uma variável de ambiente dentro do container.

### Resposta 2

```bash
kubectl create secret generic credentials --from-literal user=victor --from-literal password=giropops --dry-run=client -o yaml > meu_secret.yaml
kubectl create -f meu_secret.yaml
```

```bash
kubectl run giropops --image nginx --dry-run=client -o yaml > pod_com_secret.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: giropops
  name: giropops
spec:
  containers:
  - image: nginx
    name: giropops
    resources: {}
    env:
    - name: MEU_USER
      valueFrom:
        secretKeyRef:
          name: credentials
          key: user
    - name: MEU_PASSWORD
      valueFrom:
        secretKeyRef:
          name: credentials
          key: password
    volumeMounts:
    - name: credentials
      mountPath: /opt/giropops
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  volumes:
  - name: credentials
    secret:
      secretName: credentials
```

```bash
kubectl create -f pod_com_secret.yaml
```
