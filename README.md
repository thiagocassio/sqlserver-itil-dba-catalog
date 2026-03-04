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
