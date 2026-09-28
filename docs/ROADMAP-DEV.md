# 🗺️ ROADMAP DE DESENVOLVIMENTO — Projeto Monitor

> ROADMAP prático de desenvolvimento para nossa equipe, com a ordem de implementação, dependências entre tarefas e marcos de entrega.
> Baseado no [ESCOPO-PROJETO.md](./ESCOPO-PROJETO.md), [ESTRUTURA-PASTAS.md](./ESTRUTURA-PASTAS.md) e [README.md](../README.md).

---

## 📍 Estado Atual do Projeto

| Item | Status |
|---|---|
| Documentação de escopo | ✅ Concluída |
| Estrutura de pastas | ✅ Criada (diretórios vazios) |
| Repositório Git / GitHub | ✅ Configurado |
| Código back-end | ❌ Não iniciado |
| Código front-end | ❌ Não iniciado |
| Infraestrutura (Docker/Nginx) | ❌ Não iniciado |

> A Fase 1 (Planejamento) está essencialmente concluída. O próximo passo concreto é **começar a codar**.

---

## 🧭 Estratégia Geral

```text
SPRINT 1          SPRINT 2          SPRINT 3          SPRINT 4          SPRINT 5
─────────         ─────────         ─────────         ─────────         ─────────
 Fundação       Monitoramento       Dashboard        Infra/Deploy     Histórico &
 Back-end         Automático       Front-end         Containerização    Alertas
```

A ideia central é: **construir de dentro pra fora**. Primeiro o coração do sistema (API + banco), depois o motor de monitoramento, em seguida a interface visual, depois o deploy real, e por último os indicadores avançados.

---

## 🏗️ SPRINT 1 — Fundação do Back-end

**Objetivo:** Ter a API rodando com CRUD completo de serviços e banco de dados funcionando.

**Duração estimada:** 1–2 semanas

### Por onde começar (ordem exata):

#### 1.1 — Inicializar o projeto ASP.NET Core
- [ ] Criar o projeto `webapi` dentro de `src/backend/Monitor.API/`
- [ ] Configurar o `.gitignore` adequado para .NET
- [ ] Validar que o projeto compila e roda (`dotnet run`)

#### 1.2 — Configurar o banco de dados
- [ ] Instalar o PostgreSQL localmente (ou via Docker Compose para dev)
- [ ] Adicionar pacotes NuGet: `Npgsql.EntityFrameworkCore.PostgreSQL`, `Microsoft.EntityFrameworkCore.Tools`
- [ ] Criar a connection string no `appsettings.Development.json`
- [ ] Criar o `MonitorDbContext` em `Data/`

#### 1.3 — Modelar as entidades
- [ ] Criar a entidade `Servico` em `Models/` com os campos:
  - `Id` (Guid ou int)
  - `Nome`
  - `Endereco` (IP ou domínio)
  - `Porta`
  - `Endpoint`
  - `Tipo` (HTTP / HTTPS)
  - `IntervaloSegundos`
  - `Ativo` (bool)
  - `CriadoEm`, `AtualizadoEm`
- [ ] Criar a entidade `Verificacao` em `Models/`:
  - `Id`
  - `ServicoId` (FK)
  - `Status` (Online / Offline / Atencao)
  - `TempoRespostaMs`
  - `VerificadoEm`
  - `MensagemErro` (nullable)
- [ ] Criar as configurações Fluent API em `Data/Configurations/`

#### 1.4 — Criar Migrations e aplicar
- [ ] Gerar migration inicial (`dotnet ef migrations add InitialCreate`)
- [ ] Aplicar migration (`dotnet ef database update`)

#### 1.5 — Criar DTOs
- [ ] `CriarServicoRequest`
- [ ] `AtualizarServicoRequest`
- [ ] `ServicoResponse`
- [ ] `VerificacaoResponse`

#### 1.6 — Criar o CRUD de Serviços
- [ ] Criar `IServicoService` e `ServicoService` em `Services/`
- [ ] Criar `ServicosController` em `Controllers/`
  - `POST /api/servicos` — Cadastrar
  - `GET /api/servicos` — Listar todos
  - `GET /api/servicos/{id}` — Buscar por ID
  - `PUT /api/servicos/{id}` — Atualizar
  - `DELETE /api/servicos/{id}` — Remover

### ✅ Marco da Sprint 1
> **API rodando localmente com CRUD completo. É possível cadastrar, listar, editar e deletar serviços via Postman/Insomnia.**

---

## ⚙️ SPRINT 2 — Motor de Monitoramento

**Objetivo:** O sistema realiza verificações automáticas e periódicas de disponibilidade dos serviços cadastrados.

**Duração estimada:** 1–2 semanas

**Depende de:** Sprint 1 concluída

