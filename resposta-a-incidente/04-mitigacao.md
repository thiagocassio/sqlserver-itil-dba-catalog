# 🛠️ 04 – Mitigação de Incidente

**Categoria:** Resposta a Incidente
**Escopo:** Ações para estabilizar o ambiente SQL Server durante um incidente  
**Objetivo:** Reduzir o impacto do incidente e restaurar a operação normal do ambiente.

---

## 🎯 Objetivo

A etapa de mitigação consiste em aplicar ações técnicas para reduzir ou eliminar o impacto do incidente identificado durante o diagnóstico.

Nem sempre a mitigação resolve definitivamente a causa raiz do problema, mas tem como objetivo **restaurar a estabilidade do ambiente o mais rápido possível**.

---

## ⚡ Tipos Comuns de Mitigação

Dependendo do tipo de incidente, diferentes ações podem ser aplicadas.

Exemplos comuns em ambientes SQL Server:

- encerramento de sessões problemáticas
- reinício de jobs falhos
- liberação de bloqueios
- ajuste temporário de recursos
- execução de manutenção corretiva
- correção de configuração

Essas ações devem sempre considerar o impacto na aplicação e nos usuários.

---

## 🔧 Mitigações Comuns em SQL Server

Alguns exemplos de ações frequentemente utilizadas por DBAs:

### 🔹 Encerramento de Sessão

Quando uma sessão está causando bloqueios ou consumo excessivo de recursos, pode ser necessário encerrá-la.

```sql
KILL <session_id>
