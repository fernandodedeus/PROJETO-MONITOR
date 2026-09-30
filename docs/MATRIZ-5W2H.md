# 📋 Matriz 5W2H — Projeto Monitor

> Análise estruturada do projeto utilizando a metodologia 5W2H, elaborada a partir da documentação existente.

---

## Fontes Consultadas

| Documento | Descrição |
|---|---|
| [README.md](../README.md) | Visão geral, personas, requisitos funcionais e não funcionais, casos de uso |
| [ESCOPO-PROJETO.md](./ESCOPO-PROJETO.md) | Escopo completo, MVP, stack, fases de desenvolvimento, critérios de sucesso |
| [ESTRUTURA-PASTAS.md](./ESTRUTURA-PASTAS.md) | Organização de diretórios e mapeamento escopo → estrutura |
| Documentacao_Inicial_Monitor.pdf | Documentação acadêmica inicial do projeto |

---

## Matriz 5W2H

### 1. WHAT — O que será feito?

| Aspecto | Descrição |
|---|---|
| **Produto** | **Monitor** — Plataforma web de monitoramento de infraestrutura de TI |
| **Função principal** | Centralizar e automatizar a verificação de disponibilidade de servidores e serviços |
| **Escopo do MVP** | Monitoramento de disponibilidade de serviços HTTP/HTTPS com dashboard centralizado |
| **Funcionalidades do MVP** | Cadastro de serviços (CRUD), verificações automáticas periódicas, classificação de status (Online/Offline/Atenção), registro de tempo de resposta, dashboard de visualização |
| **Evolução futura** | Histórico de checagens, alertas, monitoramento de CPU/RAM/disco, agente de monitoramento, autenticação, CI/CD |

#### Requisitos Funcionais (RF)

| ID | Requisito |
|---|---|
| RF01 | Cadastro de servidores (CRUD com nome, IP/domínio, porta, intervalo de checagem) |
| RF02 | Verificação periódica de disponibilidade via requisições de rede |
| RF03 | Classificação de status: Online 🟢, Offline 🔴, Atenção 🟡 |
| RF04 | Dashboard centralizado com tabela de status, serviço, última verificação |
| RF05 | Histórico de checagens com registros de disponibilidade e tempo de resposta |

#### Requisitos Não Funcionais (RNF)

| ID | Requisito |
|---|---|
| RNF01 | Checagens em segundo plano sem degradar a interface web |
| RNF02 | Persistência segura em banco de dados relacional |
| RNF03 | Interface limpa, intuitiva, com identificação visual por cores |
| RNF04 | Arquitetura modular — API REST desacoplada do front-end |

---

### 2. WHY — Por que será feito?

| Aspecto | Descrição |
|---|---|
| **Problema identificado** | Falta de visibilidade centralizada sobre a disponibilidade e o consumo de recursos em múltiplos servidores |
| **Dor do usuário** | Descobrir indisponibilidades apenas após reclamações de clientes; perder tempo checando servidores individualmente; dificuldade de visualizar métricas de saúde em um único local |
| **Valor gerado** | Substituição de rotinas manuais e descentralizadas de monitoramento; centralização do status de múltiplos servidores em um único painel; rápida tomada de decisão ao identificar falhas antes que afetem os clientes finais |
| **Contexto acadêmico** | Projeto A3 que integra duas áreas de interesse: Desenvolvimento de Software/Back-end e Infraestrutura de TI |

---

### 3. WHERE — Onde será feito?

| Aspecto | Descrição |
|---|---|
| **Ambiente de desenvolvimento** | Repositório GitHub (`ProjetoMonitor`), desenvolvimento local com ferramentas de cada membro |
| **Ambiente de execução** | Servidor Linux com Docker, Nginx como proxy reverso e PostgreSQL |
| **Acesso ao sistema** | Via navegador web (aplicação web acessível pela internet) |
| **Arquitetura de deploy** | Containers Docker em servidor Linux, expostos através de Nginx |
| **Estrutura do código** | `src/backend/` (API .NET), `src/frontend/` (HTML/CSS/JS), `infra/` (Docker, Nginx, scripts) |

#### Arquitetura da Infraestrutura

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

---

### 4. WHEN — Quando será feito?

| Fase | Escopo | Resultado Esperado |
|---|---|---|
| **Fase 1 — Planejamento** | Definição de problema, objetivo, público-alvo, MVP, tecnologias, arquitetura e responsabilidades | Documento de escopo e arquitetura inicial definidos |
| **Fase 2 — Back-end Básico** | Repositório, projeto ASP.NET Core, PostgreSQL, EF Core, entidades, migrations, CRUD de serviços | API capaz de cadastrar, consultar, alterar e excluir serviços |
| **Fase 3 — Sistema de Monitoramento** | Mecanismo de verificação HTTP/HTTPS, timeout, status Online/Offline, tempo de resposta, verificações periódicas | Sistema de verificação automática de disponibilidade |
| **Fase 4 — Dashboard** | Interface web, listagem de serviços, exibição de status/tempo de resposta/última verificação | Dashboard funcional para visualização do estado da infraestrutura |
| **Fase 5 — Infraestrutura** | Servidor Linux, Docker, Dockerfile, Nginx, deploy, documentação do ambiente | Aplicação executando em ambiente de infraestrutura real |
| **Fase 6 — Histórico e Indicadores** | Armazenamento de verificações, cálculo de disponibilidade, tempo médio, gráficos | Informações históricas além do estado atual |
| **Fase 7 — Alertas** | Detecção de indisponibilidade, registros de alerta, regras, notificações externas | Sistema de notificação de problemas |

