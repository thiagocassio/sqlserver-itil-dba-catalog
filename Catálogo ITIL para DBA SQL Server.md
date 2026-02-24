Catálogo ITIL para DBA SQL Server
(Categoria → Subcategoria → Item de Configuração – CI)

1. Infraestrutura
1.1 Servidor
1.1.1 CPU elevada → Servidor SQL / VM SQL / Host físico
-- DMV
Ferramenta: DMVs
Comando: sys.dm_exec_query_stats
Objetivo: identificar queries com maior consumo de CPU
SELECT TOP 10 qs.total_worker_time, st.text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) st
ORDER BY qs.total_worker_time DESC;

Ferramenta: DMVs
Comando: sys.dm_exec_requests 
Objetivo: verificar sessões ativas consumindo CPU
SELECT * FROM sys.dm_exec_requests;

Ferramenta: DMVs
Comando: sys.dm_os_wait_stats
Objetivo: analisar waits relacionados a CPU
SELECT * FROM sys.dm_os_wait_stats;

-- PerfMon
Ferramenta: Performance Monitor
Contadores: % Processor Time, Batch Requests/sec
Objetivo: validar consumo de CPU no servidor

-- Query Store
Ferramenta: Query Store
Visão: Top CPU Queries
Objetivo: acompanhar histórico de consumo de CPU

-- Script DBA
Ferramenta: sp_whoisactive
Comando: EXEC sp_whoisactive
Objetivo: visualizar sessões ativas e uso de recursos

-------------------------------------

1.1.2 Memória sob pressão → Servidor SQL / Instância SQL
-- DMV
Ferramenta: DMVs
Comando: sys.dm_os_process_memory
Objetivo: verificar uso de memória pelo SQL Server
SELECT * FROM sys.dm_os_process_memory;

Ferramenta: DMVs
Comando: sys.dm_os_memory_clerks
Objetivo: identificar quais componentes internos estão consumindo memória
SELECT * FROM sys.dm_os_memory_clerks;

Ferramenta: DMVs
Comando: sys.dm_os_sys_memory
Objetivo: analisar memória disponível no SO
SELECT * FROM sys.dm_os_sys_memory;

Ferramenta: DMVs
Comando: sys.dm_exec_query_memory_grants
Objetivo: identificar consultas aguardando memória
SELECT * FROM sys.dm_exec_query_memory_grants;

-- PerfMon
Ferramenta: Performance Monitor
Contadores: Available MBytes, Page Life Expectancy, Memory Grants Pending
Objetivo: acompanhar pressão de memória

-- Script DBA
Ferramenta: DBCC
Comando: DBCC MEMORYSTATUS
Objetivo: diagnóstico detalhado da memória
DBCC MEMORYSTATUS;

-------------------------------------

1.1.3 Exaustão de threads (THREADPOOL) → Instância SQL
-- DMV
Ferramenta: DMVs
Comando: sys.dm_os_waiting_tasks
Objetivo: verificar waits THREADPOOL
SELECT * FROM sys.dm_os_waiting_tasks WHERE wait_type = 'THREADPOOL';

Ferramenta: DMVs
Comando: sys.dm_os_schedulers
Objetivo: analisar workers disponíveis
SELECT * FROM sys.dm_os_schedulers;

Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: identificar sessões presas aguardando worker
SELECT * FROM sys.dm_exec_requests;

-- PerfMon
Ferramenta: Performance Monitor
Contadores: Worker Threads, Runnable Tasks Count
Objetivo: acompanhar esgotamento de threads

-------------------------------------

1.1.4 Excesso de conexões → Instância SQL
-- DMV
Ferramenta: DMVs
Comando: sys.dm_exec_sessions
Objetivo: listar sessões conectadas
SELECT * FROM sys.dm_exec_sessions;

Ferramenta: DMVs
Comando: sys.dm_exec_connections
Objetivo: validar conexões ativas
SELECT * FROM sys.dm_exec_connections;

Ferramenta: DMVs
Comando: sys.dm_os_performance_counters
Objetivo: acompanhar número de conexões
SELECT * FROM sys.dm_os_performance_counters;

-- PerfMon
Ferramenta: Performance Monitor
Contadores: User Connections, Logins/sec
Objetivo: monitorar crescimento de sessões

-- Script DBA
Ferramenta: sp_whoisactive
Comando: EXEC sp_whoisactive
Objetivo: identificar origem das conexões

--------------------------------------------------------------------------

