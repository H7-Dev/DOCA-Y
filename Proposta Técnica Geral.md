# Proposta Técnica

**Plataforma Operacional Integrada** — Diretoria • Comercial • Jurídico • Produção • Compras • Almoxarifado • Custos


## 1) Sumário Executivo

Esta proposta consolida todo o material já elaborado em um **documento final** (estilo proposta comercial) para implantação de uma **plataforma operacional integrada**. O escopo cobre o ciclo ponta-a-ponta:

**VENDER (Comercial) → CONTRATAR (Jurídico) → PRODUZIR (Fábrica) → ENTREGAR (Frota/Expedição) → SUSTENTAR (5S/Compliance) → REPOR (Compras/Estoque) → GOVERNAR (Diretoria/Financeiro/Contábil).**

A entrega final inclui **modelos de tela**, **pipelines EL-01** (diagramas textuais), **regras de negócio** (inventários, CMP, absorção, quantidade sugerida), **governança, dados e integrações**, além de **KPIs** e um **roadmap** para implantação por fases.

**Mensagem-chave:** desenhamos para **agilidade operacional (teclado/scan, zero-mouse)**, **rastreabilidade total** e **decisão por dados** (dashboards executivos e comparativos).


## 2) Objetivos de Negócio

* **Aumentar a acurácia de estoque** (inventário geral + contagem cíclica + governança de movimentações).
* **Acelerar operação de balcão** (entradas/saídas/devoluções) com **atalhos PDV** e leitura por **código/QR**.
* **Reduzir ruptura e excesso**, com **reposição semi-automática** e política de **quantidade sugerida**.
* **Padronizar contratos e renovações**, conectando **Comercial ↔ Jurídico ↔ Produção**.
* **Elevar a transparência**: **trilha de auditoria**, KPIs e relatórios em diferentes granularidades.
* **Convergir custos** (CMP/absorção) com políticas normativas e impactos contábeis/fiscais claros.


## 3) Escopo Funcional por Módulo

### 3.1 Diretoria — Painéis & Comparativos

* **Painel Resumo Geral:** filtros (período, unidade, cliente), request idempotente, backend de agregação (KPIs, séries, alertas), render com estados UX e timestamp.
* **Comparativos:** períodos base × comparação, KPIs de delta (% e absoluto), gráfico principal (linhas/barras), top contributors, export CSV, governança (timestamp, consistência, latência).
* **Relatório Rápido Entrou × Saiu:** presets de período, agregação (dia/semana/mês), séries paralelas, KPIs (E, S, saldo, razão), listas rastreáveis, insights e export.

### 3.2 Comercial — Onde o ciclo nasce (Kanban de Contratos)

* Colunas: **Propostas, Ativos, Renovação, Encerrados**.
* Cartões: código/cliente, equipamentos, períodos, local, valor/mês, status “vence em X meses”.
* Filtros por **cliente/equipamento**, criação rápida, **drag&drop**, export JSON/CSV, persistência local.

### 3.3 Jurídico — Ciclo de Vida do Contrato

* Colunas: **requests → draft → review → approval → sent → signed_pending → active** (+ renovação, vencidos, on_hold, terminated, archived, canceled).
* Cartões com **metadados, risco, progresso**, filtros estáticos e ganchos para integrações futuras.

### 3.4 Gestão (Fábrica) — Integração Comercial/Jurídico ↔ Produção

* Lotes de fabricação: **Engenharia & Liberação → Planejado → Em Produção → QA/FAT → Pronto p/ Expedição → Entregue**.
* Estoques MP/PA, 5S/compliance, relatórios de produção (Q, HH/HM, custos), frota (disponibilidade, manutenção), **visões transversais** (“Hoje, Semana, Mês, Cliente/Projeto”).

### 3.5 Compras — Política de Quantidade Sugerida

