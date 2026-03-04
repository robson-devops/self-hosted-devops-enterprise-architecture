# ADR-0008 – GitOps Strategy

## Status

Aceito

## Contexto

A arquitetura evoluiu até a camada 06 com pipeline CI/CD completo, incluindo segurança (DevSecOps), assinatura de imagens e políticas de cluster.

Até este ponto, o deploy ainda depende de execução direta via pipeline.

Esse modelo apresenta limitações:

- Dependência do pipeline para aplicar mudanças;
- Risco de drift entre estado desejado e estado real;
- Dificuldade de auditoria histórica de mudanças de infraestrutura;
- Baixa rastreabilidade operacional.

O projeto requer evolução para um modelo declarativo moderno amplamente adotado em ambientes enterprise.

---

## Decisão

Adotar o modelo GitOps pull-based como estratégia oficial de deploy.

Princípios definidos:

- Git como fonte única da verdade (Single Source of Truth);
- O cluster deve reconciliar seu próprio estado;
- Deploys não serão mais executados diretamente pelo pipeline;
- Mudanças devem ocorrer exclusivamente via Pull Request;
- Drift deve ser detectado e corrigido automaticamente.

O pipeline CI passa a ter responsabilidade apenas por:

- Build da imagem;
- Execução de validações de segurança;
- Publicação do artefato assinado;
- Atualização declarativa do repositório.

O cluster, por meio do controlador GitOps, passa a ter responsabilidade por:

- Detectar alterações no repositório;
- Aplicar mudanças;
- Reconciliar diferenças;
- Restaurar estado desejado em caso de divergência.

---

## Justificativa

1. GitOps é padrão moderno em ambientes Kubernetes enterprise;
2. Elimina dependência de execução manual;
3. Aumenta rastreabilidade e auditoria;
4. Facilita rollback via Git;
5. Reduz risco de erro humano;
6. Implementa modelo declarativo puro;
7. Permite evolução futura para multi-cluster.

---

## Consequências

### Positivas

- Deploy totalmente auditável;
- Reconciliação automática de drift;
- Melhor governança de mudanças;
- Separação clara entre build e deploy;
- Maior confiabilidade operacional.

### Negativas

- Curva de aprendizado adicional;
- Maior complexidade conceitual;
- Dependência do controlador GitOps para reconciliação;
- Necessidade de organização disciplinada do repositório.

---

## Melhorias Futuras

- Promotion automática entre ambientes;
- Multi-cluster management;
- Progressive delivery;
- GitOps também para infraestrutura;
- Integração com políticas avançadas de compliance.

---

## Fluxo da Arquitetura

Git Push  
→ CI (Build + DevSecOps)  
→ Publicação de imagem assinada  
→ Atualização declarativa no Git  
→ Controlador GitOps detecta mudança  
→ Reconciliação automática  
→ Estado aplicado no Kubernetes  

---

Autor: Robson Ferreira  
Projeto: self-hosted-devops-enterprise-architecture