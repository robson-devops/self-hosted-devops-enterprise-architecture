# 04 - Infrastructure Layer

## Objetivo

A camada 04-infrastructure é responsável por fornecer os serviços fundamentais
que suportam o ciclo de vida de aplicações dentro da arquitetura DevOps.

Esta camada não hospeda aplicações de negócio.
Ela sustenta o pipeline, o versionamento e o armazenamento de imagens.

---

## Componentes

### 1. GitLab CE (CI/CD Server)

- Instalado em VM Ubuntu 24 dedicada;
- Responsável por versionamento e pipelines;
- Integração com Kubernetes via Runner.

### 2. GitLab Runner

- Executando dentro do cluster Kubernetes;
- Responsável pela execução dos pipelines.

### 3. Harbor (Container Registry)

- Registry privado corporativo;
- Armazena imagens Docker versionadas;
- Integrado ao pipeline de CI.

---

## Responsabilidades da Camada

- Controle de versão;
- Automação de build;
- Armazenamento de imagens;
- Integração contínua;
- Base para entrega contínua (CD).

---

## Decisões Arquiteturais

- GitLab CE instalado em VM dedicada;
- Runner executando dentro do cluster Kubernetes;
- Registry privado (Harbor) self-hosted;
- Separação clara entre Infraestrutura e Workloads,

Ver ADR-0002 (Harbor)
Ver ADR-0003 (GitLab CI Architecture)

---

## Princípios Aplicados

- Separação de responsabilidades;
- Modularidade;
- Reprodutibilidade via script;
- Documentação via ADR;
- Arquitetura enterprise simulada.

---

## Próxima Camada

A próxima etapa da arquitetura é:

05-kubernetes

Responsável por:

- Estruturação do cluster;
- Namespaces;
- Segurança;
- Deploy das aplicações;
- Estratégia de exposição (Ingress).

---

Autor: Robson Ferreira  
Projeto: self-hosted-devops-enterprise-architecture
