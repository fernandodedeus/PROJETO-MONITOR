# MONITOR — Escopo do Projeto

## 1. Visão Geral

O **Monitor** será uma plataforma web voltada ao monitoramento de servidores e serviços de uma infraestrutura de TI.

A proposta é centralizar informações sobre a disponibilidade dos serviços monitorados, permitindo identificar rapidamente quando um serviço está online, offline ou apresenta algum problema.

O projeto integra duas áreas de interesse da equipe:

- **Desenvolvimento de Software / Back-end**
- **Infraestrutura de TI**

O desenvolvimento será realizado de forma incremental, começando por um MVP simples e evoluindo conforme as necessidades do projeto e o tempo disponível.

---

## 2. Problema

Em uma infraestrutura de TI, diversos servidores e serviços precisam permanecer disponíveis. Quando não existe uma forma centralizada de acompanhar esses serviços, a identificação de indisponibilidades pode depender de verificações manuais.

O Monitor busca solucionar esse problema oferecendo uma visão centralizada do estado dos serviços monitorados.

---

## 3. Objetivo Geral

Desenvolver uma plataforma capaz de **monitorar e centralizar a disponibilidade de servidores e serviços**, permitindo identificar de forma simples quando determinado serviço está funcionando ou indisponível.

---

## 4. Objetivos Específicos

- Permitir o cadastro de servidores e serviços;
- Realizar verificações periódicas de disponibilidade;
- Identificar se um serviço está online ou offline;
- Registrar informações básicas das verificações;
- Apresentar os resultados em um dashboard;
- Preparar a aplicação para execução em um ambiente de infraestrutura real;
- Evoluir futuramente para o monitoramento de métricas e recursos dos servidores.

---

# 5. Escopo do MVP

A primeira versão do Monitor terá como foco o **monitoramento de disponibilidade de serviços HTTP/HTTPS**.

### O MVP deverá permitir:

1. Cadastrar um serviço;
2. Informar seu endereço;
3. Definir a porta ou endpoint a ser monitorado;
4. Definir um intervalo de verificação;
5. Realizar verificações automáticas;
6. Determinar se o serviço está online ou offline;
7. Registrar a data e hora da verificação;
8. Registrar o tempo de resposta;
9. Exibir os resultados em um dashboard.

### Exemplo de monitoramento

```text
API Principal
      ↓
Requisição HTTP/HTTPS
      ↓
Serviço respondeu?
      ↓
 ┌───────────────┐
 │               │
SIM             NÃO
 │               │
 ▼               ▼
🟢 ONLINE       🔴 OFFLINE
```

O MVP **não terá como requisito inicial** o monitoramento detalhado de CPU, memória, disco ou outros recursos do servidor.

Essas funcionalidades poderão ser adicionadas posteriormente.

---

# 6. Funcionamento Inicial

A arquitetura inicial será baseada em quatro componentes principais:

```text
┌──────────────────┐
│    Dashboard     │
│     Web App      │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│     API .NET     │
│   ASP.NET Core   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    PostgreSQL    │
└──────────────────┘

         │
         │ Verificações
         ▼

┌──────────────────┐
│ Serviços / APIs  │
│    monitorados   │
└──────────────────┘
```

O funcionamento será:

1. O usuário cadastra um serviço;
2. O serviço é armazenado no banco de dados;
3. O sistema realiza verificações periódicas;
4. A aplicação envia uma requisição para o endereço configurado;
5. A resposta é analisada;
6. O sistema determina o status do serviço;
7. A verificação é registrada;
8. O dashboard apresenta o estado atual.

---

# 7. Estados dos Serviços

Inicialmente, os serviços poderão apresentar três estados principais:

### 🟢 Online

O serviço respondeu corretamente à verificação.

### 🔴 Offline

O serviço não respondeu, apresentou erro ou ultrapassou o tempo máximo definido para resposta.

### 🟡 Atenção

Estado reservado para situações que futuramente poderão indicar comportamento anormal, como tempo de resposta elevado.

O significado exato do estado **Atenção** poderá ser definido durante a implementação.

---

# 8. Informações de um Serviço Monitorado

Cada serviço poderá possuir inicialmente:

| Informação | Descrição |
|---|---|
| ID | Identificador do serviço |
| Nome | Nome utilizado para identificação |
| Endereço | IP ou domínio |
| Porta | Porta utilizada pelo serviço |
| Endpoint | Caminho utilizado para verificação |
| Tipo | HTTP ou HTTPS |
| Intervalo | Frequência das verificações |
| Ativo | Define se o monitoramento está habilitado |