1.2 Storage
1.2.1 Latência de I/O elevada → Disco Data / Disco Log / Storage Azure
-- DMV
Ferramenta: DMVs
Comando: sys.dm_io_virtual_file_stats
Objetivo: identificar arquivos com maior latência de leitura/escrita
SELECT 
    DB_NAME(database_id) AS DatabaseName,
    file_id,
    io_stall_read_ms,
    io_stall_write_ms,
    num_of_reads,
    num_of_writes
FROM sys.dm_io_virtual_file_stats(NULL,NULL);

Ferramenta: DMVs
Comando: sys.dm_os_wait_stats
Objetivo: validar waits relacionados a I/O (PAGEIOLATCH / WRITELOG)
SELECT *
FROM sys.dm_os_wait_stats
WHERE wait_type LIKE 'PAGEIOLATCH%'
   OR wait_type LIKE 'WRITELOG%';

Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: localizar operações intensivas de I/O em execução
SELECT * FROM sys.dm_exec_requests;

-- PerfMon
Ferramenta: Performance Monitor
Contadores: Disk sec/Read, Disk sec/Write, Avg Disk Queue Length
Objetivo: acompanhar latência no nível do disco

-- SSMS
Ferramenta: Activity Monitor
Visão: Data File I/O
Objetivo: visualizar gargalos de I/O em tempo real

-------------------------------------

1.2.2 Falta de espaço em disco → Volume Data / Volume Log / TempDB
-- DMV / Comandos
Ferramenta: Catálogo do SQL
Comando: sys.database_files
Objetivo: validar tamanho e crescimento dos arquivos
SELECT name, size, max_size, growth FROM sys.database_files;

Ferramenta: DBCC
Comando: DBCC SQLPERF(LOGSPACE)
Objetivo: verificar uso do Transaction Log
DBCC SQLPERF(LOGSPACE);

Ferramenta: Catálogo TempDB
Comando: tempdb.sys.database_files
Objetivo: analisar consumo do TempDB
SELECT * FROM tempdb.sys.database_files;

-- PerfMon
Ferramenta: Performance Monitor
Contadores: Free Megabytes, % Free Space
Objetivo: acompanhar espaço disponível nos volumes

-- SO / Infraestrutura
Ferramenta: Explorer / Azure Portal
Ação: verificar volumes Data, Log e TempDB
Objetivo: confirmar falta real de espaço no disco

-------------------------------------

1.2.3 Consumo anormal de storage → Storage Account / Datafiles
-- DMV
Ferramenta: sys.master_files
Comando: consulta de tamanho dos datafiles
Objetivo: identificar arquivos maiores
SELECT DB_NAME(database_id), name, size FROM sys.master_files;

Ferramenta: Catálogo de objetos
Comando: sys.dm_db_partition_stats
Objetivo: localizar tabelas com maior consumo
SELECT TOP 10
OBJECT_NAME(object_id) AS TableName,
SUM(reserved_page_count)*8/1024 AS SizeMB
FROM sys.dm_db_partition_stats
GROUP BY object_id
ORDER BY SizeMB DESC;

-- SSMS
Ferramenta: Standard Reports → Disk Usage
Objetivo: visão geral de crescimento por banco
EXEC sp_MSforeachdb '
SELECT ''?'' AS DatabaseName,
SUM(size)*8/1024 AS SizeMB
FROM ?.sys.database_files
GROUP BY database_id';

-- Azure Monitor (se aplicável)
Ferramenta: Métricas de Storage
Objetivo: acompanhar crescimento da Storage Account

-------------------------------------

1.2.4 Autogrowth inesperado → Arquivos MDF/NDF/LDF
-- DMV
Ferramenta: Catálogo do SQL
Comando: sys.database_files
Objetivo: validar configuração de autogrowth
SELECT 
    name,
    growth,
    is_percent_growth
FROM sys.database_files;

Ferramenta: DMV
Comando: sys.dm_db_log_space_usage
Objetivo: acompanhar uso do log em tempo real
SELECT * FROM sys.dm_db_log_space_usage;

Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: identificar operações gerando crescimento
SELECT * FROM sys.dm_exec_requests;

-- Extended Events
Ferramenta: Extended Events
Evento: database_file_size_change
Objetivo: capturar eventos de crescimento automático

-------------------------------------

1.3 Cluster / Sistema Operacional
1.3.1 Falha de cluster/WSFC → Cluster WSFC
-- PowerShell / SO
Ferramenta: PowerShell
Comando: Get-ClusterNode
Objetivo: verificar estado dos nós do cluster

Ferramenta: PowerShell
Comando: Get-ClusterGroup
Objetivo: validar recursos e ownership do cluster

