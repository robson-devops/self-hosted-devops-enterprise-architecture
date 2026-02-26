# 05 - Kubernetes Layer

## Objetivo

A camada 05-kubernetes é responsável por orquestrar e executar as aplicações containerizadas dentro da arquitetura DevOps.
Esta camada materializa a entrega contínua (CD), garantindo que imagens construídas na camada de infraestrutura sejam implantadas de forma segura,
declarativa e automatizada.

---

## Componentes

### 1. Kubernetes Cluster (Bare Metal)

* Cluster Kubernetes v1.32.x;
* 1 nó control-plane e 1 worker;
* Runtime: containerd;
* Sistema operacional: Ubuntu 24.04 LTS;
* Rede interna 192.168.15.0/24.

### 2. Helm (Package Manager)

* Gerenciamento declarativo das aplicações;
* Estruturação de chart reutilizável (demo-app);
* Versionamento de configurações por ambiente;
* Deploy com rollback automático (--atomic).

### 3. NGINX Ingress Controller

* Controlador de entrada HTTP/HTTPS;
* Gerenciamento de rotas por host;
* Integração com Services internos do cluster.

### 4. MetalLB (LoadBalancer Bare Metal)

* Implementação de LoadBalancer para ambiente on-premise;
* Pool de IP configurado: 192.168.15.240-192.168.15.250;
* Exposição externa real do Ingress Controller.

### 5. Namespace apps-dev

* Isolamento lógico da aplicação demo-app;
* Separação entre infraestrutura e workloads;
* Escopo dedicado para ambiente de desenvolvimento.

---

## Responsabilidades da Camada

* Orquestração de containers;
* Deploy automatizado via Helm;
* Rolling updates e rollback automático;
* Gerenciamento de namespaces;
* Segurança via ServiceAccount e imagePullSecrets;
* Exposição controlada via Ingress;
* Integração com CI/CD da camada 04-infrastructure.

---

## Decisões Arquiteturais

* Cluster Kubernetes self-hosted (bare metal);
* Deploy das aplicações exclusivamente via Helm;
* Versionamento de imagem por commit SHA (imutabilidade);
* Registry privado (Harbor) com autenticação obrigatória;
* Ingress Controller dedicado dentro do cluster;
* LoadBalancer implementado via MetalLB;
* Separação clara entre infraestrutura e workloads.

Ver ADR-0004 (Helm Deployment Strategy)
Ver ADR-0005 (Bare Metal LoadBalancer com MetalLB)

---

## Princípios Aplicados

* Infraestrutura declarativa;
* Automação completa via pipeline;
* Imutabilidade de artefatos;
* Separação de ambientes;
* Segurança por padrão;
* Arquitetura enterprise simulada.

---

## Fluxo de Deploy Implementado

Git Push
→ Pipeline GitLab CI
→ Build com Kaniko
→ Push para Harbor
→ Helm Upgrade/Install
→ Rolling Update no Kubernetes
→ Exposição via Ingress + MetalLB
→ Acesso via domínio (dev.demo.local)

---

## Próxima Camada

A próxima etapa da arquitetura pode evoluir para:

06-devsecops

Responsável por:

- Segurança no pipeline (SAST, DAST, Dependency Scan);
- Scan de imagens de container;
- Policy as Code;
- Gestão de vulnerabilidades;
- Shift-left security;
- Compliance e rastreabilidade.

---

## Estrutura Futura: Ingress e Storage

Os diretórios `ingress/` e `storage/` fazem parte da organização arquitetural da camada `05-kubernetes`, mesmo que ainda não estejam sendo utilizados ativamente pela aplicação de demonstração.

Esses diretórios foram mantidos intencionalmente para:

- Preservar a separação de responsabilidades dentro do cluster
- Antecipar evolução para workloads com persistência (Stateful)
- Definir estratégias futuras de exposição externa (Ingress Controller)
- Manter consistência estrutural entre ambientes

Os arquivos existentes nesses diretórios atuam como *placeholders documentais*, indicando que a fundação do cluster já considera:

- Estratégias de entrada de tráfego
- Estratégias de armazenamento e persistência

Essa decisão reforça a visão arquitetural do repositório como base corporativa e não apenas como ambiente de demonstração.

---

Autor: Robson Ferreira  
Projeto: self-hosted-devops-enterprise-architecture