* **Alvo = 1,5 × Mínimo**; **Sugerido = ceil(max(0, Alvo − Atual))** com política “Estoque Livre (0,001)”.
* Simulador, exemplos, legendas, checklist, limitações (lead time, pedidos abertos, MOQ/múltiplos, etc.).
* **Pedido semi-automático** considerando **em trânsito**, **MOQ/múltiplos** e **lead time** (quando ativado).

### 3.6 Almoxarifado — Inventário Geral

* Sessão com **planned → locked → counting → recount → reconciling → approved → posted → closed**.
* Tipos: geral, por localização, ABC, cego, dupla contagem.
* Reconciliação e ajustes com justificativa, anexos e impactos em custo/valorização.
* Permissões: contador, conciliador, aprovador, admin. KPIs: acurácia, % divergentes, tempo médio, % recontagens.

### 3.7 Almoxarifado — Contagem Cíclica

* Plano por **ABC/valor/giro/risco/histórico**.
* Fluxo: **scheduled → counting → recount → reconciling → posted → done**.
* Tarefas por endereço/lote, tolerâncias por grupo, KPIs de tendência e % auto-aprovação.

### 3.8 Almoxarifado — Movimentação Operacional & Governança

* **Entrada:** chave bipada/digitada, manual ou apontamento sem documento; validações de lote/validade/destino.
* **Saída PDV (teclado/scan):** solicitante, liberador, contrato/OP; **devolução obrigatória** por família quando definida.
* **Devolução:** fecha pendência ou direciona para **5S** (inservível/reciclagem/conserto); histórico de cumprimento.
* **Retorno 5S:** baixa para perda/consumo 5S; indicadores de desperdício.
* **Retorno Simples:** repõe saldo ao endereço/lote de origem (erro de retirada).
* **Relatórios:** tempo real e por períodos (dia, quinzena, mês, semestre, ano, custom), “o que saiu” por **cliente/contrato/OP**, quem solicitou/liberou, tipo (venda/locação/outros).
* **Monitoramento de estoque & reposição:** sugeridos, em trânsito, rupturas/excessos, catálogo governado (uso/validades/devolução), **pedido semi-automático**.

### 3.9 Custos — CMP e Custeio por Absorção

* **CMP (perpétuo):** recalcula custo médio nas entradas; saídas usam CM vigente; devoluções revertem a operação; **VRL** quando custo > realizável.
* **Absorção:** diretos (MP/MOD) + CIF (fixos/variáveis) rateados por driver coerente; **capacidade normal** para CIF fixos; SG&A fora do estoque.
* Tabelas de governança, riscos/controles e KPIs (giro, CMV/receita, provisões VRL, acurácia de inventário, %CIF etc.).


## 4) Arquitetura de Dados & Integrações (resumo)

**Entidades centrais (amostra):**

* `contracts`, `contract_events` (Comercial/Jurídico)
* `lots`, `lot_stages`, `quality_checks` (Produção/QA)
* `stock_items`, `stock_locations`, `stock_batches` (MP/PA)
* `movements` (entrada/saída/devolução/retorno/5S), `movement_attachments`
* `inventory_sessions`, `inventory_counts`, `inventory_discrepancies`, `inventory_adjustments`, `inventory_logs`, `inventory_attachments`
* `cc_plans`, `cc_tasks`, `cc_counts`, `cc_discrepancies`, `cc_adjustments`, `cc_logs`
* `purchasing_suggestions`, `purchase_orders`
* `cost_cm_ledger`, `cost_absorption_rules`, `cost_postings`

**Integrações:** ERP/Fiscal (NF-e, CFOP, contas), BI (export APIs/CSV), Coletor móvel (QR/Barcode), Frota/OS, PCP, MDM (cadastro, UMs, endereços), Autenticação/Perfis.


## 5) Governança, Segurança & Auditoria