-- SQL Server
Ferramenta: DMV
Comando: sys.dm_hadr_cluster
Objetivo: validar comunicação com o cluster WSFC
SELECT * FROM sys.dm_hadr_cluster;

Ferramenta: DMV
Comando: sys.dm_hadr_availability_replica_states
Objetivo: verificar estado das réplicas
SELECT * FROM sys.dm_hadr_availability_replica_states;

-- Failover Cluster Manager
Ferramenta: Console WSFC
Ação: verificar status do role SQL Server
Objetivo: confirmar falha de recurso ou nó

-------------------------------------

1.3.2 Indisponibilidade da instância SQL Server → Serviço MSSQLSERVER
-- Serviços do Windows
Ferramenta: Services.msc / PowerShell
Comando: Get-Service MSSQLSERVER
Objetivo: validar se o serviço está parado ou falhando ao iniciar

-- SQLCMD
Ferramenta: sqlcmd
Comando: tentativa de conexão local
Objetivo: confirmar indisponibilidade da engine
sqlcmd -S localhost -E

-- Event Viewer
Ferramenta: Event Viewer
Log: Application / System
Objetivo: identificar falhas de inicialização da instância

-------------------------------------

1.3.3 Erros críticos no error log → Instância SQL
-- SQL Server
Ferramenta: Stored Procedure
Comando: xp_readerrorlog
Objetivo: localizar mensagens críticas recentes
EXEC xp_readerrorlog; 
EXEC xp_readerrorlog 0,1,N'Error'; -- erros recentes

Ferramenta: SSMS
Ação: SQL Server Logs
Objetivo: revisar histórico de falhas

-- Extended Events
Ferramenta: Extended Events
Evento: error_reported
Objetivo: capturar erros críticos em tempo real

-- Event Viewer
Ferramenta: Event Viewer
Log: Application
Objetivo: correlacionar falhas do SQL Server com o sistema operacional

--------------------------------------------------------------------------

2. Plataforma SQL Server
2.1 Engine
2.1.1 Contenção de TempDB → Banco TempDB
-- DMV
Ferramenta: DMVs
Comando: sys.dm_db_file_space_usage
Objetivo: analisar consumo de espaço e version store no TempDB
SELECT * FROM sys.dm_db_file_space_usage;

Ferramenta: DMVs
Comando: sys.dm_os_waiting_tasks
Objetivo: identificar waits relacionados ao TempDB (PAGELATCH_*)
SELECT *
FROM sys.dm_os_waiting_tasks
WHERE wait_type LIKE 'PAGELATCH%';

Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: localizar sessões utilizando intensivamente TempDB
SELECT * FROM sys.dm_exec_requests;
EXEC sp_whoisactive;

-- PerfMon
Ferramenta: Performance Monitor
Contadores: Temp Tables Creation Rate, Version Store Size
Objetivo: acompanhar uso e pressão sobre TempDB

-- SSMS
Ferramenta: Activity Monitor
Visão: Data File I/O (TempDB)
Objetivo: visualizar contenção em arquivos TempDB

-------------------------------------

2.1.2 Contenção de locks/latches → Instância SQL / Banco específico
-- DMV
Ferramenta: DMVs
Comando: sys.dm_tran_locks
Objetivo: listar locks ativos na instância ou banco
SELECT * FROM sys.dm_tran_locks;

Ferramenta: DMVs
Comando: sys.dm_os_waiting_tasks
Objetivo: identificar sessões aguardando locks ou latches
SELECT * FROM sys.dm_os_waiting_tasks;

Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: localizar sessões bloqueadas e bloqueadoras
SELECT * FROM sys.dm_exec_requests;

-- Script DBA
Ferramenta: sp_whoisactive
Comando: EXEC sp_whoisactive @find_block_leaders = 1;
Objetivo: identificar cadeias de bloqueio

-- Extended Events
Ferramenta: Extended Events
Evento: lock_deadlock, lock_timeout
Objetivo: capturar contenção e deadlocks em tempo real

-------------------------------------

2.2 Alta Disponibilidade
2.2.1 Falha de Always On Availability Groups → Availability Group / Réplica
-- DMV
Ferramenta: DMVs
Comando: sys.dm_hadr_availability_replica_states
Objetivo: verificar estado das réplicas
SELECT * FROM sys.dm_hadr_availability_replica_states;

Ferramenta: DMVs
Comando: sys.dm_hadr_database_replica_states
Objetivo: validar sincronização dos bancos
SELECT * FROM sys.dm_hadr_database_replica_states;

Ferramenta: DMVs
Comando: sys.availability_groups
Objetivo: confirmar configuração do AG
SELECT * FROM sys.availability_groups;

