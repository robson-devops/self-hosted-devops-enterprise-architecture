# ADR-0009 – Adoção do Argo CD como Controlador GitOps

## Status

Aceito

## Contexto

Após a decisão de adoção do modelo GitOps (ADR-0008), tornou-se necessário definir qual ferramenta será utilizada como controlador GitOps da arquitetura.

As opções avaliadas foram:

- Argo CD
- Flux

Critérios considerados:

- Adoção no mercado enterprise;
- Facilidade de operação;
- Integração com Helm;
- Interface gráfica para auditoria;
- Suporte a múltiplos ambientes;
- Comunidade ativa e maturidade do projeto.

---

## Decisão

Adotar o Argo CD como controlador GitOps oficial da arquitetura.

Modelo de instalação:

- Instalado dentro do cluster Kubernetes;
- Namespace dedicado: argocd;
- Acesso controlado via Ingress;
- RBAC configurado por projeto.

Modelo operacional:

- Pull-based;
- Monitoramento contínuo do repositório;
- Reconciliação automática;
- Self-healing habilitado.

Estratégia arquitetural:

- Implementação inicial single-cluster;
- Preparação estrutural para futura expansão multi-cluster;
- Utilização do padrão App of Apps.

---

## Justificativa

1. Argo CD é amplamente adotado em ambientes corporativos;
2. Interface visual facilita auditoria;
3. Integração nativa com Helm;
4. Suporte robusto a múltiplos ambientes;
5. Modelo declarativo consistente com a arquitetura;
6. Comunidade madura e documentação extensa.

---

## Consequências

### Positivas

- Visualização clara do estado do cluster;
- Reconciliação automática e contínua;
- Melhor rastreabilidade de mudanças;
- Base sólida para expansão futura;
- Alinhamento com práticas modernas de Platform Engineering.

### Negativas

- Introdução de novo componente crítico no cluster;
- Necessidade de proteção adequada do namespace argocd;
- Requer governança disciplinada do repositório;
- Exposição do painel deve ser cuidadosamente protegida.

---

## Melhorias Futuras

- Implementação de Argo CD multi-cluster;
- Autenticação via SSO;
- Integração com políticas avançadas;
- Progressive delivery com Argo Rollouts;
- Auditoria centralizada.

---

## Fluxo da Arquitetura

Git Repository  
→ Argo CD monitora alterações  
→ Detecta divergência  
→ Aplica mudanças no cluster  
→ Mantém estado reconciliado  

---

Autor: Robson Ferreira  
Projeto: self-hosted-devops-enterprise-architecture