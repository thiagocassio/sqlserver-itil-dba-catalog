![SQL Server](https://img.shields.io/badge/SQLServer-ITIL--Catalog-blue)

# sqlserver-itil-dba-catalog

# 📘 Catálogo ITIL para DBA SQL Server

Framework operacional para gestão de incidentes em ambientes Microsoft SQL Server, organizado por categorias técnicas e alinhado com práticas ITIL.

---

## 🔁 Modelo de Resposta a Incidentes

O catálogo está estruturado para apoiar um fluxo de resposta a incidentes em ambientes SQL Server.

```mermaid
flowchart TD

A[Detecção de Incidente] --> B[Triagem]

B --> C{Incidente confirmado?}

C -- Não --> D[Encerrar alerta]

C -- Sim --> E[Classificação de Severidade]

E --> F[Diagnóstico Técnico]

F --> G[Mitigação]

G --> H[Monitoramento]

H --> I[Pós-incidente / Análise de causa raiz]

I --> J[Atualização do Runbook / Catálogo]