> **Princípio:** Desenvolvimento incremental — priorizar o núcleo funcional antes de funcionalidades avançadas.

---

### 5. WHO — Quem fará?

| Papel | Responsabilidades |
|---|---|
| **Equipe de Desenvolvimento / Back-end** | Arquitetura da API, desenvolvimento back-end, modelagem do banco, regras de negócio, CRUD de serviços, sistema de monitoramento, registro das verificações, desenvolvimento do dashboard, funcionalidades futuras |
| **Equipe de Infraestrutura** | Configuração de servidores Linux, redes, Docker, Nginx, deploy, configuração dos ambientes, manutenção da infraestrutura, automação e CI/CD futuramente |

#### Stakeholders

| Stakeholder | Descrição |
|---|---|
| **Público-alvo principal** | Empresas de infraestrutura e provedores de hospedagem/venda de servidores |
| **Público-alvo secundário** | Empresas que mantêm infraestruturas locais ou em nuvem com serviços críticos |
| **Usuários diretos** | Equipes de TI, suporte técnico, administradores de sistemas (SysAdmins) |

#### Persona de Referência

> **Carlos Eduardo**, 35 anos — Gestor de Infraestrutura e TI em empresa de hospedagem de servidores.
> Precisa manter alta taxa de disponibilidade (uptime) e ter um painel consolidado para saber instantaneamente quais máquinas estão operando normalmente.

---

### 6. HOW — Como será feito?

| Aspecto | Descrição |
|---|---|
| **Metodologia** | Desenvolvimento incremental — MVP primeiro, evoluções posteriores |
| **Arquitetura** | API REST desacoplada do front-end (arquitetura modular) |
| **Back-end** | C# com ASP.NET Core, Entity Framework Core, REST API |
| **Banco de dados** | PostgreSQL |
| **Front-end** | HTML, CSS, JavaScript (framework avaliado posteriormente) |
| **Infraestrutura** | Linux, Docker, Nginx, Git/GitHub |
| **Mecanismo de monitoramento** | Requisições HTTP/HTTPS periódicas aos serviços cadastrados → análise de resposta → classificação de status → registro em banco |
| **Versionamento** | Git com repositório no GitHub |

#### Fluxo de Monitoramento

```text
Usuário cadastra serviço
        ↓
Serviço armazenado no banco
        ↓
Verificações periódicas automáticas
        ↓
Requisição HTTP/HTTPS ao endereço configurado
        ↓
Análise da resposta
        ↓
Classificação: 🟢 Online / 🔴 Offline / 🟡 Atenção
        ↓
Registro da verificação (data/hora + tempo de resposta)
        ↓
Dashboard atualizado
```

#### Estrutura do Projeto

| Componente | Localização | Fase |
|---|---|---|
| API .NET / ASP.NET Core | `src/backend/Monitor.API/` | Fase 2 |
| CRUD de serviços | `Controllers/` + `Services/` | Fase 2 |
| Entidades (Serviço, Verificação) | `Models/` | Fase 2 |
| Entity Framework Core / PostgreSQL | `Data/` + `Migrations/` | Fase 2 |
| Sistema de monitoramento | `Services/` | Fase 3 |
| Dashboard | `src/frontend/` | Fase 4 |
| Docker / Nginx / Deploy | `infra/` | Fase 5 |

---

### 7. HOW MUCH — Quanto custará?

| Aspecto | Descrição |
|---|---|
| **Custo financeiro direto** | Não especificado na documentação; projeto acadêmico (A3) sem investimento financeiro declarado |
| **Infraestrutura** | Servidor Linux (custo de hospedagem a ser definido), PostgreSQL (open-source, sem custo de licença) |
| **Tecnologias** | Stack inteiramente baseada em tecnologias open-source e gratuitas: .NET, PostgreSQL, Docker, Nginx, Linux |
| **Recursos humanos** | Equipe dividida em duas frentes: Desenvolvimento/Back-end e Infraestrutura |
| **Esforço estimado** | 7 fases de desenvolvimento — do planejamento à implementação de alertas |
| **Critério de sucesso do MVP** | Cadastrar serviço → Configurar monitoramento → Verificar automaticamente → Identificar status → Registrar resultado → Exibir no dashboard → Executar em ambiente preparado |

---

## Matriz Resumida