* **Trilha completa**: quem, quando, onde, o quê, por quê (ID de origem, anexos, justificativas).
* **Segregação de funções**: contador × conciliador × aprovador × admin.
* **Políticas**: bloqueio de estoque negativo, janelas de reabertura, casas decimais, tolerâncias por grupo.
* **Compliance 5S/Legal**: extintores, licenças ambientais, alvarás; calendário e SLA de vencimentos.
* **LGPD**: dados pessoais mínimos (identificação de solicitante/liberador), com controles de acesso.
* **Observabilidade**: logs de consulta, tempos de resposta, alertas operacionais, testes de regressão.


## 6) KPIs Executivos (capa da reunião)

* **Contratos**: #vigentes, #renovar≤60d, valor/mês.
* **Lotes**: #em produção, OTIF, top-3 causas de atraso.
* **MP**: itens < mínimo, **dias de cobertura**, compras sugeridas.
* **PA**: pronta-entrega, backlog expedição, lead QA.
* **5S/Compliance**: % em dia, #30d / #7d / vencidos.
* **Produção**: Q (dia/mês), HH/HM, custo unitário real vs plano.
* **Frota**: disponibilidade, preventiva vencendo, custo/km.
* **Estoque**: acurácia (qtd/valor), % itens divergentes, valor divergente.
* **Financeiro/Costing**: CMV/Receita, provisões VRL, %CIF no custo.


## 7) Roadmap de Implantação (fases sugeridas)

**Sprint 0 — Descoberta & Blueprint**
Levantamento de cadastros, fluxos, integrações e KPIs; definição de políticas (tolerâncias, devolução, 5S, casas decimais).

**S1 — Fundacional (MDM + Movimentação PDV)**
Catálogo/endereçamento, perfis, atalhos/scan; **entradas/saídas/devoluções/retornos** com trilha.

**S2 — Inventário Geral (INV-01)**
Sessões, contagens, recontagens, reconciliação e ajustes; relatórios e anexos.

**S3 — Contagem Cíclica (CC-01)**
Plano, tarefas, coleta móvel, reconciliação, KPIs de tendência.

**S4 — Reposição & Compras (Sugeridos → Pedido Semi-automático)**
Regra 1,5×Mínimo, em trânsito, MOQ/múltiplos/lead, export para ERP.

**S5 — Dashboards de Diretoria & Comparativos**
KPIs executivos, comparativos, relatório Entrou×Saiu.

**S6 — Custos (CMP → Absorção)**
Livro de CM, VRL, drivers e absorção; relatórios para controladoria.

**S7 — Integrações ampliadas (Frota/OS, QA/FAT, BI)**
Fechos, automações, painéis transversais (“Hoje, Semana, Mês, Cliente”).

> **Critérios de aceite** por sprint: UAT com cenários mínimos, export evidenciável (CSV/PDF), KPIs batendo com amostra contábil/operacional.


## 8) Riscos & Mitigações

| Risco                                       | Impacto                      | Mitigação                                              |
| ------------------------------------------- | ---------------------------- | ------------------------------------------------------ |
| Cadastros inconsistentes (UM/lote/endereço) | Divergências recorrentes     | **MDM** e validações na entrada                        |
| Tolerâncias mal calibradas                  | Ajustes em excesso/escassez  | Piloto por **ABC** + revisão mensal                    |
| Estoque negativo                            | CMV e relatórios distorcidos | **Bloqueio sistêmico** + janela de reabertura aprovada |
| Falta de devolução                          | Perdas e falta de rastreio   | **Pendência por colaborador** + políticas por família  |
| VRL ignorado                                | Estoque acima do realizável  | Rotina de **teste VRL** (mensal/trimestral)            |
| Drivers de CIF inadequados                  | Custo unitário distorcido    | Política de **drivers** + sensibilidade periódica      |


## 9) Papéis, SLAs & Controles

