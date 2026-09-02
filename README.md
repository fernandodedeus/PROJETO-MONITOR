# 🖥️ Monitor - Plataforma de Monitoramento de Infraestrutura

> Projeto A3 voltado ao acompanhamento e centralização do estado de servidores e serviços de TI.

---

## 📌 ETAPA 1: Entendimento do Problema e Contexto

### Quem é o Usuário
* **Perfil:** Gestor/Administrador de TI da empresa responsável pelo gerenciamento dos servidores e pela estabilidade dos serviços[cite: 1].

### Qual Problema seu Projeto Resolve
* **Problema:** A falta de visibilidade centralizada sobre a disponibilidade e a gestão de status e consumo de recursos (como uso de CPU) em múltiplos servidores[cite: 1].
* **Solução:** Uma plataforma web que automatiza a checagem de integridade das máquinas e serviços, eliminando a necessidade de verificações manuais isoladas e reduzindo o tempo de identificação de indisponibilidades[cite: 1].

### Quem são os Interessados (Stakeholders)
* Empresas de infraestrutura e provedores de hospedagem/venda de servidores (público-alvo principal).
* Empresas em geral que mantêm infraestruturas locais ou em nuvem com serviços críticos em produção.
* Equipes de TI, suporte técnico e administradores de sistemas (SysAdmins).

### Por que seu Projeto Gera Valor
* Substitui rotinas manuais e descentralizadas de avaliação de desempenho[cite: 1].
* Centraliza o status de múltiplos servidores em um único painel (dashboard)[cite: 1].
* Permite rápida tomada de decisão ao identificar falhas de disponibilidade antes que afetem os clientes finais[cite: 1].

---

## 👥 ETAPA 2: Entendimento do Usuário e Requisitos

### Persona

* **Nome:** Carlos Eduardo, 35 anos
* **Cargo:** Gestor de Infraestrutura e TI em uma empresa de serviços e hospedagem de servidores.
* **Objetivos:** 
  * Manter alta taxa de disponibilidade (uptime) dos clientes[cite: 1].
  * Ter um painel consolidado para saber instantaneamente quais máquinas estão operando normalmente[cite: 1].
* **Dores:** 
  * Perder tempo checando servidores individualmente[cite: 1].
  * Descobrir que uma máquina ou serviço caiu apenas após reclamação de clientes[cite: 1].
  * Dificuldade em visualizar métricas de saúde dos recursos em um único local[cite: 1].

---

### Jornada do Usuário

1. **Acesso:** Carlos acessa a plataforma via navegador web[cite: 1].
2. **Cadastro:** Cadastra um novo servidor informando o nome, IP/domínio, serviço, porta e intervalo de checagem[cite: 1].
3. **Monitoramento:** O sistema realiza verificações periódicas automáticas de conectividade e disponibilidade[cite: 1].
4. **Visualização:** Carlos acompanha a saúde da frota pelo Dashboard (status: Online, Offline ou Atenção)[cite: 1].
5. **Ação:** Identificando uma falha (status *Offline*), ele atua imediatamente na causa raiz com base no tempo de resposta e no histórico registrado[cite: 1].

---

### Requisitos Funcionais (RF) e Não Funcionais (RNF)

#### Requisitos Funcionais (RF)
* **[RF01] Cadastro de Servidores:** O sistema deve permitir criar, editar, listar e remover servidores informando nome, endereço IP/domínio, porta e intervalo de checagem[cite: 1].
* **[RF02] Verificação de Disponibilidade:** O sistema deve executar requisições periódicas de rede para aferir se os servidores/serviços estão respondendo[cite: 1].
* **[RF03] Classificação de Status:** O sistema deve atualizar o estado da máquina para *Online*, *Offline* ou *Atenção*[cite: 1].
* **[RF04] Dashboard Centralizado:** A interface deve exibir uma tabela ou lista com nome do servidor, serviço associado, status atual e data/hora da última verificação.
* **[RF05] Histórico de Checagens:** O sistema deve armazenar registros básicos de disponibilidade e tempo de resposta de cada verificação.

#### Requisitos Não Funcionais (RNF)
* **[RNF01] Desempenho:** As checagens periódicas em segundo plano não devem degradar o tempo de resposta da interface web.
* **[RNF02] Confiabilidade:** O sistema deve persistir os históricos de verificação de forma segura em banco de dados relacional.
* **[RNF03] Usabilidade:** O dashboard deve ter interface limpa e intuitiva, facilitando a identificação visual rápida de status críticos (ex: cores diferenciadas).
* **[RNF04] Arquitetura Escalável:** A aplicação deve ser desenvolvida de forma modular (API REST desacoplada do Front-end).

---

### Modelagem de Casos de Uso

| ID | Caso de Uso | Ator Principal | Descrição Resumida |
| :--- | :--- | :--- | :--- |
| **UC01** | Cadastrar Servidor | Gestor de TI | Permite registrar um novo host/serviço no sistema para que entre na fila de checagens[cite: 1]. |
| **UC02** | Consultar Dashboard | Gestor de TI | Permite visualizar a visão geral de todos os servidores cadastrados e seus respectivos status em tempo real[cite: 1]. |
| **UC03** | Executar Varredura Periódica | Sistema (Automático) | O motor do backend realiza as chamadas de rede aos servidores e atualiza a base de dados[cite: 1]. |
| **UC04** | Visualizar Histórico de Disponibilidade | Gestor de TI | Permite consultar logs passados de tempo de resposta e quedas de um servidor específico. |
