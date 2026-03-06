# 🧠 03 – Diagnóstico de Incidente

**Categoria:** Resposta a Incidente
**Escopo:** Investigação técnica do incidente em ambientes SQL Server  
**Objetivo:** Identificar a causa do problema utilizando métricas, logs e ferramentas de diagnóstico do SQL Server.

---

## 🎯 Objetivo

A etapa de diagnóstico tem como finalidade identificar a causa raiz do incidente.

Nesta fase são analisados:

- métricas de desempenho
- sessões ativas
- waits do SQL Server
- queries em execução
- logs e mensagens de erro

O diagnóstico deve permitir compreender o comportamento do ambiente no momento do incidente.

---

## 🔎 Análise Inicial

O diagnóstico normalmente começa verificando o estado atual da instância SQL Server.

Alguns pontos comuns de verificação:

- sessões em execução
- queries com alto consumo de CPU
- bloqueios entre sessões
- waits predominantes
- consumo de memória

Ferramentas como **SQL Server Management Studio (SSMS)** são frequentemente utilizadas nessa etapa.

---

## 🧰 Ferramentas Comuns de Diagnóstico

Algumas ferramentas e consultas são frequentemente utilizadas por DBAs para investigação.

Exemplos:

- `sp_whoisactive`
- `sys.dm_exec_requests`
- `sys.dm_exec_query_stats`
- `sys.dm_os_wait_stats`
- `sys.dm_exec_sessions`

Essas ferramentas permitem identificar sessões problemáticas e consultas que estão impactando o ambiente.

---

## 📊 Análise de Waits

Wait statistics ajudam a identificar o tipo de contenção que está ocorrendo no ambiente.

Exemplos comuns:

- PAGEIOLATCH → espera por leitura de disco
- LCK_M_* → bloqueios
- CXPACKET → paralelismo
- SOS_SCHEDULER_YIELD → pressão de CPU

A análise de waits é uma das formas mais eficazes de identificar gargalos no SQL Server.

---

## 🔍 Análise de Queries

Durante o diagnóstico também é importante identificar consultas com alto consumo de recursos.

Alguns sintomas comuns:

- alto consumo de CPU
- alto número de leituras lógicas
- consultas com tempo de execução elevado

Ferramentas como Query Store ou consultas nas DMVs ajudam a identificar queries problemáticas.

---

## 📄 Análise de Logs

Logs podem fornecer informações importantes sobre falhas no ambiente.

Principais fontes:

- SQL Server Error Log
- Windows Event Viewer
- logs da aplicação

Esses logs podem indicar erros de I/O, falhas de autenticação ou outros problemas relevantes.

---

## 🔁 Próximo Passo no Processo

Após identificar a causa do incidente, o processo segue para a etapa de **Mitigação**.

Objetivos da mitigação:

- reduzir o impacto do incidente
- restaurar a operação normal do ambiente
- aplicar correções temporárias ou definitivas

---

## 📌 Observação Operacional

O objetivo do diagnóstico não é apenas identificar o problema imediato, mas também reunir informações suficientes para análise de causa raiz na etapa pós-incidente.
