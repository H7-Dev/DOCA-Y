# Proposta Técnica — **Sumário Executivo (1–2 páginas)**

**Plataforma Operacional Integrada** — Diretoria • Comercial • Jurídico • Produção • Compras • Almoxarifado • Custos


## 1) Visão Geral

Arquitetura única para orquestrar o ciclo **VENDER → CONTRATAR → PRODUZIR → ENTREGAR → SUSTENTAR → REPOR → GOVERNAR**.
Foco em **agilidade operacional (PDV por teclado/scan)**, **rastreabilidade total** e **decisão por dados** (dashboards executivos, comparativos e relatórios).

**Mensagem-chave:** *“Execução rápida, estoque íntegro e fechamento contábil confiável — com trilha de auditoria ponta-a-ponta.”*


## 2) Objetivos de Negócio

* **Elevar acurácia de estoque** (inventário geral + contagem cíclica + governança de movimentações).
* **Acelerar balcão** (entradas/saídas/devoluções/retornos) com **atalhos de teclado** e **leitura por código/QR**.
* **Evitar ruptura/excesso** via **reposição semi-automática** e política **1,5× Mínimo**.
* **Padronizar contratos** e renovações, integrando **Comercial ↔ Jurídico ↔ Produção**.
* **Convergir custos** (CMP/VRL e Absorção) sob normas **CPC 16 / IAS 2**.

## 3) Escopo Essencial (por módulo)

* **Diretoria** — *Painel & Comparativos & Entrou×Saiu*: filtros executivos; KPIs de delta; séries temporais; export auditável.
* **Comercial** — *Kanban de Contratos*: propostas/ativos/renovação/encerrados; status “vence em X meses”; DnD e export.
* **Jurídico** — *Ciclo de Vida*: requests→draft→review→approval→sent→signed_pending→active (+renovação/exceções).
* **Gestão/Produção** — *Lotes & QA/FAT & Frota & 5S*: gating de qualidade; OTIF; disponibilidades; compliance de prazos.
* **Compras** — *Quantidade Sugerida*: **Alvo = 1,5×Mínimo**; **Sugerido = ceil(max(0, Alvo − (Atual + Em trânsito)))**; pedido semi-automático (MOQ/múltiplos/lead).
* **Almoxarifado (Inventário Geral)** — sessão **planned→locked→counting→recount→reconciling→approved→posted→closed**; ajustes com justificativa/anexos.
* **Almoxarifado (Contagem Cíclica)** — plano por **ABC/valor/giro/risco/histórico**; **scheduled→counting→recount→reconciling→posted→done**.
* **Almoxarifado (Movimentação PDV)** — **Entrada** (chave/scan/manual/apontamento), **Saída** sem mouse (atalhos), **Devolução** (obrigatória/opcional), **Retorno 5S**, **Retorno Simples**; pendências por colaborador com trilha.


## 4) Diferenciais Funcionais

* **PDV 100% teclado/scan** no balcão (velocidade e ergonomia).
* **Trilha de auditoria completa** (quem/quando/onde/o quê/por quê + anexos).
* **Tolerâncias configuráveis** (auto-aprovação dentro do limite; workflow acima).
* **Inventário vivo**: geral (marco) + cíclico (tendência).
* **Governança de custos** (CMP/VRL + Absorção por drivers e capacidade normal).
* **Export universal** (CSV/PDF/API) para BI e auditoria.


## 5) KPIs Executivo-Chave

* **Estoque:** acurácia (qtd/valor), % itens divergentes, valor divergente, dias de cobertura.
* **Operação:** tempo médio de atendimento de balcão, % recontagem, % auto-aprovação.
* **Compras/Reposição:** sugeridos vs pedidos vs recebidos; rupturas evitadas; lead time médio.
* **Produção/Entrega:** OTIF, gargalos/top-causas, backlog QA/expedição.
* **Comercial/Jurídico:** #vigentes, #renovar≤60d, valor mensal, vencidos.
* **Custos/Financeiro:** CMV/Receita, provisões VRL, %CIF no custo.


## 6) Roadmap de Implantação (fases)

* **S0 — Descoberta & Blueprint:** políticas (devolução, 5S, tolerâncias, casas decimais), cadastros/endereços/UM, integrações-alvo e KPIs.
* **S1 — Fundacional (MDM + PDV de Movimentações):** entradas/saídas/devoluções/retornos por teclado/scan; pendências e relatórios imediatos.
* **S2 — Inventário Geral (INV-01):** sessões, recontagens, reconciliação, ajustes, evidências.
* **S3 — Contagem Cíclica (CC-01):** plano ABC/giro/risco, tarefas e KPIs de tendência.
* **S4 — Reposição & Compras:** 1,5×Mínimo + em trânsito, MOQ/múltiplos/lead; pedido semi-automático → ERP.
* **S5 — Dashboards Diretoria & Comparativos:** deltas, séries, Entrou×Saiu, export.
* **S6 — Custos (CMP/VRL → Absorção):** livro de CM, testes VRL, drivers e relatórios de absorção.

> **Critérios de aceite por sprint:** UAT com casos mínimos, export evidenciável (CSV/PDF), KPIs reconciliados com amostra operacional/contábil.


## 7) Riscos & Mitigações

* **Cadastros/UM/lotes/endereço inconsistentes** → *MDM com validação e bloqueios de entrada*.
* **Tolerâncias mal calibradas** → *piloto por ABC + revisão mensal*.
* **Estoque negativo** → *bloqueio sistêmico + janelas de reabertura aprovadas*.
* **Devoluções não cumpridas** → *pendência por colaborador/item, SLA e rastreio*.
* **VRL ignorado** → *rotina de testes VRL e reversões*.
* **Drivers de CIF inadequados** → *política de drivers + testes de sensibilidade*.


## 8) Governança, Segurança & Compliance

* **Segregação de funções** (contador/conciliador/aprovador/admin).
* **Auditoria** por movimento e por sessão (inventários, ajustes e custos).
* **CPC 16 / IAS 2** para CMP/VRL/absorção; **LGPD** (mínimo necessário; perfis e trilhas).
* **Observabilidade** (logs, métricas de latência, alarmes operacionais, testes de regressão).


## 9) Entregáveis

* **Blueprint funcional** + **EL-01 (diagramas textuais)** por processo.
* **Wireframes/Telas** (PDV, inventários, comparativos, kanbans).
* **Catálogo de políticas** (devolução, 5S, casas decimais, tolerâncias, CMP/VRL/Absorção).
* **Esquema de dados** (entidades e integrações) + **modelos de export** (CSV/PDF/API).
* **Plano de testes UAT** por sprint + **checklists de aceite**.