#### 2.1 — Criar o serviço de verificação
- [ ] Criar `IMonitoramentoService` e `MonitoramentoService` em `Services/`
- [ ] Implementar a lógica de requisição HTTP/HTTPS ao endereço do serviço
- [ ] Definir timeout configurável
- [ ] Tratar respostas:
  - Resposta com sucesso (2xx) → **Online**
  - Timeout ou erro de conexão → **Offline**
  - Resposta lenta (acima de threshold) → **Atenção** (opcional nesta fase)
- [ ] Registrar tempo de resposta em milissegundos
- [ ] Salvar o resultado como `Verificacao` no banco

#### 2.2 — Implementar verificações periódicas
- [ ] Criar um `BackgroundService` (Hosted Service) que:
  - Busca todos os serviços ativos
  - Executa a verificação de cada serviço no intervalo configurado
  - Roda de forma assíncrona e não bloqueia a API
- [ ] Implementar lógica para respeitar o intervalo individual de cada serviço

#### 2.3 — Criar endpoints de consulta de status
- [ ] `GET /api/servicos/{id}/status` — Status atual (última verificação)
- [ ] `GET /api/servicos/{id}/verificacoes` — Histórico de verificações
- [ ] `GET /api/dashboard` — Resumo geral (total de serviços, online, offline)

### ✅ Marco da Sprint 2
> **O sistema monitora automaticamente os serviços cadastrados e registra resultados no banco. É possível consultar o status via API.**

---

## 🎨 SPRINT 3 — Dashboard Front-end

**Objetivo:** Interface web funcional que consome a API e exibe o estado dos serviços.

**Duração estimada:** 1–2 semanas

**Depende de:** Sprint 2 concluída (API com dados reais)

#### 3.1 — Estrutura base do front-end
- [ ] Criar `index.html` na raiz de `src/frontend/` ou em `pages/`
- [ ] Criar os arquivos CSS base em `css/` (reset, variáveis, layout)
- [ ] Criar o arquivo JS principal em `js/` (módulo de comunicação com a API)

#### 3.2 — Página do Dashboard
- [ ] Exibir contadores de resumo (total, online, offline)
- [ ] Listar serviços monitorados em uma tabela/cards com:
  - Nome do serviço
  - Status (com indicador visual colorido 🟢🔴🟡)
  - Tempo de resposta
  - Última verificação
- [ ] Atualização automática dos dados (polling a cada X segundos)

#### 3.3 — Página de cadastro de serviços
- [ ] Formulário para criar novo serviço
- [ ] Validação básica dos campos
- [ ] Feedback visual de sucesso/erro

#### 3.4 — Página de detalhes do serviço
- [ ] Exibir informações completas do serviço
- [ ] Listar histórico recente de verificações
- [ ] Botão para ativar/desativar monitoramento
- [ ] Botão para editar/excluir

#### 3.5 — Configurar CORS na API
- [ ] Habilitar CORS no back-end para permitir chamadas do front-end
- [ ] Servir arquivos estáticos (se necessário durante dev)

### ✅ Marco da Sprint 3
> **Dashboard visual funcionando. Usuário consegue ver status dos serviços, cadastrar novos serviços e visualizar detalhes, tudo pelo navegador.**

---

## 🐳 SPRINT 4 — Infraestrutura e Deploy

**Objetivo:** Aplicação containerizada e acessível em um ambiente real.

**Duração estimada:** 1–2 semanas

**Depende de:** Sprint 3 concluída (aplicação funcional de ponta a ponta)

#### 4.1 — Dockerizar a aplicação
- [ ] Criar `Dockerfile` para a API .NET em `infra/docker/`
- [ ] Criar `docker-compose.yml` com:
  - Container da API
  - Container do PostgreSQL
  - Volumes para persistência do banco
  - Rede interna entre containers
- [ ] Validar que `docker-compose up` sobe tudo corretamente

#### 4.2 — Configurar Nginx
- [ ] Criar configuração do Nginx em `infra/nginx/`
- [ ] Configurar como proxy reverso para a API
- [ ] Servir os arquivos estáticos do front-end via Nginx
- [ ] Adicionar Nginx ao `docker-compose.yml`

#### 4.3 — Preparar servidor Linux
- [ ] Provisionar servidor (VPS, máquina virtual, etc.)
- [ ] Instalar Docker e Docker Compose
- [ ] Configurar firewall e portas necessárias
- [ ] Configurar acesso SSH

#### 4.4 — Deploy
- [ ] Criar script de deploy em `infra/scripts/`
- [ ] Realizar o primeiro deploy no servidor
- [ ] Validar a aplicação acessível via browser
- [ ] Documentar o processo de deploy

### ✅ Marco da Sprint 4
> **Aplicação rodando em um servidor Linux real, acessível pela internet (ou rede local), com Docker e Nginx.**

