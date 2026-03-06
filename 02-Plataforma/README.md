# ⚙️ Plataforma SQL Server

Incidentes relacionados ao funcionamento interno da instância SQL Server, incluindo Engine, Alta Disponibilidade e SQL Server Agent.

Este módulo cobre problemas diretamente ligados ao comportamento da engine, concorrência, replicação e automações.

---

## 📌 Escopo

- Engine SQL Server
- TempDB
- Locks e Latches
- Alta Disponibilidade (AG, Mirroring, Log Shipping, Replicação)
- SQL Server Agent

---

## 🧭 Incidentes

---

### 🧠 2.1 Engine

#### [2.1.1 Contenção de TempDB → Banco TempDB](2.1.1-contencao-tempdb.md)

**Objetivo**  
Identificar contenção de recursos internos na TempDB causados por alta concorrência.

---

#### [2.1.2 Contenção de locks/latches → Instância SQL / Banco específico](2.1.2-locks-latches.md)

**Objetivo**  
Analisar bloqueios e contenções internas que impactam concorrência.

---

### 🔁 2.2 Alta Disponibilidade

#### [2.2.1 Falha de Always On Availability Groups → Availability Group / Réplica](2.2.1-falha-alwayson.md)

**Objetivo**  
Diagnosticar falhas de sincronização ou indisponibilidade em AG.

---

#### [2.2.2 Falha de mirroring → Endpoint Mirroring](2.2.2-falha-mirroring.md)

**Objetivo**  
Validar conectividade e estado de sessão de mirroring.

---

#### [2.2.3 Falha de log shipping → Configuração Log Shipping](2.2.3-falha-logshipping.md)

**Objetivo**  
Identificar falhas em backup, copy ou restore do log shipping.

---

#### [2.2.4 Falha de replicação → Publisher / Distributor / Subscriber](2.2.4-falha-replicacao.md)

**Objetivo**  
Diagnosticar erros de sincronização entre os nós de replicação.

---

### 🛠️ 2.3 SQL Server Agent

#### [2.3.1 Falha de jobs do SQL Server Agent → Job SQL Agent](2.3.1-falha-jobs-agent.md)

**Objetivo**  
Identificar falhas de execução, histórico de erro e problemas de credenciais.

---

## 🎯 Objetivo do Módulo

Centralizar diagnósticos operacionais relacionados à plataforma SQL Server, estruturando a análise por componente da engine e serviços associados.

Este módulo complementa a camada de Infraestrutura, aprofundando na lógica interna da instância.
