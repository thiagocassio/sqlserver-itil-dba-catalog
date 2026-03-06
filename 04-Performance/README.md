# ⚡ Performance e Concorrência

Incidentes relacionados à degradação de desempenho, contenção de recursos e concorrência em ambientes SQL Server.

Este módulo aborda situações em que o banco continua disponível, porém com impacto significativo na performance ou bloqueio de operações.

---

## 📌 Escopo

- Degradação de performance
- Bloqueios prolongados
- Deadlocks
- Concorrência entre sessões
- Contenção de recursos

---

## 🧭 Incidentes

---

### ⚡ 4.1 Performance

#### [4.1.1 Degradação severa de desempenho → Instância SQL / Database](4.1.1-degradacao-performance.md)

**Objetivo**  
Identificar consultas, waits e recursos responsáveis pela degradação de desempenho.

---

### 🔒 4.2 Bloqueios

#### [4.2.1 Bloqueios prolongados → Sessão SQL / Database](4.2.1-bloqueios-prolongados.md)

**Objetivo**  
Diagnosticar cadeias de bloqueio que impedem execução de queries.

---

#### [4.2.2 Deadlocks → Database / Stored Procedure](4.2.2-deadlocks.md)

**Objetivo**  
Identificar conflitos entre transações que resultam em deadlock.

---

## 🎯 Objetivo do Módulo

Estruturar diagnósticos relacionados a performance e concorrência, permitindo identificar rapidamente consultas problemáticas, contenção de recursos e conflitos entre sessões.
