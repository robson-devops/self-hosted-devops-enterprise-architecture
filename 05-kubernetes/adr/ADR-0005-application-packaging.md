# ADR-0005 – Application Packaging Strategy with Helm

## Status

Accepted

## Contexto

Durante a implementação da camada `05-kubernetes`, a aplicação `demo-app` foi inicialmente implantada utilizando manifests Kubernetes estáticos (Deployment, Service e Ingress).

Essa abordagem foi adotada para:

- Validar a organização do cluster;

- Garantir entendimento detalhado dos recursos Kubernetes;

- Testar a integração com o pipeline CI/CD;

- Validar o fluxo completo: Build → Push → Deploy → Access.

Com a evolução do projeto, tornou-se necessário definir uma estratégia mais madura para:

- Padronização de deploy;

- Reutilização de templates;

- Versionamento da aplicação;

- Suporte a múltiplos ambientes (dev/prod);

- Redução de duplicação de YAML.

## Decisão

Foi decidido empacotar a aplicação utilizando **Helm Charts** como padrão oficial de deployment.

A estrutura adotada será:

05-kubernetes/  
└── helm/  
└── demo-app/  
├── Chart.yaml  
├── values.yaml  
├── values-dev.yaml  
├── values-prod.yaml  
└── templates/   

### Diretrizes definidas

1. Os manifests "raw" serão mantidos para fins educacionais e históricos;

2. O Helm Chart será a forma oficial de deploy da aplicação;

3. Serão utilizados arquivos `values` separados para ambientes distintos;

4. A pipeline CI/CD será ajustada para utilizar `helm upgrade --install`;

5. O chart incluirá boas práticas de produção:
   - readinessProbe
   - livenessProbe
   - resource limits
   - imagePullSecrets
   - suporte a escalabilidade futura (HPA opcional)

## Consequências

### Positivas

- Padronização de empacotamento;

- Facilidade de manutenção;

- Reutilização de templates;

- Clareza na separação de ambientes;

- Aderência a práticas enterprise;

- Melhor organização do repositório.

### Negativas

- Introdução de camada adicional de abstração;

- Necessidade de conhecimento em Helm;

- Leve aumento na complexidade inicial.

## Justificativa

Helm é amplamente adotado em ambientes corporativos como ferramenta padrão para gerenciamento de aplicações Kubernetes.

Sua adoção neste projeto:

- Eleva o nível arquitetural da solução;

- Demonstra maturidade técnica;

- Aproxima o laboratório de cenários reais de produção.

A decisão está alinhada com o objetivo do projeto de simular uma arquitetura DevOps enterprise realista e bem estruturada.

---

Autor: Robson Ferreira  
Projeto: arquitetura empresarial Devops autohospedada