| Papel                      | Responsabilidades                                 | SLAs (exemplo-guia)                         | Controles                                |
| -------------------------- | ------------------------------------------------- | ------------------------------------------- | ---------------------------------------- |
| **Solicitante**            | Retirada e devolução conforme política            | Devolução obrigatória **T+1** (ou por item) | Identificação + vínculo (contrato/OP/CC) |
| **Conferente/Almoxarife**  | Liberar via PDV (teclado/scan), checar saldo/lote | Atendimento balcão **< X min**              | Check de lote/validade/endereço          |
| **Supervisor Almox**       | Pendências, 5S, auditoria diária                  | Fechar pendências **D+1**                   | Relatórios e exceções                    |
| **Compras**                | Revisar sugeridos, gerar pedidos                  | Pedido revisado **≤ 48h**                   | MOQ/múltiplos/lead aplicados             |
| **PCP/Produção**           | Reservas por lote, consumo                        | Apontamentos **dia D**                      | Integração com ordens                    |
| **Controladoria/Contábil** | Ajustes, CMP/VRL, absorção                        | Fechamento **M+3 úteis**                    | Logs e reconciliações                    |
| **TI/MDM**                 | Cadastros, perfis, integrações                    | Mudanças **via change**                     | Versionamento e testes                   |
| **SSMA/Qualidade**         | 5S, licenças, inspeções                           | Itens 7d/30d sem atrasos                    | Evidências anexadas                      |

*(SLAs finos serão acordados na Sprint 0.)*


## 10) Termos de Entrega & Materiais

* **Documento funcional** (este) + **Anexos EL-01** (diagramas textuais).
* **Wireframes/telas** (já esboçados no material de base: FAQ, Inventários, Comparativos, Kanbans).
* **Catálogo de políticas** (devolução, 5S, CMP/VRL, absorção, tolerâncias, casas decimais).
* **Arquivos de export** (modelos CSV) e **esquema de dados** (resumo).
* **Plano de testes UAT** por sprint e **checklists**.


## 11) Anexos — Pipelines EL-01 (diagramas textuais)

### 11.1 Pipeline Geral (do pedido ao fechamento)

```Ruby
[ Pedido/Contrato aprovado + Handover ]
│ ├─ Comercial move contrato p/ “Ativo”; Jurídico em “Vigente”
│ ├─ PCP cria Lote(s) com BOM/rota e prazos
│ ├─ Compras verifica MP crítica e sugeridos (1,5×Mínimo)
│ └─ Almoxarifado prepara endereços e políticas (devolução/5S)
│
├──▶ Conclusão: Produção liberada com material garantido

[ Execução de Produção + QA/FAT ]
│ ├─ Lote em Produção (apontamentos HH/HM e consumo)
│ ├─ QA/FAT check; retrabalho se necessário
│ ├─ PA reservado por contrato/entrega
│ └─ Frota agenda coleta/rota
│
├──▶ Conclusão: Produto pronto e janela de expedição marcada

[ Movimentações de Estoque (PDV/Scan) ]
│ ├─ Entradas (NF-e/chave/manual/apontamento)
│ ├─ Saídas (contrato/OP/CC) com atalhos e sem mouse
│ ├─ Devoluções (obrigatórias/opcionais) e Retornos (5S/Simples)
│ └─ Pendências e evidências tratadas diariamente
│
├──▶ Conclusão: Saldos íntegros e rastreáveis

[ Reposição & Compras ]
│ ├─ Sugeridos (1,5×Mínimo – Atual – Em trânsito)
│ ├─ Ajustes por MOQ/múltiplos/lead time
│ ├─ Geração de pedido semi-automático
│ └─ Integração ERP & follow-up
│
├──▶ Conclusão: Ruptura evitada, giro saudável

[ Inventários & Custos ]
│ ├─ Inventário Geral (sessão completa) e Cíclico (recorrente)
│ ├─ Ajustes aprovados com trilha
│ ├─ CMP/VRL e Absorção (drivers/normal capacity)
│ └─ Fechamento e notas explicativas
│
├──▶ Conclusão: Fecho contábil consistente e auditável
```

### 11.2 Pipeline — Almoxarifado (Movimentação)

