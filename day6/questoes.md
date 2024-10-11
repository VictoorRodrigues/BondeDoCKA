## DAY6

### Questão 1

O nosso gerente observou no dashboard do Lens que um dos nossos nodes não está bem. Temos algum problema com o nosso cluster e precisamos resolver agora.

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
``

### Resposta 1

```yaml
```

```bash
```

### Questão 2

### Resposta 2

```bash
```