### Exemplo

```text
Nome:        API Principal
Endereço:    api.exemplo.com
Porta:       443
Endpoint:    /health
Tipo:        HTTPS
Intervalo:   30 segundos
Ativo:       Sim
```

---

# 9. Dashboard

O dashboard terá como objetivo apresentar uma visão rápida dos serviços monitorados.

Exemplo:

```text
┌─────────────────────────────────────────────┐
│                   MONITOR                   │
├─────────────────────────────────────────────┤
│                                             │
│  Serviços monitorados: 8                    │
│  Online:              6 🟢                  │
│  Offline:             2 🔴                  │
│                                             │
├─────────────────────────────────────────────┤
│ Serviço       Status       Resposta          │
│                                             │
│ API Principal 🟢 Online       42 ms         │
│ Website       🟢 Online       31 ms         │
│ API Teste     🔴 Offline       --           │
│ Database      🟢 Online       18 ms         │
└─────────────────────────────────────────────┘
```

O dashboard poderá evoluir posteriormente para apresentar gráficos, histórico e indicadores de disponibilidade.

---

# 10. Histórico

Após o funcionamento básico do monitoramento, o sistema poderá armazenar o resultado de cada verificação.

Exemplo:

```text
14:00:00 → Online  → 42 ms
14:00:30 → Online  → 38 ms
14:01:00 → Online  → 41 ms
14:01:30 → Offline
14:02:00 → Offline
14:02:30 → Online  → 52 ms
```

Esses dados poderão ser utilizados futuramente para calcular:

- Disponibilidade;
- Tempo médio de resposta;
- Períodos de indisponibilidade;
- Quantidade de falhas;
- Histórico de incidentes.

---

# 11. Arquitetura de Infraestrutura

A infraestrutura será desenvolvida de forma progressiva.

Inicialmente:

```text
┌────────────────────┐
│   Servidor Linux   │
│                    │
│      Docker        │
│    ┌──────────┐    │
│    │   API    │    │
│    └──────────┘    │
│    ┌──────────┐    │
│    │PostgreSQL│    │
│    └──────────┘    │
└────────────────────┘
          │
        Nginx
          │
       Internet
```

O responsável pela infraestrutura ficará encarregado principalmente de preparar o ambiente necessário para execução e disponibilização da aplicação.

---

# 12. Stack Inicial

## Back-end

- C#
- .NET / ASP.NET Core
- Entity Framework Core
- REST API

## Banco de Dados

- PostgreSQL

## Infraestrutura

- Linux
- Docker
- Nginx
- Git
- GitHub

## Front-end

A interface poderá ser desenvolvida inicialmente utilizando:

- HTML
- CSS
- JavaScript

A adoção de um framework front-end poderá ser avaliada posteriormente.

---

# 13. Divisão da Equipe

## Desenvolvimento / Back-end

Responsabilidades principais:

- Arquitetura da API;
- Desenvolvimento do back-end;
- Modelagem do banco;
- Regras de negócio;
- Cadastro de serviços;
- Sistema de monitoramento;
- Registro das verificações;
- Desenvolvimento do dashboard;
- Funcionalidades futuras.

## Infraestrutura

Responsabilidades principais:

- Configuração de servidores;
- Linux;
- Redes;
- Docker;
- Nginx;
- Deploy;
- Configuração dos ambientes;
- Manutenção da infraestrutura;
- Automação e CI/CD futuramente.

---

# 14. Plano de Desenvolvimento

## Fase 1 — Planejamento

- [ ] Definir problema
- [ ] Definir objetivo
- [ ] Definir público-alvo
- [ ] Definir MVP
- [ ] Definir funcionalidades
- [ ] Definir tecnologias
- [ ] Criar arquitetura inicial
- [ ] Dividir responsabilidades

### Resultado esperado

Documento de escopo e arquitetura inicial definidos.

---

## Fase 2 — Back-end Básico

- [ ] Criar repositório
- [ ] Criar projeto ASP.NET Core
- [ ] Configurar PostgreSQL
- [ ] Configurar Entity Framework Core
- [ ] Criar entidades
- [ ] Criar migrations
- [ ] Criar estrutura da API
- [ ] Criar CRUD de serviços

### Resultado esperado

