# 🖥️ Infraestrutura

Incidentes relacionados à camada de servidor, sistema operacional e storage em ambientes SQL Server.

---

## 📌 Escopo

- Servidor SQL Server
- Sistema Operacional
- Storage / Disco
- Cluster / WSFC

---

## 🧭 Incidentes

---

### 🔥 1.1 Servidor

#### [1.1.1 CPU elevada → Servidor SQL / VM SQL / Host físico](1.1.1-cpu-elevada.md)

**Ferramentas**
- sys.dm_exec_requests
- sys.dm_exec_query_stats
- sys.dm_os_wait_stats
- sp_whoisactive *(opcional)*

**Objetivo**
Identificar sessões com alto consumo de CPU e consultas dominantes.

---

#### [1.1.2 Memória sob pressão → Servidor SQL / Instância SQL](1.1.2-memoria-sob-pressao.md)

**Ferramentas**
- sys.dm_os_process_memory
- sys.dm_exec_query_memory_grants
- sys.dm_os_memory_clerks
- sys.dm_os_sys_memory

**Objetivo**
Analisar consumo interno e disponibilidade de memória no SO.

---

#### [1.1.3 Exaustão de threads (THREADPOOL) → Instância SQL](1.1.3-threadpool.md)

**Ferramentas**
- sys.dm_os_waiting_tasks
- sys.dm_exec_requests

**Objetivo**
Identificar waits THREADPOOL e sessões concorrentes.

---

#### [1.1.4 Excesso de conexões → Instância SQL](1.1.4-excesso-conexoes.md)

**Ferramentas**
- sys.dm_exec_sessions
- sys.dm_exec_connections
- sys.dm_os_performance_counters

**Objetivo**
Avaliar volume de sessões ativas e impacto na instância.

---

### 💽 1.2 Storage

#### [1.2.1 Latência de I/O elevada → Disco Data / Disco Log](1.2.1-latencia-io.md)

**Ferramentas**
- sys.dm_io_virtual_file_stats
- sys.dm_os_wait_stats
- sys.dm_exec_requests

**Objetivo**
Detectar gargalos de leitura/escrita.

---

#### [1.2.2 Falta de espaço em disco → Volume Data / Log / TempDB](1.2.2-falta-espaco.md)

**Ferramentas**
- sys.database_files
- DBCC SQLPERF(LOGSPACE)
- tempdb.sys.database_files

**Objetivo**
Validar crescimento de arquivos e consumo de log.

---

#### [1.2.3 Consumo anormal de storage → Datafiles](1.2.3-consumo-storage.md)

**Ferramentas**
- sys.dm_db_partition_stats
- sys.master_files

**Objetivo**
Identificar objetos com maior ocupação.

---

#### [1.2.4 Autogrowth inesperado → MDF/NDF/LDF](1.2.4-autogrowth.md)

**Ferramentas**
- sys.database_files
- sys.dm_db_log_space_usage

**Objetivo**
Analisar crescimento automático de arquivos.

---

### 🧱 1.3 Cluster / SO

#### [1.3.1 Falha de cluster / WSFC](1.3.1-falha-cluster.md)

**Ferramentas**
- Get-ClusterNode
- Get-ClusterGroup
- sys.dm_hadr_cluster

**Objetivo**
Validar estado dos nós e grupos do cluster.

---

#### [1.3.2 Indisponibilidade da instância SQL Server](1.3.2-indisponibilidade-instancia.md)

**Ferramentas**
- Get-Service MSSQLSERVER
- Event Viewer
- sqlcmd

**Objetivo**
Confirmar status do serviço SQL Server.

---

#### [1.3.3 Erros críticos no error log](1.3.3-erros-errorlog.md)

**Ferramentas**
- xp_readerrorlog
- Extended Events

**Objetivo**
Identificar falhas críticas registradas pelo SQL Server.