-- SSMS
Ferramenta: Always On Dashboard
Ação: verificar Health / Sync State
Objetivo: visualizar status geral do Availability Group

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: identificar falhas de conexão ou sincronização
EXEC xp_readerrorlog;

-------------------------------------

2.2.2 Falha de mirroring → Endpoint Mirroring
-- DMV
Ferramenta: DMVs
Comando: sys.database_mirroring
Objetivo: verificar estado do espelhamento
SELECT * FROM sys.database_mirroring;

Ferramenta: DMVs
Comando: sys.tcp_endpoints
Objetivo: validar endpoint de mirroring
SELECT * FROM sys.tcp_endpoints;

-- SSMS
Ferramenta: Database Properties → Mirroring
Objetivo: verificar estado da sessão de mirroring

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: identificar falhas de endpoint ou conexão
EXEC xp_readerrorlog;

-------------------------------------

2.2.3 Falha de log shipping → Configuração Log Shipping
-- MSDB
Ferramenta: Tabelas MSDB
Comando: log_shipping_monitor_error_detail
Objetivo: identificar erros do log shipping
SELECT * FROM msdb.dbo.log_shipping_monitor_error_detail;

Ferramenta: Tabelas MSDB
Comando: log_shipping_monitor_history_detail
Objetivo: validar histórico de cópia e restore
SELECT * FROM msdb.dbo.log_shipping_monitor_history_detail;

-- SQL Agent
Ferramenta: Jobs do SQL Server Agent
Objetivo: verificar falhas nos jobs de backup/copy/restore

-- SSMS
Ferramenta: Log Shipping Status Report
Objetivo: visualizar atraso e estado da sincronização

-------------------------------------

2.2.4 Falha de replicação → Publisher / Distributor / Subscriber
-- Replication Monitor
Ferramenta: Replication Monitor
Objetivo: verificar status dos agentes e latência

-- MSDB / Distribution
Ferramenta: Tabelas Distribution
Comando: MSreplication_monitordata
Objetivo: validar erros e atrasos de replicação
SELECT * FROM distribution.dbo.MSreplication_monitordata;

-- SQL Agent
Ferramenta: Jobs de replicação
Objetivo: identificar falhas em Snapshot / Log Reader / Distribution Agent

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar erros críticos relacionados à replicação

-------------------------------------

2.3 SQL Server Agent
2.3.1 Falha de jobs do SQL Server Agent → Job SQL Agent
-- MSDB (Histórico de Jobs)
Ferramenta: Tabelas MSDB
Comando: msdb.dbo.sysjobhistory
Objetivo: identificar falhas e códigos de erro do job
SELECT * FROM msdb.dbo.sysjobhistory ORDER BY run_date DESC, run_time DESC;

Ferramenta: Tabelas MSDB
Comando: msdb.dbo.sysjobs
Objetivo: validar status e configuração do job
SELECT * FROM msdb.dbo.sysjobs;

Ferramenta: Tabelas MSDB
Comando: msdb.dbo.sysjobactivity
Objetivo: verificar execução atual e estado do job
SELECT * FROM msdb.dbo.sysjobactivity;
EXEC xp_readerrorlog;

-- SSMS
Ferramenta: SQL Server Agent → Job Activity Monitor
Ação: visualizar status e histórico do job
Objetivo: identificar falha rapidamente pela interface

-- SQL Server Agent Service
Ferramenta: Services.msc / PowerShell
Comando: Get-Service SQLSERVERAGENT
Objetivo: confirmar se o serviço do Agent está ativo

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: identificar erros relacionados à execução do job

-- Extended Events
Ferramenta: Extended Events
Evento: sqlserver.agent_job_failure
Objetivo: capturar falhas de execução em tempo real

--------------------------------------------------------------------------

3. Dados
3.1 Disponibilidade
-- DMV / Catálogo
Ferramenta: Catálogo do SQL
Comando: sys.databases
Objetivo: verificar estado do banco (ONLINE, OFFLINE, SUSPECT, RECOVERY_PENDING)
SELECT name, state_desc FROM sys.databases;

Ferramenta: DMV
Comando: sys.dm_exec_requests
Objetivo: validar sessões tentando acessar o banco
SELECT * FROM sys.dm_exec_requests;

-- SSMS
Ferramenta: Object Explorer
Ação: verificar status do database
Objetivo: identificar indisponibilidade rapidamente

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar mensagens críticas relacionadas ao database
EXEC xp_readerrorlog;

-- Event Viewer
Ferramenta: Event Viewer → Application
Objetivo: correlacionar falhas do SQL Server com o sistema operacional

-------------------------------------