---

## 📊 SPRINT 5 — Histórico, Indicadores e Alertas

**Objetivo:** Adicionar camada analítica ao monitoramento e sistema de alertas.

**Duração estimada:** 2–3 semanas

**Depende de:** Sprint 4 concluída (aplicação em produção)

#### 5.1 — Histórico e métricas
- [ ] Implementar cálculo de disponibilidade (% uptime)
- [ ] Implementar cálculo de tempo médio de resposta
- [ ] Identificar e registrar períodos de indisponibilidade
- [ ] Criar endpoints para consulta de métricas
  - `GET /api/servicos/{id}/metricas?periodo=24h`

#### 5.2 — Gráficos no dashboard
- [ ] Adicionar gráfico de disponibilidade ao longo do tempo
- [ ] Adicionar gráfico de tempo de resposta
- [ ] Usar uma lib JS leve (Chart.js ou similar)

#### 5.3 — Sistema de alertas
- [ ] Detectar quando serviço muda de Online → Offline
- [ ] Criar registro de alerta no banco
- [ ] Exibir alertas no dashboard (badge, notificação visual)
- [ ] Definir regras configuráveis (ex: alertar após N falhas consecutivas)

#### 5.4 — Notificações (opcional/futuro)
- [ ] Avaliar integração com e-mail ou webhook
- [ ] Implementar envio de notificação quando alerta é criado

### ✅ Marco da Sprint 5
> **Dashboard com gráficos, métricas de disponibilidade e sistema de alertas funcional.**

---

## 🔮 Pós-MVP — Evolução Futura

Estas funcionalidades **não são prioridade agora**, mas devem ser consideradas para versões futuras:

| Funcionalidade | Prioridade | Complexidade |
|---|---|---|
| Autenticação e login | 🔴 Alta | Média |
| Agente de monitoramento (CPU, RAM, disco) | 🟡 Média | Alta |
| Monitoramento de containers Docker | 🟡 Média | Média |
| CI/CD (deploy automatizado) | 🟡 Média | Média |
| Controle de usuários e permissões | 🟢 Baixa | Média |
| Centralização de logs | 🟢 Baixa | Alta |
| Monitoramento de múltiplos ambientes | 🟢 Baixa | Alta |

---

## 📋 Resumo da Timeline

```text
                        SPRINT 1          SPRINT 2          SPRINT 3          SPRINT 4          SPRINT 5
                       (1-2 sem)         (1-2 sem)         (1-2 sem)         (1-2 sem)         (2-3 sem)
                     ┌───────────┐     ┌───────────┐     ┌───────────┐     ┌───────────┐     ┌───────────┐
                     │ Fundação  │     │   Motor   │     │ Dashboard │     │  Infra &  │     │ Histórico │
                     │ Back-end  │────▶│   Monit.  │────▶│ Front-end │────▶│  Deploy   │────▶│ & Alertas │
                     │           │     │           │     │           │     │           │     │           │
                     │ • Projeto │     │ • HTTP    │     │ • HTML/   │     │ • Docker  │     │ • Métricas│
                     │ • EF Core │     │ • BackSvc │     │   CSS/JS  │     │ • Nginx   │     │ • Gráficos│
                     │ • CRUD    │     │ • Status  │     │ • Cards   │     │ • Linux   │     │ • Alertas │
                     │ • Postgres│     │ • Timeout │     │ • Forms   │     │ • Script  │     │ • Notif.  │
                     └───────────┘     └───────────┘     └───────────┘     └───────────┘     └───────────┘
                          │                 │                 │                 │                  │
                          ▼                 ▼                 ▼                 ▼                  ▼
                     API + CRUD        Monitoramento      Interface        App em             Dashboard
                      funcional         automático         visual         produção            completo
```

**Tempo total estimado: 6 a 11 semanas** (dependendo da disponibilidade da equipe)

---

## 💡 Dicas para o Início

1. **Comece pelo `dotnet new webapi`** — não perca tempo configurando ferramentas, comece a codar logo.
2. **Use Docker Compose para o PostgreSQL desde o dia 1** — evita instalar o banco na máquina e padroniza o ambiente de dev.
3. **Teste a API pelo Postman/Insomnia** antes de começar o front-end — garante que o back-end funciona isoladamente.
4. **Faça commits pequenos e frequentes** — facilita rastrear problemas e manter o histórico limpo.
5. **Não pule para o front-end antes de ter a Sprint 2 concluída** — o dashboard precisa de dados reais para fazer sentido.

---

> 📅 **Documento criado em:** 28/09/2026
> 📄 **Baseado em:** ESCOPO-PROJETO.md, ESTRUTURA-PASTAS.md, README.md
