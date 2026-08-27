# QuickBite

Sistema de pedidos de comida construído como **laboratório de estudo de QA e Engenharia
de Software**. Pequeno de propósito, completo de propósito.

O fluxo é um só:

```
Cliente vê o cardápio → faz o pedido → pagamento assíncrono confirma
→ restaurante aceita → PREPARING → OUT_FOR_DELIVERY → DELIVERED
```

Cada item do cardápio tem estoque diário limitado. Isso não é um detalhe de produto —
é o coração do laboratório: é o que produz *race conditions* reais sob carga.

## Trilha de estudo

| # | Etapa | Conceitos |
|---|-------|-----------|
| 0 | Fundação | Spec Kit, Docker Compose, healthchecks, variáveis de ambiente |
| 1 | Domínio + API + Postgres | Modelagem, migrations, testes unitários, Testcontainers |
| 2 | Frontend + E2E | React, Playwright determinístico, trace viewer |
| 3 | Fila + worker Python | Assincronia, idempotência, DLQ, contract testing (Pact) |
| 4 | Segurança | JWT, autorização, IDOR, OWASP Top 10, SAST/DAST |
| 5 | Performance | k6, caça à race condition, p95/p99, thresholds no CI |
| 6 | Observabilidade | OpenTelemetry, Grafana, Prometheus, Loki, Tempo |
| 7 | Redes e resiliência | Traefik, TLS, Toxiproxy, rate limit, retry, circuit breaker |
| 8 | Configuração e segredos | 12-factor, Docker secrets, HashiCorp Vault |
| 9 | Kubernetes | k3d, manifests, Helm, probes, Secrets |
| 10 | Infraestrutura como código | Terraform, state, módulos |

## Arquitetura

| Componente | Tecnologia |
|------------|-----------|
| Frontend | React + TypeScript (Vite) |
| API | Node.js + Fastify + TypeScript |
| Worker de pagamentos | Python + FastAPI |
| Banco de dados | PostgreSQL |
| Cache e locks | Redis |
| Mensageria | RabbitMQ |

## Como rodar

> Disponível a partir da Etapa 1.

```bash
cp .env.example .env
docker compose up -d
```

## Como o projeto é desenvolvido

Cada etapa segue o ciclo do [Spec Kit](https://github.com/github/spec-kit):
`/speckit-specify` → `/speckit-plan` → `/speckit-tasks` → `/speckit-implement`,
em branch própria e fechada por pull request.

As regras do projeto vivem em [`.specify/memory/constitution.md`](.specify/memory/constitution.md).