3.1.1 Indisponibilidade do banco de dados → Database (DB específico)
-- DMV / Catálogo
Ferramenta: Catálogo do SQL
Comando: sys.databases
Objetivo: verificar estado do database (ONLINE, OFFLINE, SUSPECT, RECOVERY_PENDING)
SELECT name, state_desc FROM sys.databases;

Ferramenta: DMV
Comando: sys.dm_exec_requests
Objetivo: identificar sessões tentando acessar o database
SELECT * FROM sys.dm_exec_requests;
EXEC sp_whoisactive;

-- SSMS
Ferramenta: Object Explorer
Ação: validar status visual do database
Objetivo: identificar indisponibilidade rapidamente

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar mensagens críticas relacionadas ao database
EXEC xp_readerrorlog;

-- Event Viewer
Ferramenta: Event Viewer → Application
Objetivo: correlacionar falhas do banco com eventos do SO

-------------------------------------

3.2 Integridade
3.2.1 Corrupção de banco de dados → Database
-- DBCC / Integridade
Ferramenta: DBCC
Comando: DBCC CHECKDB
Objetivo: validar integridade lógica e física do database
DBCC CHECKDB('NomeDoBanco') WITH NO_INFOMSGS, ALL_ERRORMSGS;

Ferramenta: DBCC
Comando: DBCC CHECKTABLE
Objetivo: identificar corrupção em objetos específicos
DBCC CHECKTABLE('NomeDaTabela') WITH NO_INFOMSGS, ALL_ERRORMSGS;

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar mensagens de corrupção (824, 825, 823)
EXEC xp_readerrorlog;

-- DMV / Catálogo
Ferramenta: sys.databases
Objetivo: verificar estado do banco após erro de corrupção
SELECT name, state_desc FROM sys.databases;

Ferramenta: sys.dm_exec_requests
Objetivo: identificar processos impactados
SELECT * FROM sys.dm_exec_requests;

-- SSMS
Ferramenta: SQL Server Logs
Ação: revisar mensagens críticas
Objetivo: confirmar início e origem da corrupção

-------------------------------------

3.2.2 Corrupção de índices → Tabela / Índice
-- DBCC / Integridade
Ferramenta: DBCC
Comando: DBCC CHECKTABLE
Objetivo: validar índice/tabela específica
DBCC CHECKTABLE('NomeDaTabela') WITH NO_INFOMSGS, ALL_ERRORMSGS;

Ferramenta: DBCC
Comando: DBCC CHECKDB
Objetivo: validar integridade geral do database e detectar índices corrompidos
DBCC CHECKDB('NomeDoBanco') WITH NO_INFOMSGS, ALL_ERRORMSGS;

-- Catálogo do SQL Server
Ferramenta: sys.indexes
Objetivo: identificar índices associados à tabela afetada
SELECT * FROM sys.indexes WHERE object_id = OBJECT_ID('NomeDaTabela');

Ferramenta: sys.objects
Objetivo: confirmar objeto impactado pela corrupção

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar mensagens relacionadas a corrupção de páginas ou índices (823, 824, 825)
EXEC xp_readerrorlog;

-- SSMS
Ferramenta: SQL Server Logs
Ação: revisar mensagens críticas
Objetivo: confirmar falhas relacionadas ao índice

-------------------------------------

3.3 Transaction Log
3.3.1 Crescimento anormal do Transaction Log → Arquivo LDF
-- DBCC / DMV
Ferramenta: DBCC
Comando: DBCC SQLPERF(LOGSPACE)
Objetivo: verificar percentual de uso do log
DBCC SQLPERF(LOGSPACE);

Ferramenta: DMV
Comando: sys.dm_db_log_space_usage
Objetivo: analisar espaço utilizado e disponível no LDF
SELECT *
FROM sys.dm_db_log_space_usage;

Ferramenta: DMV
Comando: sys.databases (log_reuse_wait_desc)
Objetivo: identificar motivo da não reutilização do log
SELECT name, log_reuse_wait_desc FROM sys.databases;

-- Sessões e Transações
Ferramenta: DMV
Comando: sys.dm_tran_active_transactions
Objetivo: localizar transações abertas
SELECT * FROM sys.dm_tran_active_transactions;

Ferramenta: DMV
Comando: sys.dm_exec_requests
Objetivo: identificar operações que geram crescimento do log
SELECT * FROM sys.dm_exec_requests;

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar eventos de crescimento automático do log

-- SSMS
Ferramenta: Activity Monitor → Data File I/O
Objetivo: acompanhar escrita intensa no log

-------------------------------------

