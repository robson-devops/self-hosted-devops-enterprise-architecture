# ADR-0001 – Estratégia de Cluster Kubernetes

**Status:** Aprovado

## Contexto

A arquitetura da plataforma DevOps precisava definir a estratégia de isolamento entre ambientes (DEV, HML e PROD).

As opções consideradas foram:

- Clusters separados por ambiente;

- Cluster único com segregação por namespaces;

- Clusters efêmeros por feature.

O objetivo do projeto é manter arquitetura limpa, moderna, executável e com boa relação entre complexidade e valor.

## Decisão

Foi adotado:

- Cluster único Kubernetes com segregação lógica via namespaces (dev, hml, prod).

## Justificativa

- Reduz complexidade operacional;

- Diminui custo de infraestrutura;

- Permite governança centralizada;

- Mantém separação lógica adequada;

- É prática comum em empresas de médio porte;

- Facilita observabilidade unificada.

## Consequências

**Positivas:**

- Arquitetura simples e sustentável;

- Fácil manutenção;

- Menor overhead operacional.

**Negativas:**

- Isolamento físico não é absoluto;

- Requer políticas de RBAC e NetworkPolicy bem definidas.