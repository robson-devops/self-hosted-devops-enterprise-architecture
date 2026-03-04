# 07 - GitOps Layer

## Objetivo

A camada 07-gitops implementa o modelo de entrega contínua declarativa utilizando Git como fonte única da verdade (Single Source of Truth).

Esta camada substitui o modelo tradicional onde o pipeline executa o deploy diretamente no cluster por um modelo pull-based, no qual o próprio cluster reconcilia automaticamente o estado desejado definido no repositório.

O objetivo principal é:

- Garantir rastreabilidade total de mudanças
- Eliminar deploy manual em cluster
- Implementar reconciliação automática (self-healing)
- Reduzir riscos operacionais
- Evoluir a arquitetura para um modelo de Platform Engineering
- Implementar governança por ambiente

---

## Ferramenta Adotada

Argo CD

O Argo CD foi escolhido como controlador GitOps oficial da arquitetura.

Motivos:

- Amplamente adotado em ambientes enterprise
- Modelo pull-based nativo
- Integração profunda com Kubernetes
- Suporte nativo a Helm e Kustomize
- Interface visual para auditoria e rastreabilidade
- Controle de RBAC por projeto e aplicação
- Suporte ao padrão App of Apps

---

## Modelo Operacional

O cluster Kubernetes passa a operar sob o seguinte princípio:

> O estado desejado do cluster é definido exclusivamente via Git.

Fluxo operacional:

1. Desenvolvedor realiza alteração no repositório
2. Pipeline executa build e validações de segurança (camada 06)
3. Imagem é assinada e publicada
4. Manifestos ou valores Helm são atualizados
5. Argo CD detecta alteração no repositório
6. O cluster reconcilia automaticamente o estado desejado

Nenhum deploy é executado diretamente pelo pipeline no cluster.

---

## Modelo Arquitetural Implementado

A camada utiliza o padrão:

App of Apps

Estrutura lógica:

root-application  
→ environments  
→ aplicações por ambiente  
→ overlays  
→ base

Esse modelo permite:

- Escalabilidade
- Separação por ambiente
- Promoção controlada
- Expansão futura para multi-cluster
- Governança por projeto

---

## Instalação do Argo CD

O Argo CD é instalado:

- Dentro do cluster Kubernetes
- Em namespace dedicado: argocd
- Como conjunto de Deployments e Services
- Com acesso controlado via Ingress
- Com projeto configurado para governança

Modelo adotado: GitOps Pull-Based

---

## Estrutura Organizacional

07-gitops/
- bootstrap/ → Root Application (App of Apps)
- argocd/ → Manifests de instalação e configuração de projects
- environments/ → Applications organizadas por ambiente
- adr/ → Decisões arquiteturais da camada

Estrutura de environments:

environments/
- kustomization.yaml
- dev/
- staging/
- prod/

Cada ambiente contém:
- Application(s) do Argo CD
- kustomization.yaml local

---

## Separação por Ambientes

A arquitetura adota segregação explícita:

| Ambiente | Sync Automático | Objetivo |
|-----------|----------------|----------|
| Dev       | Sim            | Feedback rápido |
| Staging   | Não            | Validação controlada |
| Prod      | Não            | Deploy governado |

Dev possui sync automático habilitado.  
Staging e Prod exigem sincronização manual, garantindo controle e governança.

---

## Integração com a Aplicação (Kustomize)

As aplicações seguem o padrão:

examples/demo-app/
- base/
- overlays/
  - dev/
  - staging/
  - prod/

Base contém os manifests reutilizáveis.  
Overlays aplicam:
- Namespace específico
- Sufixo por ambiente
- Labels de ambiente
- Versão da imagem por ambiente

Isso permite promoção controlada entre ambientes.

---

## Responsabilidades da Camada

- Implementar Git como fonte única da verdade
- Automatizar deploy declarativo
- Detectar e corrigir drift
- Garantir rollback simples via Git
- Implementar segregação por ambientes
- Suportar promoção entre ambientes
- Preparar arquitetura para multi-cluster

---

## Fluxo Arquitetural Consolidado

Git Push  
→ CI (Build + DevSecOps)  
→ Imagem assinada publicada  
→ Atualização declarativa no repositório  
→ Argo CD detecta mudança  
→ Reconciliação automática  
→ Deploy no Kubernetes

Nenhum deploy é executado manualmente no cluster.

---

## Integração com as Camadas Anteriores

Camada 05 (Kubernetes):
- Cluster executa workloads declarativamente

Camada 06 (DevSecOps):
- Apenas imagens validadas e assinadas são promovidas
- Segurança aplicada antes da reconciliação

Camada 07 (GitOps):
- Controle total do estado do cluster via Git
- Reconciliação contínua

---

## Benefícios Arquiteturais

- Redução de erro humano
- Rastreabilidade completa via histórico Git
- Rollback simples com git revert
- Ambiente declarativo e auditável
- Base sólida para Platform Engineering
- Preparado para crescimento organizacional

---

## Evolução Futura

A arquitetura pode evoluir para:
- Multi-cluster management
- Argo CD central gerenciando múltiplos clusters
- Promotion automática entre ambientes
- GitOps para infraestrutura
- Integração com Policy as Code avançado
- Progressive Delivery (Argo Rollouts)
- Separação entre repositório de plataforma e repositório de aplicações

---

Autor: Robson Ferreira  
Projeto: self-hosted-devops-enterprise-architecture