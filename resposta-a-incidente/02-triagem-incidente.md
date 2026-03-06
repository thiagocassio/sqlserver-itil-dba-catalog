# 🔎 02 – Triagem de Incidente

**Categoria:** Resposta a Incidente  
**Escopo:** Classificação inicial e validação do incidente  
**Objetivo:** Confirmar a existência do incidente, avaliar impacto e definir prioridade para investigação.

---

## 🎯 Objetivo

A triagem é a etapa responsável por confirmar se o alerta ou reporte recebido realmente representa um incidente.

Durante a triagem são avaliados:

- impacto no ambiente
- urgência do problema
- sistemas afetados
- necessidade de escalonamento

Essa etapa evita que alertas falsos ou problemas menores consumam esforço de investigação desnecessário.

---

## 🔎 Validação do Incidente

O primeiro passo da triagem é validar se o incidente realmente existe.

Perguntas comuns nessa etapa:

- O problema ainda está ocorrendo?
- O incidente afeta mais de um usuário ou sistema?
- Existe impacto direto na aplicação?
- Existe degradação significativa de performance?

Se o incidente não for confirmado, o alerta pode ser encerrado.

---

## 📊 Avaliação de Impacto

Após confirmar o incidente, é necessário avaliar seu impacto no ambiente.

Alguns exemplos de impacto:

- banco de dados indisponível
- aplicação crítica fora do ar
- degradação severa de performance
- falha em processo de carga de dados
- bloqueios prolongados

Quanto maior o impacto, maior deve ser a prioridade de investigação.

---

## ⚠️ Classificação de Prioridade

Uma forma comum de classificar incidentes é utilizando níveis de prioridade.

Exemplo:

**P1 – Crítico**

- banco de dados indisponível
- sistema principal parado
- corrupção de banco

**P2 – Alto**

- degradação severa de performance
- bloqueios massivos
- falha em replicação ou HA

**P3 – Médio**

- falha de job
- ETL interrompido
- lentidão moderada

**P4 – Baixo**

- erro isolado
- alerta informativo
- incidente sem impacto operacional

---

## 🧰 Informações Coletadas na Triagem

Durante a triagem algumas informações devem ser registradas:

- horário do incidente
- banco ou instância afetada
- tipo de problema observado
- sistemas impactados
- mensagens de erro apresentadas

Essas informações ajudam na próxima etapa de investigação.

---

## 🔁 Próximo Passo no Processo

Após a triagem, o incidente segue para a etapa de **Diagnóstico**.

Objetivos do diagnóstico:

- identificar a causa do problema
- localizar consultas ou recursos envolvidos
- analisar métricas e logs do SQL Server

---

## 📌 Observação Operacional

Uma triagem bem executada reduz o tempo de resolução de incidentes.

Classificar corretamente prioridade e impacto permite direcionar esforços para os problemas mais críticos primeiro.