```Ruby
[ Entrada de Materiais + Captura ágil e auditável ]
│ ├─ Identifica origem (NF-e/chave bipada, digitação, manual, apontamento)
│ ├─ Valida item/lote/validade e destino (depósito/endereço)
│ ├─ Registra evidências e responsável
│ └─ Atualiza saldo + integrações fiscais
│
├──▶ Conclusão: Entrada registrada e estoque atualizado

[ Saída de Material (PDV) + 100% via teclado/scan ]
│ ├─ Busca rápida (código/QR), quantidade e destino (Contrato/OP/CC)
│ ├─ Aplica política (devolução obrigatória / lote exigido)
│ ├─ Baixa estoque com solicitante/liberador/motivo
│ └─ Comprovante + abertura de pendência (se aplicável)
│
├──▶ Conclusão: Saída confirmada em segundos

[ Devolução + Pendência rastreável ]
│ ├─ Vincula ao movimento original e confere condição
│ ├─ Fecha pendência e decide retorno/5S
│ ├─ Ajusta saldo e registra causa
│ └─ Mantém histórico por colaborador
│
├──▶ Conclusão: Pendência fechada, saldo reconciliado

[ Retornos 5S & Simples ]
│ ├─ 5S: classifica (inservível/reciclagem/conserto) e evidencia
│ ├─ Simples: devolução por engano → repõe no endereço/lote
│ └─ Atualiza indicadores de desperdício/acurácia
│
├──▶ Conclusão: Conformidade e disciplina de uso preservadas
```

### 11.3 Pipeline — Inventário Geral (INV-01)

```Ruby
[ Planejar → Bloquear → Contar → Recontar → Reconciliar → Aprovar → Lançar → Encerrar ]
│ ├─ Escopo e tolerâncias definidos; lock aplicado (cut-off)
│ ├─ 1ª contagem por endereço; recontagem automática se > tolerância
│ ├─ Delta SIS×Físico, causas e proposta de ajuste com anexos
│ ├─ Aprovação segregada; lançamentos refletidos em custo/valorização
│ └─ Relatórios e evidências em arquivo (sessão “closed”)
│
├──▶ Conclusão: Acurácia validada e trilha completa
```

### 11.4 Pipeline — Contagem Cíclica (CC-01)

```Ruby
[ Definir critérios/frequências → Gerar tarefas → Executar → Recontar → Reconciliar → Postar → Feedback ]
│ ├─ ABC/valor/giro/risco/histórico + tolerâncias
│ ├─ Coleta guiada por endereço (código/QR), bloqueio temporário
│ ├─ Ajustes dentro da tolerância auto-aprovados; fora → aprovação
│ └─ KPIs de tendência alimentam nova priorização
│
├──▶ Conclusão: Desvio corrigido cedo, sem parar a operação
```

### 11.5 Pipeline — Quantidade Sugerida & Reposição

```Ruby
[ Políticas → Cálculo → Revisão → Pedido → Acompanhamento ]
│ ├─ Alvo = 1,5×Mínimo; Estoque Livre (0,001) conforme política
│ ├─ Sugerido = ceil(max(0, Alvo − (Atual + Em trânsito))) ± MOQ/múltiplos/lead
│ ├─ Revisão por Compras/Almox; gerar pedido semi-automático
│ └─ Acompanhar recebimento e atualizar cobertura
│
├──▶ Conclusão: Repor certo, na hora certa
```

### 11.6 Pipeline — Custos (CMP)

```Ruby
[ Cadastro → Recebimento/Fiscal → Produção (se houver) → Entrada (recalcula CM) → Saída (CM vigente) → VRL → Fechamento ]
│ ├─ Entradas recalculam CM; saídas usam CM do momento
│ ├─ Devoluções revertem operação original; VRL quando aplicável
│ └─ Notas explicativas e trilha do cálculo por evento
│
├──▶ Conclusão: CMV e estoque confiáveis, auditáveis
```



