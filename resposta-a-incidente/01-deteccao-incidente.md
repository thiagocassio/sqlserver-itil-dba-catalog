# 🚨 01 – Detecção de Incidente

**Categoria:** Resposta a Incidente  
**Escopo:** Identificação inicial de problemas em ambientes SQL Server  
**Objetivo:** Detectar rapidamente sinais de incidentes que possam impactar disponibilidade, desempenho ou integridade do ambiente.

---

## 🎯 Objetivo

A detecção de incidente é a primeira etapa do processo de resposta.  
Seu objetivo é identificar sinais de comportamento anormal no ambiente SQL Server antes que o impacto ao negócio aumente.

A detecção pode ocorrer por meio de monitoramento automatizado, alertas ou reportes de usuários.

---

## 🔎 Fontes de Detecção

Um incidente pode ser detectado por diferentes fontes.

### 🔹 Monitoramento Automatizado

Ferramentas de monitoramento podem gerar alertas automáticos.

Exemplos:

- CPU elevada
- Uso excessivo de memória
- Latência de I/O
- Crescimento anormal de transaction log
- Falha de jobs

Ferramentas comuns:

- SQL Server Agent Alerts
- Extended Events
- PerfMon
- Azure Monitor
- Sistemas de observabilidade

---

### 🔹 Alertas do SQL Server

O SQL Server pode gerar alertas para eventos críticos.

Exemplos:

- Error 823 (I/O error)
- Error 824 (logical consistency error)
- Error 825 (read retry)

Esses erros geralmente indicam possíveis problemas de storage.

---

### 🔹 Logs do Sistema

Análise de logs pode revelar incidentes em andamento.

Principais fontes:

- SQL Server Error Log
- Windows Event Viewer
- Logs de aplicação

---

### 🔹 Monitoramento de Performance

Alguns incidentes são detectados através de métricas anormais.

Exemplos:

- aumento de waits
- queries consumindo CPU
- bloqueios prolongados
- deadlocks frequentes

---

### 🔹 Reporte de Usuários

Em muitos casos, o incidente é detectado por usuários ou sistemas da aplicação.

Exemplos:

- aplicação lenta
- erro de conexão
- consultas travadas
- falha em relatórios

Esses reportes devem ser tratados como **potenciais incidentes** até análise técnica.

---

## 🧩 Tipos Comuns de Incidentes Detectados

A detecção inicial pode revelar diferentes tipos de problemas.

Exemplos:

- indisponibilidade de banco de dados
- degradação de performance
- bloqueios entre sessões
- falha de backup
- falha de login
- corrupção de banco
- falha em jobs de manutenção
- crescimento anormal de transaction log

Esses incidentes são detalhados no **Incident Catalog** do projeto.

---

## 🛠️ Ferramentas Comuns de Detecção

Ferramentas frequentemente utilizadas por DBAs:

- SQL Server Management Studio (SSMS)
- sp_whoisactive
- sys.dm_exec_requests
- sys.dm_os_wait_stats
- SQL Server Agent Alerts
- Monitoramento de infraestrutura

Essas ferramentas ajudam a identificar rapidamente sinais de incidentes.

---

## 🔁 Próximo Passo no Processo

Após a detecção, o incidente deve seguir para a etapa de **Triagem**.

Objetivos da triagem:

- confirmar se é realmente um incidente
- identificar impacto
- definir prioridade
- direcionar investigação

---

## 📌 Observação Operacional

Detecção rápida reduz significativamente o impacto de incidentes.

Ambientes maduros utilizam:

- monitoramento proativo
- alertas automáticos
- observabilidade contínua

para identificar problemas antes que afetem o negócio.