3.4 Backup e Recuperação
3.4.1 Falha de backup → Plano de Backup / Job Backup
-- MSDB / Histórico de Backup
Ferramenta: Tabelas MSDB
Comando: msdb.dbo.backupset
Objetivo: validar últimos backups executados
SELECT TOP 10
    database_name,
    backup_finish_date,
    type
FROM msdb.dbo.backupset
ORDER BY backup_finish_date DESC;

Ferramenta: Tabelas MSDB
Comando: msdb.dbo.backupmediafamily
Objetivo: verificar destino do backup

-- SQL Server Agent
Ferramenta: msdb.dbo.sysjobhistory
Objetivo: identificar falha no job de backup
SELECT * FROM msdb.dbo.sysjobhistory ORDER BY run_date DESC, run_time DESC;

Ferramenta: msdb.dbo.sysjobactivity
Objetivo: verificar execução atual do job
SELECT * FROM msdb.dbo.sysjobactivity;

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar erros relacionados a BACKUP DATABASE / BACKUP LOG
EXEC xp_readerrorlog;

-- SSMS
Ferramenta: Job Activity Monitor
Objetivo: validar falha visualmente no job de backup

-------------------------------------

3.4.2 Falha de restore → Database / Arquivo Backup
-- MSDB / Histórico de Restore
Ferramenta: Tabelas MSDB
Comando: msdb.dbo.restorehistory
Objetivo: identificar tentativas de restore e falhas recentes
SELECT * FROM msdb.dbo.restorehistory ORDER BY restore_date DESC;

Ferramenta: Tabelas MSDB
Comando: msdb.dbo.backupset
Objetivo: validar cadeia de backups utilizada no restore

-- Validação do Arquivo de Backup
Ferramenta: RESTORE VERIFYONLY
Objetivo: validar integridade do arquivo de backup
RESTORE VERIFYONLY 
FROM DISK = 'C:\Backup\Arquivo.bak';

Ferramenta: RESTORE HEADERONLY
Objetivo: validar integridade do arquivo de backup
RESTORE HEADERONLY 
FROM DISK = 'C:\Backup\Arquivo.bak';

Ferramenta: RESTORE FILELISTONLY
Objetivo: validar integridade do arquivo de backup
RESTORE FILELISTONLY
FROM DISK = 'C:\Backup\Arquivo.bak';

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar erros durante a execução do restore
EXEC xp_readerrorlog;

-- SSMS
Ferramenta: Restore Database Wizard
Objetivo: validar parâmetros e histórico do restore

--------------------------------------------------------------------------

4. Performance e Concorrência
4.1 Performance
-- DMVs — Execução e Consumo
Ferramenta: DMVs
Comando: sys.dm_exec_query_stats
Objetivo: identificar consultas com maior consumo de CPU/tempo
EXEC sp_whoisactive;

SELECT TOP 10 qs.total_worker_time, st.text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) st
ORDER BY qs.total_worker_time DESC;

Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: verificar sessões em execução e waits atuais
EXEC sp_whoisactive;
SELECT * FROM sys.dm_exec_requests;

Ferramenta: DMVs
Comando: sys.dm_os_wait_stats
Objetivo: analisar waits predominantes relacionados à performance
SELECT * FROM sys.dm_os_wait_stats;

-- Query Store
Ferramenta: Query Store
Ação: Top Resource Consuming Queries
Objetivo: avaliar degradação histórica de performance

-- SSMS
Ferramenta: Activity Monitor
Visão: Processes / Resource Waits
Objetivo: identificar gargalos rapidamente

-- PerfMon
Ferramenta: Performance Monitor
Contadores: Batch Requests/sec, Page Life Expectancy
Objetivo: validar comportamento do servidor durante a degradação

-------------------------------------

4.1.1 Degradação severa de desempenho → Instância SQL / Database
-- DMVs — Execução e Sessões
Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: identificar sessões com waits e alto tempo de execução
EXEC sp_whoisactive;
SELECT * FROM sys.dm_exec_requests;

Ferramenta: DMVs
Comando: sys.dm_exec_query_stats
Objetivo: localizar consultas com maior consumo acumulado
SELECT TOP 10 qs.total_worker_time, st.text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) st
ORDER BY qs.total_worker_time DESC;

-- Waits e Recursos
Ferramenta: DMVs
Comando: sys.dm_os_wait_stats
Objetivo: identificar gargalo predominante (CPU, I/O, Locks, Memory)
SELECT * FROM sys.dm_os_wait_stats;

Ferramenta: DMVs
Comando: sys.dm_os_waiting_tasks
Objetivo: verificar sessões aguardando recursos
SELECT * FROM sys.dm_os_waiting_tasks;

