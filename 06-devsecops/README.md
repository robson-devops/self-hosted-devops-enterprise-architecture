# 06 - DevSecOps Layer

## Objetivo

A camada 06-devsecops é responsável por implementar segurança contínua em todo o ciclo de entrega da arquitetura DevOps.

Esta camada materializa o conceito de Shift-Left Security, garantindo que código, dependências, containers e workloads sejam validados antes, durante e após o deploy no cluster Kubernetes.

O objetivo não é apenas escanear vulnerabilidades, mas criar uma esteira de segurança automatizada, auditável e compatível com padrões modernos de Supply Chain Security (2026).

---

## Componentes

**1. Secret Scanning (Gitleaks)**

- Detecção automática de credenciais expostas no repositório;

- Bloqueio imediato do pipeline em caso de vazamento;

- Prevenção contra exposição de tokens, chaves privadas e senhas.

**2. SAST (Semgrep)**

- Análise estática do código-fonte;

- Identificação de padrões inseguros;

- Detecção de vulnerabilidades antes do build;

---

## Integração direta com pipeline CI.

**3. SCA – Software Composition Analysis (Trivy FS)**

- Scan de dependências e bibliotecas;

- Identificação de CVEs críticos e HIGH;

- Bloqueio de artefatos vulneráveis;

- Base de dados CVE atualizada automaticamente.

**4. Container Image Scan (Trivy Image)**

- Análise da imagem Docker final;

- Verificação de pacotes do sistema operacional;

- Bloqueio de imagens com vulnerabilidades críticas;

- Integração com registry privado.

**5. Image Signing (Cosign / Sigstore)**

- Assinatura criptográfica das imagens;

- Verificação obrigatória antes do deploy;

- Garantia de integridade e origem confiável;

- Proteção contra ataques de Supply Chain.

**6. DAST (OWASP ZAP)**

- Teste dinâmico da aplicação em execução;

- Identificação de falhas como:

    - XSS

    - Headers inseguros

    - Configurações incorretas

- Geração de relatório automatizado.

**7. Policy as Code (Kyverno)**

- Enforcement de políticas no cluster Kubernetes;

- Bloqueio de uso da tag latest;

- Obrigatoriedade de resource limits;

- Execução obrigatória como non-root;

- Verificação de imagens assinadas.

---

## Responsabilidades da Camada

- Implementar segurança no pipeline CI/CD;

- Bloquear vulnerabilidades antes do deploy;

- Garantir integridade de imagens containerizadas;

- Aplicar políticas de segurança em runtime;

- Garantir rastreabilidade e compliance;

- Proteger contra ataques de Supply Chain;

- Integrar segurança à arquitetura Kubernetes (camada 05).

---

## Decisões Arquiteturais

- Segurança implementada como código (Security as Code);

- Bloqueio automático de CVEs HIGH/CRITICAL;

- Assinatura obrigatória de imagens antes do deploy;

- Enforcement de políticas via Kyverno no cluster;

- Separação entre segurança de build e segurança de runtime;

- Integração direta com pipeline GitLab CI;

- Compatibilidade com padrões SLSA e Supply Chain Security.

Ver ADR-0006 (DevSecOps Strategy)  
Ver ADR-0007 (Container Image Signing Strategy)  

---

## Princípios Aplicados

- Shift-Left Security;

- Automação total da segurança;

- Fail Fast (pipeline bloqueia em falhas críticas);

- Imutabilidade de artefatos;

- Segurança em múltiplas camadas;

- Policy as Code;

- Arquitetura enterprise simulada.

---

## Fluxo de Segurança Implementado

Git Push  
→ Secret Scan (Gitleaks)  
→ SAST (Semgrep)  
→ SCA (Trivy FS)  
→ Build da Imagem  
→ Container Scan (Trivy Image)  
→ Image Signing (Cosign)  
→ Image Verification    
→ Deploy (Camada 05)    
→ Policy Enforcement (Kyverno)  
→ DAST (ZAP)    

---

## Integração com a Camada 05

A camada 06 atua diretamente sobre a 05-kubernetes:

- Imagens assinadas são exigidas antes do deploy;

- Policies Kyverno impedem workloads inseguros;

- CVEs críticos bloqueiam atualização via Helm;

- Segurança aplicada tanto no pipeline quanto no cluster.

Isso cria uma arquitetura com defesa em profundidade.

---

## Estrutura Organizacional do Diretório

A estrutura da camada 06 foi organizada por responsabilidade de segurança:

**secrets/** → Secret scanning  
**sast/** → Análise estática    
**sca/** → Dependências 
**container-scan/** → Imagem Docker 
**image-signing/** → Assinatura e verificação   
**dast/** → Testes dinâmicos    
**policies/kyverno/** → Policy as Code  
**ci/** → Orquestração do pipeline  

Essa organização reforça separação de responsabilidades e clareza arquitetural.

---

# Nível de Maturidade Alcançado

Com esta camada implementada, a arquitetura passa a possuir:

- Segurança automatizada ponta a ponta;

- Proteção contra vazamento de credenciais;

- Controle de vulnerabilidades antes do deploy;

- Garantia de integridade de imagens;

- Enforcement contínuo em runtime;

- Estrutura compatível com ambientes corporativos modernos.

---

## Próxima Evolução Possível

A arquitetura pode evoluir para:

07-platform-engineering

Com foco em:

- Observabilidade avançada;

- Golden Paths para desenvolvedores;

- Internal Developer Platform;

- GitOps completo com ArgoCD;

- Runtime Security (Falco);

- SBOM automático e attestation SLSA.

---

Autor: Robson Ferreira
Projeto: self-hosted-devops-enterprise-architecture