# Harbor Registry – Portfólio DevOps

## Instalação

```bash
kubectl create namespace harbor
helm repo add harbor https://helm.goharbor.io
helm repo update
helm install harbor harbor/harbor -n harbor -f values.yaml
```

## Acesso

URL: http://harbor.local:31764

Usuário: admin  
Senha: Minh@S&nh@

### Criação do projeto no Harbor

Nome: portifolio-devops  
Privado

Primeiro push de imagem:

```bash
docker login harbor.local:31764
docker pull nginx:latest
docker tag nginx:latest harbor.local:31764/portifolio-devops/nginx:latest
docker push harbor.local:31764/portifolio-devops/nginx:latest
```

### Observação

> Por se tratar de uma instalação sem certificado ssl, é necessário informar ao Docker que a url harbor.local é insegura.

Em um host Linux, faça:

```bash
cat > /etc/docker/daemon.json << 'EOF'
{
  "insecure-registries": ["harbor.local:31764"]
}
EOF

systemctl restart docker
```

Em cada Node do cluster, duas configurações precisam ser realizadas.

**Primeiro, configure o acesso ao Harbor:** 

```bash
mkdir -p /etc/containerd/registries/"harbor.local:31764"

cat > "/etc/containerd/registries/harbor.local:31764/hosts.toml" << 'EOF'
server = "http://harbor.local:31764"

[host."http://harbor.local:31764"]
  capabilities = ["pull", "resolve", "push"]
  skip_verify = true
EOF

sed -i "s|config_path = '/etc/containerd/certs.d:/etc/docker/certs.d'|config_path = '/etc/containerd/registries'|" /etc/containerd/config.toml

systemctl restart containerd

```

**Segundo, adicione o endereço harbor.local no arquivo de hosts:**

```bash
echo "IP_NODE_MASTER harbor.local" >> /etc/hosts
```