API capaz de cadastrar, consultar, alterar e excluir serviços monitorados.

---

## Fase 3 — Sistema de Monitoramento

- [ ] Criar mecanismo de verificação
- [ ] Implementar requisições HTTP/HTTPS
- [ ] Definir timeout
- [ ] Determinar status Online/Offline
- [ ] Registrar tempo de resposta
- [ ] Registrar data e hora da verificação
- [ ] Implementar verificações periódicas

### Resultado esperado

O sistema consegue verificar automaticamente se um serviço está disponível.

---

## Fase 4 — Dashboard

- [ ] Criar interface inicial
- [ ] Listar serviços monitorados
- [ ] Exibir status
- [ ] Exibir tempo de resposta
- [ ] Exibir última verificação
- [ ] Exibir quantidade de serviços online/offline

### Resultado esperado

Dashboard funcional para visualização do estado da infraestrutura.

---

## Fase 5 — Infraestrutura

- [ ] Preparar servidor Linux
- [ ] Configurar Docker
- [ ] Criar Dockerfile
- [ ] Containerizar aplicação
- [ ] Configurar PostgreSQL
- [ ] Configurar Nginx
- [ ] Realizar deploy
- [ ] Documentar ambiente

### Resultado esperado

Aplicação executando em um ambiente de infraestrutura preparado pela equipe.

---

## Fase 6 — Histórico e Indicadores

- [ ] Armazenar verificações
- [ ] Criar histórico
- [ ] Calcular disponibilidade
- [ ] Calcular tempo médio de resposta
- [ ] Criar gráficos
- [ ] Registrar períodos de indisponibilidade

### Resultado esperado

O Monitor passa a apresentar não apenas o estado atual, mas também informações históricas.

---

## Fase 7 — Alertas

- [ ] Detectar indisponibilidade
- [ ] Criar registros de alerta
- [ ] Exibir alertas no dashboard
- [ ] Definir regras de alerta
- [ ] Avaliar notificações externas

### Resultado esperado

O sistema consegue informar quando um serviço apresenta problemas.

---

# 15. Evolução Futura

Após a conclusão do MVP, poderão ser avaliadas funcionalidades mais avançadas:

- Monitoramento de CPU;
- Monitoramento de memória RAM;
- Monitoramento de armazenamento;
- Monitoramento de processos;
- Monitoramento de serviços do sistema operacional;
- Agente de monitoramento;
- Autenticação;
- Controle de usuários e permissões;
- Centralização de logs;
- Notificações;
- CI/CD;
- Monitoramento de containers;
- Monitoramento de múltiplos ambientes.

Essas funcionalidades são consideradas **possíveis evoluções** e não fazem parte do escopo obrigatório inicial.

---

# 16. Agente de Monitoramento

O agente de monitoramento será considerado uma evolução do projeto.

Ele seria um pequeno programa instalado diretamente no servidor monitorado, responsável por coletar informações do sistema operacional.

Exemplo:

```text
Servidor
    │
    ├── CPU → 42%
    ├── RAM → 65%
    ├── Disco → 51%
    └── Serviços → Online
            │
            ▼
        AGENTE
            │
            ▼
       API Monitor
```

O agente poderá permitir que o Monitor acompanhe informações que não são obtidas apenas através de uma requisição HTTP/HTTPS.

A implementação do agente será avaliada após a conclusão do MVP.

---

# 17. Critério de Sucesso do MVP

O MVP será considerado funcional quando for possível:

1. Cadastrar um serviço;
2. Configurar seus dados de monitoramento;
3. Iniciar o monitoramento;
4. Realizar verificações automáticas;
5. Identificar corretamente se o serviço está online ou offline;
6. Registrar o resultado das verificações;
7. Exibir o status no dashboard;
8. Executar a aplicação em um ambiente preparado pela equipe.

---

# 18. Princípio de Desenvolvimento

O Monitor será desenvolvido de forma **incremental**.

A equipe deverá priorizar o funcionamento do núcleo do sistema antes de adicionar funcionalidades avançadas.

A ordem de prioridade será:

```text
MVP
 ↓
Monitoramento
 ↓
Dashboard
 ↓
Infraestrutura
 ↓
Histórico
 ↓
Alertas
 ↓
Funcionalidades avançadas
```

O objetivo é evitar que o projeto tenha um escopo excessivamente grande antes que sua funcionalidade principal esteja concluída.
