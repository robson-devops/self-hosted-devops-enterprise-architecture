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