-- Query Store
Ferramenta: Query Store
Ação: Top Resource Consuming Queries
Objetivo: comparar performance atual com histórico

-- SSMS
Ferramenta: Activity Monitor
Objetivo: identificar rapidamente processos e waits críticos

-- Script DBA
Ferramenta: sp_whoisactive
Objetivo: visão consolidada de sessões, waits e bloqueios

-------------------------------------

4.2 Bloqueios
4.2.1 Bloqueios prolongados → Sessão SQL / Database
-- DMVs — Locks e Sessões
Ferramenta: DMVs
Comando: sys.dm_tran_locks
Objetivo: identificar locks ativos na instância ou database
SELECT * FROM sys.dm_tran_locks;

Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: localizar sessões bloqueadas e bloqueadoras
SELECT * FROM sys.dm_exec_requests;

Ferramenta: DMVs
Comando: sys.dm_os_waiting_tasks
Objetivo: identificar sessões aguardando recursos (LCK_*)
SELECT * FROM sys.dm_os_waiting_tasks;

-- Script DBA
Ferramenta: sp_whoisactive
Comando: EXEC sp_whoisactive @find_block_leaders = 1;
Objetivo: identificar cadeia de bloqueios
EXEC sp_whoisactive @find_block_leaders = 1;

-- Extended Events
Ferramenta: Extended Events
Evento: blocked_process_report
Objetivo: capturar bloqueios prolongados em tempo real

-- SSMS
Ferramenta: Activity Monitor
Visão: Processes
Objetivo: visualizar sessões bloqueadas rapidamente

-------------------------------------

4.2.2 Deadlocks → Database / Stored Procedure
-- DMVs — Sessões e Execução
Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: identificar sessões envolvidas no momento da contenção
SELECT * FROM sys.dm_exec_requests;

Ferramenta: DMVs
Comando: sys.dm_os_waiting_tasks
Objetivo: verificar waits relacionados a bloqueios (LCK_*)
SELECT * FROM sys.dm_os_waiting_tasks WHERE wait_type LIKE 'LCK_%';

-- Extended Events
Ferramenta: Extended Events
Evento: xml_deadlock_report
Objetivo: capturar gráfico XML do deadlock e identificar objetos envolvidos

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar registros de deadlock capturados no log
EXEC xp_readerrorlog;

-- SSMS
Ferramenta: Activity Monitor → Resource Waits
Objetivo: identificar sintomas de deadlock ou contenção intensa

-- Script DBA
Ferramenta: sp_whoisactive
Comando: EXEC sp_whoisactive @get_locks = 1;
Objetivo: visualizar locks ativos no momento da análise
EXEC sp_whoisactive @get_locks = 1;



--------------------------------------------------------------------------
5. Operação e Segurança
5.1 Manutenção
5.1.1 Falha em manutenção (rebuild/reorg/update stats) → Plano de Manutenção / Job
-- DMVs — Execução
Ferramenta: DMVs
Comando: sys.dm_exec_requests
Objetivo: verificar se manutenção está travada ou aguardando recursos
SELECT * FROM sys.dm_exec_requests;

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar erros durante rebuild, reorg ou update stats
EXEC xp_readerrorlog;

-- SQL Server Agent / MSDB
Ferramenta: Tabelas MSDB
Comando: msdb.dbo.sysjobhistory
Objetivo: identificar falhas nos jobs de manutenção
SELECT * FROM msdb.dbo.sysjobhistory ORDER BY run_date DESC, run_time DESC;

Ferramenta: Tabelas MSDB
Comando: msdb.dbo.sysjobactivity
Objetivo: validar execução atual do plano de manutenção
SELECT * FROM msdb.dbo.sysjobactivity;
SELECT * FROM msdb.dbo.sysjobs;

-- SSMS
Ferramenta: Maintenance Plan Designer
Ação: validar histórico e configuração do plano
Objetivo: identificar falha visualmente

-------------------------------------

5.1.2 Falha em ETL/carga crítica → Pacote ETL / Job SQL
-- SQL Server Agent / MSDB
Ferramenta: Tabelas MSDB
Comando: msdb.dbo.sysjobhistory
Objetivo: identificar falhas nos jobs de ETL ou carga
SELECT * FROM msdb.dbo.sysjobhistory ORDER BY run_date DESC, run_time DESC;

Ferramenta: Tabelas MSDB
Comando: msdb.dbo.sysjobactivity
Objetivo: verificar execução atual do job ETL
SELECT * FROM msdb.dbo.sysjobactivity;
SELECT * FROM msdb.dbo.sysjobs;

