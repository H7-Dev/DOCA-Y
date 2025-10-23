# Proposta Técnica — **Sumário Executivo (Página 2 de 2)**

**Arquitetura, Cronograma, Governança e Critérios de Aceite**


## 1) Arquitetura de Referência (alto nível)

* **Camada de Apresentação (Web/App):** PDV de Movimentações (teclado/scan), Inventário Geral, Contagem Cíclica, Comparativos e Painéis Executivos.
* **APIs & Orquestração:** serviços REST/GraphQL; filas de eventos para inventário, ajustes e logs (idempotência).
* **Dados Operacionais:** DB relacional (estoques, lotes/séries, tarefas de contagem, contratos, pedidos, custos).
* **Observabilidade:** métricas (latência, erro), logs imutáveis (auditoria), tracing de requisições.
* **Segurança:** SSO (OIDC/SAML), RBAC (Almox/Conciliador/Aprovador/Admin), MFA opcional, criptografia em trânsito/repouso.
* **Anexos & Evidências:** storage versionado (fotos, PDFs, relatórios) com trilha (quem/subiu/quando).
* **Integrações (conectores):**

  * **ERP/Fiscal:** Compras, NF-e, Financeiro/Contábil (CFOP/contas), Pedidos/Recebimentos.
  * **MDM/Cadastros:** SKU/UM, endereçamento, catálogo e políticas.
  * **Produção/PCP:** ordens, consumo/sobras, apontamentos.
  * **CRM/Contratos:** status jurídico/comercial; renovação.
  * **Frota/Manutenção:** disponibilidade, OS e preventivas.


## 2) Dados-Mestre & Qualidade (baseline)

* **Catálogo de Materiais:** código, descrição, UM, família, política de devolução (Obrigatória/Opcional/Dispensada), validade/lote.
* **Endereçamento:** depósito/rua/coluna/nível/posição, zonas críticas e endereços de quarentena/5S.
* **Políticas:** tolerâncias, casas decimais, Estoque Livre, bloqueios de estoque negativo, janelas retroativas.
* **Conformidade:** regras CMP/VRL e Absorção documentadas (normas e drivers).


## 3) Cronograma Macro (Sprints & Entregáveis)

* **S0 — Descoberta & Blueprint :** workshops, KPIs, políticas, integrações-alvo, plano de dados.
* **S1 — Fundacional PDV + MDM :** entradas/saídas/devoluções/retornos por teclado/scan; pendências; relatórios operacionais.
* **S2 — Inventário Geral :** sessão planned→closed; recontagem; reconciliação; ajustes; PDFs/CSV.
* **S3 — Contagem Cíclica :** plano ABC/giro/risco; tarefas; KPIs de tendência; heatmap por localização.
* **S4 — Reposição & Compras :** Alvo 1,5×Mínimo + em trânsito; MOQ/múltiplos/lead; pedido semi-automático → ERP.
* **S5 — Dashboards Diretoria/Comparativos :** KPIs, séries, Entrou×Saiu, export auditável.
* **S6 — Custos (CMP/VRL → Absorção) :** livro de CM; VRL; drivers; relatórios de custo.

> **Critérios de Go/No-Go por sprint:** UAT aprovado, export (CSV/PDF) conferido, trilha de auditoria funcional, KPIs batendo com amostra ERP/contábil.


## 4) RACI (resumo de papéis)

| Entrega / Decisão                  | Dir. | Comercial | Jurídico | PCP/Produção | Almox | Compras | Custos/Contábil | TI/ERP | Auditoria |
| ---------------------------------- | :--: | :-------: | :------: | :----------: | :---: | :-----: | :-------------: | :----: | :-------: |
| Políticas (tolerâncias, devolução) |   A  |     C     |     C    |       C      |   R   |    C    |        C        |    C   |     I     |
| PDV & Inventários (requisitos)     |   I  |     I     |     I    |       C      |   R   |    C    |        I        |    C   |     I     |
| Reposição (1,5×Mín + em trânsito)  |   I  |     I     |     I    |       C      |   R   |    A    |        C        |    C   |     I     |
| Integração ERP/NF-e                |   I  |     I     |     I    |       I      |   C   |    C    |        C        |    R   |     I     |
| Custos (CMP/VRL/Absorção)          |   I  |     I     |     I    |       C      |   I   |    I    |        A        |    C   |     C     |

> **R = Responsável | A = Aprovador | C = Consultado | I = Informado**


## 5) SLAs & KPIs (operação)

* **Balcão PDV:** atendimento ≤ **2 min** por retirada; 99% sem mouse (atalhos/scan).
* **Inventário Geral:** acurácia (qtd/valor) ≥ **98%**; reconciliação em ≤ **D+2**.
* **Cíclico:** cobertura semanal por zonas críticas; tendência de acurácia ↑ contínua.
* **Reposição:** rupturas evitadas; lead time médio rastreado; **≥ 95%** de pedidos dentro de política.
* **Custos:** fechamento CM/VRL em **D+3**; divergência auditável **= 0** (amostra).
* **Disponibilidade:** 99,5% mensal; latência p95 de APIs **≤ 300 ms** (internas).


## 6) Requisitos Não Funcionais

* **Segurança:** SSO, RBAC, MFA opcional; logs imutáveis por 24+ meses; anonimização quando aplicável (LGPD).
* **Desempenho:** PDV responsivo em rede comum; consultas executivas ≤ **2 s** para 95% dos casos.
* **Compatibilidade:** navegadores evergreen; coletores com câmera/laser (QR/Code128); mobile-friendly.
* **Resiliência:** retries idempotentes; fila para ajustes/inventário; fallback de dados demo (somente em dev).


## 7) Riscos & Mitigações (reforço)

* **MDM fraco (SKU/UM/endereço):** *governança obrigatória + validações de formulário + bloqueios*.
* **Tolerâncias inadequadas:** *piloto por ABC, ajustes quinzenais*.
* **Estoque negativo/retroativos:** *bloqueio sistêmico + janela de reabertura aprovada*.
* **VRL não testado:** *rotina mensal/trimestral; reversão documentada*.
* **Drivers de CIF ruins:** *comitê de custos + testes de sensibilidade e revisão por período*.


## 8) Critérios de Aceite (exemplos por domínio)

* **PDV:** realizar **entrada/saída/devolução/retorno 5S/simples** só com teclado/scan; pendência por colaborador visível.
* **Inventário Geral:** sessão completa com **recontagem automática** e **ajustes postados**; PDFs assináveis.
* **Cíclico:** plano ABC ativo; tarefas **scheduled→done** com heatmap de divergência.
* **Reposição:** geração de **pedido semi-automático** (Alvo 1,5×Mín + em trânsito + MOQ/múltiplos) → ERP.
* **Dashboards:** **Entrou×Saiu** com export; **Comparativos** com KPIs/deltas e top contributors.
* **Custos:** livro de CM consistente; **VRL** aplicado quando menor; relatório de **Absorção** por driver/capacidade.


## 9) Treinamento, Hiper-care & Documentação

* **Treinamento prático** (balcão, inventários, reposição, dashboards).
* **Hiper-care 30 dias** pós go-live (S1/S2) com plantão e SLAs de correção.
* **Manuais rápidos** (1–3 páginas) por módulo + vídeos curtos (≤5 min) por tarefa típica.

