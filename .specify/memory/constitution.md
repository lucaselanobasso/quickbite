# QuickBite Constitution

QuickBite é um sistema de pedidos de comida deliberadamente pequeno, construído como
laboratório de estudo de QA e Engenharia de Software. O objetivo do projeto **não é o
produto** — é ser um sistema completo, realista e observável o suficiente para praticar
automação de testes, performance, segurança, redes, infraestrutura e arquitetura.

## Core Principles

### I. Simplicidade é requisito, não preferência (NÃO-NEGOCIÁVEL)

Toda funcionalidade precisa justificar sua existência pelo que ensina, não pelo que
entrega ao usuário final. Antes de adicionar qualquer coisa, responda: *qual conceito
novo isso ensina?* Se a resposta repetir um conceito já coberto, a funcionalidade é
rejeitada.

Limites rígidos do domínio: no máximo 8 tabelas, 3 serviços, 5 telas. Um pedido pertence
a exatamente um restaurante. Não haverá geolocalização, mapas, cupons, avaliações,
chat ou app de entregador.

### II. O sistema está sempre inteiro

Ao final de cada etapa o sistema sobe com um comando e o fluxo principal funciona
de ponta a ponta. Nunca existe uma etapa "pela metade" mergeada na `main`. Se uma etapa
não couber inteira, ela é dividida — não entregue incompleta.

### III. Testes provam comportamento, não implementação

A pirâmide de testes segue o modelo moderno (Testing Trophy): muitos testes de
integração com dependências reais via Testcontainers, contratos verificados por
contract testing, e E2E restrito aos fluxos críticos de negócio.

Regras invioláveis:
- Nenhum `sleep` fixo em teste. Espera é sempre por condição observável.
- Nenhum mock de banco de dados ou de broker. Usa-se a dependência real em container.
- Teste flaky é bug de prioridade máxima — quarentena imediata e correção, nunca retry cego.
- Todo bug corrigido nasce de um teste que falha antes da correção.

### IV. Configuração vive no ambiente, segredo vive em cofre

O projeto segue os 12 fatores. Nenhum valor específico de ambiente é hardcoded, nenhum
segredo entra no repositório em qualquer momento da história do git. `.env` é ignorado;
`.env.example` é obrigatório e sempre atualizado. A mesma imagem de container roda em
qualquer ambiente, mudando apenas a configuração injetada.

### V. Observabilidade é ferramenta de QA

Todo serviço emite logs estruturados em JSON, traces OpenTelemetry e métricas. Toda
requisição carrega um correlation ID que atravessa os serviços. Quando um teste falha,
deve ser possível ir do teste ao trace da requisição sem adivinhação.

### VI. Segurança acontece no pull request

Análise estática (SAST), varredura de dependências e imagens, e detecção de segredos
rodam automaticamente em todo PR. Autorização é verificada por teste automatizado em
todo endpoint que acessa dado de outro usuário — IDOR é a falha que este projeto
existe para ensinar a prevenir.

## Restrições Técnicas

- **Frontend**: React + TypeScript (Vite)
- **API**: Node.js + Fastify + TypeScript
- **Worker de pagamentos**: Python + FastAPI
- **Dados**: PostgreSQL (estado), Redis (cache/lock), RabbitMQ (mensageria)
- **Testes**: Vitest, Testcontainers, Pact, Playwright, k6
- **Infra local**: Docker Compose → k3d (Kubernetes) → Terraform
- **CI**: GitHub Actions

Trocar qualquer item desta lista exige emenda a esta constitution.

## Fluxo de Trabalho

O desenvolvimento acontece em etapas numeradas. Cada etapa é uma feature do Spec Kit
e segue obrigatoriamente o ciclo `/speckit-specify` → `/speckit-plan` → `/speckit-tasks`
→ `/speckit-implement`, em uma branch própria, fechada por pull request.

A `main` é protegida. Nenhum merge ocorre com CI vermelho. Cada PR responde, na
descrição, ao que a etapa ensinou.

Idioma: documentação, specs e discussão em português; código, nomes de identificadores
e mensagens de commit em inglês.

## Governance

Esta constitution prevalece sobre qualquer outra prática do projeto. Emendas exigem
registro explícito neste arquivo com incremento de versão.

Complexidade precisa ser justificada por escrito: qualquer desvio dos limites do
Princípio I deve ser documentado no plano da etapa, com a alternativa mais simples
que foi descartada e o motivo.

**Version**: 1.0.0 | **Ratified**: 2026-08-26 | **Last Amended**: 2026-08-26