-- SSIS / ETL (quando aplicável)
Ferramenta: SSISDB Catalog
Comando: catalog.executions
Objetivo: validar status das execuções do pacote
SELECT * FROM catalog.executions ORDER BY start_time DESC;

Ferramenta: SSISDB Catalog
Comando: catalog.event_messages
Objetivo: identificar erro específico no ETL
SELECT * FROM catalog.event_messages;

-- DMVs — Execução
Ferramenta: sys.dm_exec_requests
Objetivo: verificar sessões relacionadas à carga crítica
SELECT * FROM sys.dm_exec_requests;

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar falhas de execução ou conexão durante ETL
EXEC xp_readerrorlog;

-- SSMS
Ferramenta: Job Activity Monitor
Objetivo: validar falha visualmente no job ETL

-------------------------------------

5.2 Acesso
5.2.1 Falha de conexão/autenticação → Login SQL / AD
-- Logins e Segurança
Ferramenta: Catálogo do SQL
Comando: sys.server_principals
Objetivo: validar existência e estado do login
SELECT name, type_desc, is_disabled FROM sys.server_principals;

Ferramenta: Catálogo do SQL
Comando: sys.sql_logins
Objetivo: verificar propriedades do login SQL
SELECT * FROM sys.sql_logins;

-- Sessões e Conexões
Ferramenta: DMV
Comando: sys.dm_exec_connections
Objetivo: identificar tentativas de conexão ativas
SELECT * FROM sys.dm_exec_connections;

Ferramenta: DMV
Comando: sys.dm_exec_sessions
Objetivo: validar sessões autenticadas
SELECT * FROM sys.dm_exec_sessions;

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar erros de login (18456)
EXEC xp_readerrorlog;

-- SSMS
Ferramenta: Security → Logins
Objetivo: validar status visual do login

-- Sistema Operacional / AD
Ferramenta: Event Viewer → Security
Objetivo: correlacionar falhas de autenticação do Windows/AD

-------------------------------------

5.2.2 Falha de permissões/acesso a objetos críticos → Database / Schema
-- Permissões no Database
Ferramenta: Catálogo do SQL
Comando: sys.database_permissions
Objetivo: verificar permissões concedidas ou negadas no database
SELECT * FROM sys.database_permissions;

Ferramenta: Catálogo do SQL
Comando: sys.database_principals
Objetivo: validar usuários e roles no database
SELECT * FROM sys.database_principals;

-- Objetos e Schema
Ferramenta: Catálogo do SQL
Comando: sys.objects
Objetivo: confirmar existência do objeto crítico
SELECT * FROM sys.objects WHERE name = 'NomeDoObjeto';

Ferramenta: Catálogo do SQL
Comando: sys.schemas
Objetivo: validar schema associado ao objeto
SELECT * FROM sys.schemas;

-- Sessões e Execução
Ferramenta: DMV
Comando: sys.dm_exec_requests
Objetivo: identificar sessões com erro de acesso
SELECT * FROM sys.dm_exec_requests;

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar erros de permissão (ex: 229, 916)
EXEC xp_readerrorlog;

-- SSMS
Ferramenta: Security → Users / Roles
Objetivo: validar permissões visualmente

-------------------------------------
5.3 Criptografia
5.3.1 Falha de certificados/criptografia (TDE/SSL) → Certificado / Endpoint / Database
-- Certificados e Criptografia
Ferramenta: Catálogo do SQL
Comando: sys.certificates
Objetivo: validar existência e validade do certificado
SELECT name, expiry_date FROM sys.certificates;

Ferramenta: Catálogo do SQL
Comando: sys.dm_database_encryption_keys
Objetivo: verificar estado da criptografia TDE
SELECT * FROM sys.dm_database_encryption_keys;

-- Endpoints / Conexões
Ferramenta: Catálogo do SQL
Comando: sys.endpoints
Objetivo: validar configuração de endpoints criptografados
SELECT * FROM sys.endpoints;

Ferramenta: DMV
Comando: sys.dm_exec_connections
Objetivo: verificar uso de criptografia SSL nas conexões
SELECT * FROM sys.dm_exec_connections;

-- Error Log
Ferramenta: xp_readerrorlog
Objetivo: localizar erros relacionados a certificado, TDE ou SSL
EXEC xp_readerrorlog;

-- SSMS
Ferramenta: Database Properties → Encryption
Objetivo: validar estado do TDE visualmente

-- Sistema Operacional
Ferramenta: Certificate Manager / Event Viewer
Objetivo: verificar validade e acesso ao certificado no SO
