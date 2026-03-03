# 💾 Dados

Incidentes relacionados à disponibilidade, integridade e proteção dos bancos de dados em ambientes SQL Server.

Este módulo aborda falhas que impactam diretamente os dados armazenados, incluindo indisponibilidade, corrupção, crescimento de transaction log e processos de backup e recuperação.

---

## 📌 Escopo

- Disponibilidade de banco de dados
- Integridade estrutural
- Transaction Log (LDF)
- Backup e Restore

---

## 🧭 Incidentes

---

### 🟢 3.1 Disponibilidade

#### [3.1.1 Indisponibilidade do banco de dados → Database específico](3.1.1-indisponibilidade-database.md)

**Objetivo**  
Diagnosticar bancos em estado OFFLINE, SUSPECT, RECOVERY_PENDING ou inacessíveis.

---

### 🛡️ 3.2 Integridade

#### [3.2.1 Corrupção de banco de dados → Database](3.2.1-corrupcao-database.md)

**Objetivo**  
Identificar inconsistências estruturais utilizando DBCC CHECKDB e análise do Error Log.

---

#### [3.2.2 Corrupção de índices → Tabela / Índice](3.2.2-corrupcao-indices.md)

**Objetivo**  
Diagnosticar corrupção isolada de índices e avaliar rebuild ou recreate.

---

### 📈 3.3 Transaction Log

#### [3.3.1 Crescimento anormal do Transaction Log → Arquivo LDF](3.3.1-crescimento-log.md)

**Objetivo**  
Identificar crescimento excessivo do log e falhas de truncamento.

---

### 💽 3.4 Backup e Recuperação

#### [3.4.1 Falha de backup → Plano de Backup / Job Backup](3.4.1-falha-backup.md)

**Objetivo**  
Diagnosticar falhas em rotinas de backup e validar integridade dos arquivos gerados.

---

#### [3.4.2 Falha de restore → Database / Arquivo Backup](3.4.2-falha-restore.md)

**Objetivo**  
Identificar erros durante o processo de restauração e validar a cadeia de backup.

---

## 🎯 Objetivo do Módulo

Organizar diagnósticos relacionados diretamente à proteção, integridade e recuperação dos dados, garantindo continuidade operacional e redução de risco em ambientes SQL Server.