| Dimensão | Resumo |
|---|---|
| **What** (O quê) | Plataforma web de monitoramento de disponibilidade de servidores e serviços de TI |
| **Why** (Por quê) | Eliminar a falta de visibilidade centralizada, substituir verificações manuais e reduzir o tempo de identificação de falhas |
| **Where** (Onde) | Aplicação web hospedada em servidor Linux com Docker/Nginx, acessível via navegador |
| **When** (Quando) | Desenvolvimento em 7 fases incrementais — do planejamento ao sistema de alertas |
| **Who** (Quem) | Equipe de Desenvolvimento (back-end + dashboard) e Equipe de Infraestrutura (servidores + deploy) |
| **How** (Como) | API REST em C#/.NET + Front-end HTML/CSS/JS + PostgreSQL + Docker, com verificações HTTP/HTTPS periódicas |
| **How Much** (Quanto) | Stack open-source sem custos de licença; esforço de 7 fases; custo de hospedagem a definir |

---

## 📝 Metodologia da Análise

### O que foi levado em consideração

A construção desta Matriz 5W2H foi realizada através de uma **análise cruzada** dos quatro documentos-fonte do projeto. Abaixo, o detalhamento do que foi considerado em cada etapa da análise:

#### 1. Cruzamento de informações entre documentos

Os documentos possuem informações complementares e, em alguns casos, sobrepostas. A análise priorizou:

- **README.md** como fonte principal para **problema, personas, requisitos funcionais/não funcionais e casos de uso**. Este documento fornece a visão mais estruturada do ponto de vista acadêmico (Etapas 1 e 2 do projeto A3), com persona detalhada, jornada do usuário e requisitos formais codificados (RF01–RF05, RNF01–RNF04).

- **ESCOPO-PROJETO.md** como fonte principal para **escopo do MVP, fases de desenvolvimento, stack tecnológica, arquitetura e critérios de sucesso**. Este é o documento mais abrangente (527 linhas), cobrindo desde a visão geral até a evolução futura, incluindo detalhamento técnico da arquitetura, fluxos de monitoramento e o plano incremental de 7 fases.

- **ESTRUTURA-PASTAS.md** como fonte para **organização técnica e mapeamento escopo → implementação**. Este documento permitiu validar a coerência entre o que foi planejado no escopo e como a estrutura do código foi organizada para suportá-lo.

- **Documentacao_Inicial_Monitor.pdf** como referência acadêmica complementar, validando o contexto do projeto como trabalho A3.

#### 2. Decisões de preenchimento

- **WHAT**: Optou-se por separar claramente o escopo do MVP das funcionalidades futuras, pois o documento de escopo enfatiza repetidamente a abordagem incremental e a distinção entre o que é obrigatório e o que é evolução.

- **WHY**: Foram extraídas tanto as dores do usuário (vindas da persona no README) quanto o valor de negócio (visão geral do escopo), conectando o problema técnico ao impacto nos clientes finais.

- **WHERE**: Combinou-se a arquitetura de infraestrutura descrita no escopo com a estrutura de diretórios documentada, fornecendo uma visão completa de "onde" — tanto no sentido de deploy quanto de organização do código-fonte.

- **WHEN**: Utilizou-se integralmente o plano de 7 fases do escopo, pois é a única fonte com cronograma estruturado. Não há datas específicas nos documentos — o "quando" foi mapeado como sequência de fases.

- **WHO**: A equipe é genérica nos documentos (sem nomes individuais além da persona fictícia). A divisão foi documentada conforme descrita — duas frentes de trabalho com responsabilidades listadas.

- **HOW**: Combinou-se a stack tecnológica, a arquitetura e o fluxo de monitoramento de diferentes seções do escopo para montar uma visão completa do "como".

- **HOW MUCH**: Este é o item com menor cobertura nas fontes. Não há orçamento, estimativa de horas ou custos definidos. A análise registrou o que pôde ser inferido: stack gratuita, esforço em 7 fases, critérios de sucesso como medida de "custo" em termos de entrega.

#### 3. Lacunas identificadas

Durante a análise, as seguintes lacunas foram observadas nos documentos-fonte:

| Lacuna | Impacto na Matriz |
|---|---|
| Ausência de cronograma com datas | O "When" foi preenchido com fases, sem datas-alvo |
| Nomes dos membros da equipe não documentados | O "Who" ficou limitado a papéis genéricos |
| Sem estimativa de custos ou orçamento | O "How Much" financeiro ficou como "não especificado" |
| Framework front-end não definido | Registrado como "a ser avaliado posteriormente" |
| Definição exata do estado "Atenção" pendente | Documentado como "a ser definido durante a implementação" |

#### 4. Consistência verificada

A análise verificou a **coerência interna** entre os documentos:

- ✅ Os requisitos do README estão alinhados com o MVP do escopo
- ✅ As fases do escopo correspondem ao mapeamento de pastas na estrutura
- ✅ A stack tecnológica é consistente entre todos os documentos
- ✅ O fluxo de monitoramento descrito no escopo é coerente com os casos de uso do README
- ✅ A persona e jornada do usuário refletem o problema e a solução propostos

---

*Documento gerado em 30/09/2026 com base na documentação do projeto Monitor.*
