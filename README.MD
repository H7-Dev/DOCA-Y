# Proposta Técnica



## `Módulo__Diretoria:` Painel Resumo Geral

```Ruby
[ CAPÍTULO I — CONTROLES / FILTROS (PONTO DE ENTRADA) ]
│  Moldam o escopo da consulta e direcionam o fluxo de dados.
│  ├─ Art.1  ✔ Período (granularidade temporal)
│  ├─ Art.2  ✔ Unidade (recorte organizacional)
│  ├─ Art.3  ✔ Entidade/Cliente (filtro nominal)
│  ├─ Art.4  ✔ Ações rápidas (Reset / Exportar)
│  └─ Art.5  ✔ Acessibilidade & usabilidade (labels, foco, tab)
│
├──▶ (Propósito) → Reduzir e parametrizar o universo de dados para consulta.
│
[ CAPÍTULO II — DISPARO / REQUEST (TRANSMISSÃO) ]
│  Ponto em que a UI transforma seleção do usuário em chamada estruturada.
│  ├─ Art.1  ✔ Payload claro e idempotente (filtros + meta)
│  ├─ Art.2  ✔ Validação mínima antes do envio (client-side)
│  ├─ Art.3  ✔ Controle de estado (loading / debounce)
│  ├─ Art.4  ✔ Requisições canceláveis/repetíveis (race conditions)
│  └─ Art.5  ✔ Segurança (autenticação, rate-limit)
│
├──▶ (Princípio) → Garantir que o request seja eficiente, seguro e previsível.
│
[ CAPÍTULO III — BACKEND / PROCESSAMENTO (AGREGAÇÃO) ]
│  Responsável por consultar, agregar e preparar a resposta.
│  ├─ Art.1  ✔ Query DW/DB (filtros aplicados)
│  ├─ Art.2  ✔ Cálculos de KPIs (agregações, médias, variações)
│  ├─ Art.3  ✔ Séries temporais (listas de pontos para gráficos)
│  ├─ Art.4  ✔ Geração de alertas (regras de severidade)
│  └─ Art.5  ✔ Metadata (timestamp, versionamento da consulta)
│
├──▶ (Resultado) → Resposta completa: KPIs, séries, alertas e timestamp.
│
[ CAPÍTULO IV — RENDER / VISUALIZAÇÃO (TRADUÇÃO) ]
│  Transformar dados em compreensão imediata e acionável.
│  ├─ Art.1  ✔ KPIs sintéticos (visuais, clicáveis para drill-down)
│  ├─ Art.2  ✔ Gráficos de tendência e comparativo (contexto histórico)
│  ├─ Art.3  ✔ Lista de alertas priorizada (ações sugeridas)
│  ├─ Art.4  ✔ Estados UX (carregando, vazio, erro)
│  └─ Art.5  ✔ Indicação de frescor (timestamp visível)
│
├──▶ (Objetivo) → Maximizar compreensão rápida e suportar decisão operacional.
│
[ CAPÍTULO V — AÇÕES DO USUÁRIO (FEEDBACK) ]
│  Interações que alteram o fluxo ou geram efeitos fora da visualização.
│  ├─ Art.1  ✔ Drill-down (navegar para detalhe)
│  ├─ Art.2  ✔ Reset (limpar contexto e reconsultar)
│  ├─ Art.3  ✔ Export (PDF / snapshot do estado atual)
│  ├─ Art.4  ✔ Receber/ack de alertas (workflow)
│  └─ Art.5  ✔ Persistência de preferências (últimos filtros)
│
├──▶ (Ação) → Usuário transforma insight em operação, gerando nova iteração.
│
[ CAPÍTULO VI — GOVERNANÇA & OBSERVABILIDADE (CONTRATO) ]
│  Garantias de qualidade, rastreio e confiabilidade do pipeline.
│  ├─ Art.1  ✔ Timestamp como contrato de frescor
│  ├─ Art.2  ✔ Logs de consulta e métricas (latência, erros)
│  ├─ Art.3  ✔ Testes automatizados (integração e regressão)
│  ├─ Art.4  ✔ Tratamento de falhas e fallback (carga/erro)
│  └─ Art.5  ✔ Monitoramento e alertas operacionais do pipeline
│
├──▶ (Garantia) → Transparência sobre a origem, qualidade e atualidade dos dados.
│
├──▶ (Fluxo Geral) → CONTROLES → REQUEST → BACKEND → RENDER → AÇÕES → GOVERNANÇA
│
```

## `Módulo__Diretoria:` Comparativos

```Ruby

[ CAPÍTULO I — CABEÇALHO & METADADOS (IDENTIDADE) ]
│  Identifica o comparativo e informa frescor / escopo.
│  ├─ Art.1  ✔ Título claro e contextual (mês / trimestre / ano)
│  ├─ Art.2  ✔ Timestamp de última atualização visível
│  ├─ Art.3  ✔ Indicador do escopo atual (ex.: “Comparativo — Mês vs Mês”)
│  ├─ Art.4  ✔ Nota de fonte (Data Warehouse / contrato de dados)
│  └─ Art.5  ✔ Informação mínima para confiança antes da leitura
│
├──▶ (Propósito) → Segurança cognitiva: saber o que se vê e quão recente são os dados.
│
[ CAPÍTULO II — FILTROS COMPARATIVOS (PONTO DE ENTRADA) ]
│  Parametrizam o par de períodos e recortes que definem o comparativo.
│  ├─ Art.1  ✔ Período base (ex.: Ago 2025)
│  ├─ Art.2  ✔ Período de comparação (ex.: Jul 2025)
│  ├─ Art.3  ✔ Dimensão de análise (produto / cliente / unidade / geral)
│  ├─ Art.4  ✔ Top N (amostragem: 10 / 20 / 50)
│  └─ Art.5  ✔ Ações diretas (Aplicar / Exportar CSV) e acessibilidade dos controles
│
├──▶ (Princípio) → Definir com precisão o par de séries que será comparado.
│
[ CAPÍTULO III — KPIs COMPARATIVOS (SÍNTESE NUMÉRICA) ]
│  Resumo numérico que quantifica as diferenças essenciais entre períodos.
│  ├─ Art.1  ✔ Métricas base / comparação / delta absoluto
│  ├─ Art.2  ✔ Delta % e variação relativa (normalização)
│  ├─ Art.3  ✔ Indicadores de tendência (↑/↓ / estável)
│  ├─ Art.4  ✔ Links rápidos para drill-down por métrica
│  └─ Art.5  ✔ Consistência: soma/total entre KPIs e detalhe (checagem de integridade)
│
├──▶ (Objetivo) → Fornecer a leitura imediata do que mudou entre os dois períodos.
│
[ CAPÍTULO IV — VISUALIZAÇÃO PRINCIPAL & CONTROLES DO GRÁFICO (TRADUÇÃO) ]
│  Representação visual do comparativo (séries sobrepostas + variação).
│  ├─ Art.1  ✔ Gráfico principal (linha/barras sobrepostas) — comparação direta
│  ├─ Art.2  ✔ Option: tipo de gráfico (line / bar) para diferentes leituras
│  ├─ Art.3  ✔ Índice base (normalização / mostrar índice base)
│  ├─ Art.4  ✔ Legenda, escala e indicação clara de base vs comparação
│  └─ Art.5  ✔ Estados UX (loading / sem dados / erro / tooltip explicativo)
│
├──▶ (Princípio) → Facilitar identificação de padrões, picos e variações significativas.
│
[ CAPÍTULO V — CONTRIBUTORS, TABELAS & INSIGHTS AUTOMÁTICOS (ANÁLISE) ]
│  Decomposição do delta e sugestões geradas automaticamente.
│  ├─ Art.1  ✔ Top Contributors — itens que mais explicam o delta (ranking)
│  ├─ Art.2  ✔ Tabela compacta com Base / Comp / Delta % (ordenável)
│  ├─ Art.3  ✔ Insights automáticos (ex.: “X contribuiu +30% para a queda”)
│  ├─ Art.4  ✔ Amostra por dimensão (chart-dim) para verificar padrões segmentados
│  └─ Art.5  ✔ Possibilidade de exportar subconjuntos (CSV do Top N)
│
├──▶ (Valor) → Transformar diferença em diagnóstico: quem puxou o delta e por quê.
│
[ CAPÍTULO VI — AÇÕES & INTERAÇÕES (REAÇÃO) ]
│  O que o usuário pode fazer a partir do insight do comparativo.
│  ├─ Art.1  ✔ Aplicar (refresh) → nova requisição com os filtros atuais
│  ├─ Art.2  ✔ Exportar CSV (dados tabulares) — permitir auditoria externa
│  ├─ Art.3  ✔ Drill-down para detalhe de item/cliente/produto
│  ├─ Art.4  ✔ Marcação/flag de insight para workflow (follow-up)
│  └─ Art.5  ✔ Persistência de preferências (últimos filtros / topN)
│
├──▶ (Ação) → Converter insight em tarefa/consulta mais profunda ou em arquivo auditável.
│
[ CAPÍTULO VII — FLUXO OPERACIONAL (PIPELINE) ]
│  Sequência de transformação de filtros em insight e ação.
│  ├─ Art.1  ✔ User selects filters (base, comp, dim, topN)
│  ├─ Art.2  ✔ Front-end envia request com payload comparativo
│  ├─ Art.3  ✔ Backend executa joins/aggregations → produz: séries (base & comp), top contributors, métricas derivadas e timestamp
│  ├─ Art.4  ✔ Front-end rendeiriza: KPIs, gráfico principal, tabela de contributors, chart-dim, insights automáticos
│  └─ Art.5  ✔ User realiza ação (export, drill, marcar) → possível nova iteração do pipeline
│
├──▶ (Síntese) → FILTERS → REQUEST → AGGREGATE → RENDER → ACTION → ITERATE
│
[ CAPÍTULO VIII — GOVERNANÇA, QUALIDADE E OBSERVABILIDADE (CONTRATO) ]
│  Regras e verificações que garantem confiabilidade do comparativo.
│  ├─ Art.1  ✔ Timestamp e versão da query como prova de frescor
│  ├─ Art.2  ✔ Validação de consistência (KPIs agregados vs detalhe)
│  ├─ Art.3  ✔ Rounding / normalização aplicada de forma documentada
│  ├─ Art.4  ✔ Detecção automática de anomalias para destacar vieses (ex.: outliers)
│  └─ Art.5  ✔ Logs, métrica de latência e testes de integração para o endpoint comparativo
│
├──▶ (Garantia) → Transparência sobre precisão, origem e limites da análise.
│
├──▶ (Fluxo Geral) → CABEÇALHO → FILTROS → REQUEST → AGGREGATE → RENDER → ACTIONS → GOVERNANÇA
│

```

## `Módulo__Diretoria:` Custo Média Ponderado

### [ ❗❗❗ Assista ao vídeo Extramaente Importante ❗❗❗](https://youtu.be/TDwW86IJ7Wg)

**Resumo executivo:**<BR>
O **Custo Médio Ponderado (CMP)** calcula um **custo unitário médio** sempre que ocorre **entrada** (compra/produção), incorporando tributos **não recuperáveis** e custos necessários até a condição/localização do item. As **saídas** (vendas/consumos) usam **o CM vigente do momento** para apurar o **CMV**. No fechamento, aplica-se o **menor entre custo e VRL** (valor realizável líquido) e divulga-se a **política adotada** (CMP, critérios de custos e testes de VRL).


**Regras e políticas operacionais (versão executiva):**

1. **Entradas (compra/produção)**
   • Atualizam quantidade e valor do estoque; o **CM é recalculado** com base no valor total da entrada (preço + frete/seguro/manuseio + impostos **não recuperáveis**).
   • Despesas financeiras explícitas de prazo ficam **fora** do custo (reconhecidas como financeiro).
   • Em produção, **CIF** são rateados pela **capacidade normal**; desperdícios **anormais** viram despesa do período.

2. **Saídas (vendas/consumos)**
   • Usam **o CM vigente** para reconhecer o **CMV**; o **CM não muda** por causa da saída.
   • **Devolução de venda**: repõe estoque e **reverte o CMV** da saída original (sem recalcular o CM).

3. **Ajustes e exceções**
   • **Devolução de compra**: reverte a entrada pelo **custo original** e **recalcula** o CM.
   • **VRL** (write-down/ reversão): reduzir ao VRL se menor que o custo; reverter quando cessarem os motivos.

4. **Governança e precisão**
   • Padronizar **casas decimais** (ex.: quantidade 3–4; valores 2–3).
   • **Proibir estoque negativo** e controlar **lançamentos retroativos** com janelas de reabertura aprovadas.
   • Divulgar **políticas** (fórmula CMP, critérios de custo e VRL) e manter **trilha de auditoria**.




### 1) Fundamentos normativos (essenciais)

---

* **Mensurar estoques** pelo **custo** ou **VRL**, dos dois o menor.
* **Custo** inclui aquisição (preço, impostos **não recuperáveis**, frete/seguro/manuseio) e **transformação** (mão de obra direta + CIF fixos/variáveis rateados em **capacidade normal**).
* **Fórmulas admitidas**: **CMP** e **FIFO** (LIFO não é admitido em IFRS/BR-GAAP).
* **Divulgações**: política adotada (CMP), valores reconhecidos no resultado (CMV), write-downs e reversões, critérios de custo e de VRL.

---

### 2) Política recomendada (para deliberação de diretoria)

* **Adotar CMP** (perpétuo) para itens fungíveis, alto giro e baixo risco de validade.
* **Usar FIFO** quando houver exigência de **rastreabilidade física** (lotes/validade).
* **Vedado LIFO**.
* **VRL**: estabelecer **periodicidade** (mensal/trimestral por família SKU) e critérios de reversão.
* **Casas decimais oficiais** e **bloqueio** de estoque negativo definidos como política de sistema.

---

### 3) Pipeline Linear (EL-01) — diagrama textual

**Cadastro do Item** → **Recebimento & Fiscal** (compõe custo de aquisição) →
**Produção/Transformação** (se aplicável; CIF por capacidade normal) →
**Entrada no Estoque** (**recalcula CMP**) →
**Saída** (**aplica CM vigente** e reconhece **CMV**) →
**VRL** (write-down/reversão) →
**Conciliação & Fechamento** (inventário físico, KPIs, notas explicativas)

---

### 4) Tabela detalhada das etapas (governança)

| Etapa                | Objetivo                      | Entradas                   | Regra/Política                                    | Saída/Registro     | Responsável       | Controle-chave                 |
| -------------------- | ----------------------------- | -------------------------- | ------------------------------------------------- | ------------------ | ----------------- | ------------------------------ |
| Cadastro             | Padronizar UMs, NCM, lote     | Ficha do item              | Política de casas decimais; sem estoque negativo  | Item aprovado      | Compras/Fiscal/TI | Workflow de aprovação          |
| Recebimento & Fiscal | Validar NF e tributos         | NF-e, pedido, frete/seguro | Custos **não recuperáveis** entram no custo       | Entrada lançada    | Fiscal/Contábil   | Auditoria de impostos e preços |
| Produção (se houver) | Compor custo de transformação | BOM, MO, CIF               | CIF por **capacidade normal**; anormais → despesa | Custo de produção  | PCP/Custos        | Tolerâncias/refugos            |
| Entrada              | Atualizar saldos e CM         | Q, custo unitário          | Recalcula **CM** a cada entrada                   | Q’, V’, CM’        | Estoque/Contábil  | Log de cálculo por evento      |
| Saída                | Reconhecer CMV                | Q, **CM vigente**          | CMV = quantidade × CM vigente                     | Nota/CMV           | Vendas/Estoque    | Bloqueio sem saldo             |
| Devoluções           | Corrigir operação             | NF devolução               | Compra: reverte entrada; Venda: reverte CMV       | Saldos ajustados   | Fiscal/Contábil   | Amarração à nota origem        |
| VRL                  | Prudência                     | Preços, custos de venda    | Baixa p/ VRL; reversões quando aplicável          | Provisões          | Contábil          | Testes por família SKU         |
| Fechamento           | Evidenciar política           | Relatórios, inventário     | Divulgar fórmula/política e write-downs           | Notas explicativas | Contábil          | Trilha de auditoria            |

---

### 5) Casos especiais (pontos de decisão)

* **Bonificação de compra** vinculada a itens: **reduz o custo** do item; bonificação geral: tratar como **receita/abatimento**, não no custo.
* **Frete e custos acessórios**: entram no custo **se necessários** à condição/localização do item; do contrário, são despesa.
* **Devolução de venda**: **repor** estoque e **reverter CMV** exatamente pelo custo usado na saída original.
* **Lançamentos retroativos**: definir janela controlada e **recalcular CMP** apenas com autorização.

---

### 6) Riscos & controles (para a reunião)

| Risco                        | Impacto                     | Mitigação                                  |
| ---------------------------- | --------------------------- | ------------------------------------------ |
| Estoque negativo             | CMV distorcido; auditoria   | Bloqueio sistêmico + rotina de acertos     |
| Tributos mal classificados   | Custo super/subavaliado     | Validação fiscal automática                |
| CIF mal rateado              | Custo de produção incorreto | Critérios por capacidade normal; revisões  |
| VRL ignorado                 | Estoque acima do realizável | Testes periódicos e reversões documentadas |
| Arredondamento inconsistente | Diferenças residuais        | Política única de casas decimais/algoritmo |

---

### 7) KPIs para diretoria (painel)

* **Giro de estoque** e **cobertura em dias**
* **CMV/Receita** e **margem bruta**
* **Provisões VRL** (e **reversões**) por família SKU
* **Acurácia de inventário** (%) e diferenças (R$)
* **Mapa de custos**: participação de aquisição vs. transformação

---

### 8) UAT (cenários mínimos de aceitação)

1. Entradas múltiplas com custos distintos → **CM recalculado** corretamente; saídas usando **CM do momento**.
2. Devolução parcial de compra → **reversão** pelo custo da nota e **recalcula CM**.
3. Devolução de venda → **reversão do CMV** e reposição pelo custo original.
4. Frete/manuseio pós-compra → classificar **no custo** somente se necessários à condição/localização.
5. VRL em queda de preço → **write-down**; quando recuperar, **reversão** registrada.

---

### 9) Checklist de adoção

* Política formal **CMP** (perpétuo); **FIFO** quando houver rastreio; **LIFO vedado**.
* Parametrização de **tributos recuperáveis × não recuperáveis**.
* **CIF** por **capacidade normal** e política de desperdícios.
* **Bloqueio** de estoque negativo; **fechamento duro** com reabertura controlada.
* Rotina de **VRL** e **KPIs** no fechamento.

---

### 10) Anexo didático — narrativa do exemplo

* **Saldo inicial**: Q=0; V=0.
* **Compra A**: 40 un a R$ 1,50 → Q=40; V=60; **CM=1,50**.
* **Compra B**: 25 un a R$ 1,60 → Q=65; V=100; **CM≈1,5385**.
* **Saída 1**: 20 un a **CM≈1,5385** → **CMV≈30,77**; Q=45; V≈69,23; **CM mantém**.
* **Compra C**: 10 un a R$ 1,70 → Q=55; V≈86,23; **CM≈1,5687**.
* **Saída 2**: 30 un a **CM≈1,5687** → **CMV≈47,06**; Q=25; V≈39,17; **CM mantém**.
  *(Precisões finais seguem política de casas decimais.)*

---

### 11) Comparativo rápido (ajuda à decisão)

| Critério                  | **CMP**                     | **FIFO**                           |
| ------------------------- | --------------------------- | ---------------------------------- |
| Estabilidade do custo     | Alta (suaviza oscilações)   | Sensível a ordem de entrada        |
| Complexidade              | Baixa no perpétuo           | Moderada (controle por camadas)    |
| Aderência a IFRS/BR-GAAP  | Total                       | Total                              |
| Rastreio de validade/lote | Indireto (requer metadados) | Natural (camadas cronológicas)     |
| Cenário ideal             | Itens fungíveis e alto giro | Itens com rastreio físico/validade |

---

### 12) Pipeline visual (EL-01 em 5 linhas)

**Entrada** → compõe custo (aquisição/transformação) → **recalcula CMP**
**Saída** → aplica **CM vigente** → **CMV**
**VRL** → write-down/reversões quando necessário
**Conciliação** → inventário físico + KPIs
**Fechamento** → política e fórmula divulgadas

se quiser, eu **condenso em 1–2 páginas para impressão** (linguagem 100% executiva) ou **adapto a terminologia** para o board (com foco em riscos, compliance e ROI).




```Ruby
[ CAPÍTULO I — CABEÇALHO & METADADOS (IDENTIDADE) ]
│  Identifica o documento e oferece ações globais de leitura/impressão.
│  ├─ Art.1  ✔ Título claro e objetivo (Média Ponderada — Exemplo do Vídeo)
│  ├─ Art.2  ✔ Controles globais (Expandir / Recolher / Imprimir)
│  ├─ Art.3  ✔ Indicação de frescor (timestamp / last-update)
│  ├─ Art.4  ✔ Organização visual para navegação rápida (header / ações)
│  └─ Art.5  ✔ Informação mínima para confiança antes da leitura
│
├──▶ (Propósito) → Contextualizar o leitor e disponibilizar operações primárias do documento.
│

[ CAPÍTULO II — FERRAMENTAS DE NAVEGAÇÃO & FILTRO (ACESSO RÁPIDO) ]
│  Elementos para localizar seções e controlar a exibição do conteúdo.
│  ├─ Art.1  ✔ Campo de busca com sugestão contextual (pesquisar termos técnicos)
│  ├─ Art.2  ✔ Botão limpar / reset da busca
│  ├─ Art.3  ✔ Chips / âncoras para pular às seções-chave (Resumo, Regras, Simulador, Tabela, Doc)
│  ├─ Art.4  ✔ Estado de feedback para “nenhum resultado”
│  └─ Art.5  ✔ Acessibilidade e indicação de foco para navegação por teclado
│
├──▶ (Princípio) → Reduzir o tempo até o trecho desejado; facilitar reprodução do exemplo.
│

[ CAPÍTULO III — RESUMO CONCEITUAL (IDEIA CENTRAL) ]
│  Uma frase ou bloco curto que define a essência do tópico.
│  ├─ Art.1  ✔ Definição sucinta (o que é média ponderada)
│  ├─ Art.2  ✔ Qual é a utilidade prática (cálculo do custo médio para saídas)
│  ├─ Art.3  ✔ Público-alvo do documento (estoque / contabilidade / auditoria)
│  ├─ Art.4  ✔ Link rápido para “ir ao simulador” ou exemplos práticos
│  └─ Art.5  ✔ Notas de leitura (quando usar CM vs outras metodologias)
│
├──▶ (Síntese) → Comunicar em 1 frase o propósito funcional do conteúdo.
│

[ CAPÍTULO IV — REGRAS DE CÁLCULO (NÚCLEO TÉCNICO) ]
│  Fórmulas e passos formais que definem o comportamento do método.
│  ├─ Art.1  ✔ Regra de entrada (q, cu → atualização Q', V', CM')
│  ├─ Art.2  ✔ Regra de saída (uso do CM vigente; atualização de Q e V)
│  ├─ Art.3  ✔ Condições e exceções (Q' = 0 → CM = 0; tratamento de divisão)
│  ├─ Art.4  ✔ Notação e variáveis (Q, V, CM, q, cu) claramente definidas
│  └─ Art.5  ✔ Comportamento numérico (arredondamento, precisão e modo de apresentação)
│
├──▶ (Princípio) → Garantir que a operacionalização do cálculo seja inequívoca.
│

[ CAPÍTULO V — SIMULADOR (INTERAÇÃO & PROCESSO) ]
│  Ferramenta prática para aplicar transações e observar impacto passo-a-passo.
│  ├─ Art.1  ✔ Entrada de parâmetros iniciais (Q0, CM0, casas decimais, modo)
│  ├─ Art.2  ✔ Formulário de transação (data, tipo, q, cu, cliente/tipo)
│  ├─ Art.3  ✔ Modos de exibição (preciso vs aproximação visível)
│  ├─ Art.4  ✔ Log de movimentos (tabela sequencial com Q antes/depois, CM antes/depois)
│  └─ Art.5  ✔ Ações do simulador (Adicionar, Limpar, Carregar exemplo do vídeo, Mostrar log)
│
├──▶ (Objetivo) → Permitir experimentação segura e reprodução passo-a-passo do exemplo do vídeo.
│

[ CAPÍTULO VI — OUTPUTS & MÉTRICAS (LEITURA IMEDIATA) ]
│  Valores derivados exibidos ao usuário após aplicação de transações.
│  ├─ Art.1  ✔ Quantidade atual (Q) – saldo visível
│  ├─ Art.2  ✔ Custo médio vigente (CM)
│  ├─ Art.3  ✔ Valor do estoque (V) = Q × CM (ou modo visível)
│  ├─ Art.4  ✔ CMV acumulado (sum of outputs) para auditoria
│  └─ Art.5  ✔ Widgets de resumo rápido (Entradas, Saídas, Saldo final)
│
├──▶ (Resultado) → Fornecer números prontos para análise e verificação.
│

[ CAPÍTULO VII — TABELA CMP (REPRODUÇÃO DO QUADRO) ]
│  Representação tabular passo-a-passo conforme o formato do vídeo.
│  ├─ Art.1  ✔ Cabeçalho multi-nível (Entradas / Saídas / Saldo) claramente rotulado
│  ├─ Art.2  ✔ Linhas por período/movimento com colunas Q, CU, Total e saldo
│  ├─ Art.3  ✔ Rodapé com totais consolidados (CMV, Estoque final)
│  ├─ Art.4  ✔ Controles de visualização (decimais, modo, saldo por linha)
│  └─ Art.5  ✔ Exportação/Impressão da tabela para evidência (audit trail)
│
├──▶ (Valor) → Permitir rastreabilidade linha-a-linha da formação do CM.
│

[ CAPÍTULO VIII — WIDGETS DE SOMAS & SÍNTESE (PANORAMA) ]
│  Elementos que sumarizam totais e dão visão rápida do impacto agregado.
│  ├─ Art.1  ✔ Entradas acumuladas (q, valor)
│  ├─ Art.2  ✔ Saídas acumuladas (q, CMV)
│  ├─ Art.3  ✔ Saldo final (q, CM, V)
│  ├─ Art.4  ✔ Indicadores de integridade (somas batem vs tabela)
│  └─ Art.5  ✔ Visual destaque para variações significativas
│
├──▶ (Função) → Resumo executivo dos movimentos para decisões rápidas.
│

[ CAPÍTULO IX — AÇÕES DO USUÁRIO (INTERATIVAS) ]
│  Operações disponíveis a partir do documento/simulador.
│  ├─ Art.1  ✔ Expandir / Recolher (navegação de leitura)
│  ├─ Art.2  ✔ Imprimir / Exportar (PDF, CSV do CMP)
│  ├─ Art.3  ✔ Carregar exemplo (replay do vídeo)
│  ├─ Art.4  ✔ Gerar resumo / mostrar log (evidência passo-a-passo)
│  └─ Art.5  ✔ Persistir cenário (salvar preferências ou histórico)
│
├──▶ (Ação) → Transformar observação em prova, arquivo ou nova simulação.
│

[ CAPÍTULO X — FLUXO OPERACIONAL (PIPELINE) ]
│  Sequência lógica que transforma entrada em resultado e evidência.
│  ├─ Art.1  ✔ Leitor abre documento → verifica título e atualização
│  ├─ Art.2  ✔ Usuário busca / navega até seção desejada (Resumo/Regras/Simulador)
│  ├─ Art.3  ✔ Define parâmetros iniciais e insere transações no simulador
│  ├─ Art.4  ✔ Simulador aplica regras → atualiza log, tabela CMP e widgets
│  ├─ Art.5  ✔ Usuário valida resultados, exporta ou imprime evidências
│  └─ Art.6  ✔ Itera (novo cenário) ou salva como caso de teste
│
├──▶ (Síntese) → ABRIR → NAVEGAR → SIMULAR → AGREGAR → VALIDAR → EXPORTAR/ITERAR
│

[ CAPÍTULO XI — GOVERNANÇA, VERIFICAÇÃO E AUDITABILIDADE (CONTRATO) ]
│  Regras que asseguram confiabilidade e reprodutibilidade do cálculo.
│  ├─ Art.1  ✔ Documentar versão do exemplo (ex.: vídeo referência + timestamp)
│  ├─ Art.2  ✔ Registrar precisão e modo de arredondamento usados na simulação
│  ├─ Art.3  ✔ Validar consistência (somas do widget vs totais da tabela)
│  ├─ Art.4  ✔ Log de transações com identificador e possibilidade de exportação
│  └─ Art.5  ✔ Testes de regressão para modos (preciso vs aproximação visível)
│
├──▶ (Garantia) → Rastreabilidade completa para fins contábeis e de auditoria.
│

├──▶ (Fluxo Geral) → CABEÇALHO → FERRAMENTAS → (RESUMO / REGRAS) → SIMULAR → CMP TABLE & WIDGETS → AÇÕES → GOVERNANÇA
│



```

## `Módulo__Diretoria:` Custo Por Absorção


### 🧮 Custeio por Absorção — **Classificação de Custos**

**Escopo:** definir *o que entra no custo*, *como classificar* (Direto×Indireto; Fixo×Variável) e *como ratear* os **CIF** por **base coerente**, respeitando **capacidade normal**. Estoques seguem **custo ou VRL, o menor**. ([IFRS Foundation][1])


### Referências

* **IAS 2 — Inventories**: custo inclui aquisição e conversão; **CIF fixos** pela **capacidade normal**; SG&A fora do custo; **VRL**. ([IFRS Foundation][2])
* **CPC 16 (R1) — Estoques**: tradução/local BR-GAAP das mesmas diretrizes (capacidade normal, custo/VRL). ([CVM][5])
* **Vídeo base didática**: *Aula 8 — Custeio por Absorção* (Prof. Daniel Santana). ([YouTube][4])

---


- 1: [ifrs.org](https://www.ifrs.org/issued-standards/list-of-standards/ias-2-inventories/?utm_source=chatgpt.com)
- 2: [IAS 2 Inventories](https://www.ifrs.org/content/dam/ifrs/publications/pdf-standards/english/2022/issued/part-a/ias-2-inventories.pdf?bypass=on&utm_source=chatgpt.com)
    - [IAS 2 Inventories summary - applies in 2025](https://youtu.be/hYmeQYuhS2E)
- 3: [iasplus.com](https://www.iasplus.com/en/standards/ias/ias2?utm_source=chatgpt.com)
- 4: [CLASS 8 | ABSORPTION COSTING"](https://www.youtube.com/watch?v=GI5F9k8KB8g&utm_source=chatgpt.com)
- 5: [CPC 16 (R1) - Estoques - CVM"](https://conteudo.cvm.gov.br/export/sites/cvm/menu/regulados/normascontabeis/cpc/CPC_16_R1_rev_12.pdf?utm_source=chatgpt.com)



### 1) Resumo executivo

No **custeio por absorção**, **todos** os custos de fabricação **diretos e indiretos (fixos e variáveis)** são atribuídos aos produtos: **MP**, **MOD** e **CIF** (energia, aluguel fábrica, depreciação, supervisão etc.). **Despesas de venda/administrativas ficam fora do custo do estoque**. Os **CIF fixos** são alocados com base na **capacidade normal**; variações relevantes (sub/ superabsorção) impactam o resultado. ([IFRS Foundation][2])


### 2) Pipeline Linear (EL-01) — do dado à decisão

```Ruby
[0. Preparação]
    • Separar Custo × Despesa (vendas/adm fora do estoque).
    • Levantar volumes (unid., horas MOD, horas-máquina, consumo MP).
[1. Classificação]
    • Para cada item: Direto ou Indireto (CIF); Fixo ou Variável.
[2. Bases de rateio (CIF)]
    • Escolher driver aderente: Unidades | Horas MOD | Horas-máquina | Valor de MP.
[3. Capacidade normal]
    • Definir capacidade normal do parque fabril (base das taxas de CIF fixos).
[4. Taxas de absorção]
    • CIF fixos ÷ capacidade normal  → taxa fixa.
    • CIF variáveis ÷ driver real     → taxa variável.
[5. Apropriação aos produtos]
    • Diretos por medição; CIF = taxa × consumo de driver por produto.
[6. Custo total e unitário]
    • Custo total = Diretos + CIF alocados.
    • Custo unitário = Custo total ÷ Unidades produzidas.
[7. Checagens & VRL]
    • Estoques por custo ou VRL (o menor); SG&A fora do custo.
[8. KPIs & Governança]
    • %CIF no custo; taxa de absorção; sub/superabsorção; margem vs. preço.
```

**Notas-chave**
• **Capacidade normal** é a média sustentável ao longo de períodos; evita distorção da taxa de CIF fixos. ([IFRS Foundation][2])
• Estoques **não** incluem despesas de venda/adm; mensurar por **custo ou VRL (menor)**. ([IFRS Foundation][1])


### 3) Regras operacionais (direto ao ponto)

* **Diretos**: apropriados por medição (ordem, BOM/rota, apontamentos).
* **Indiretos (CIF)**: **selecionar driver que represente consumo**:
  • **Energia & depreciação de máquinas** → **horas-máquina**;
  • **Supervisão & utilidades de processo** → **horas MOD**;
  • **Aluguel da fábrica** → **m² produtivos** ou **horas-máquina**;
  • **Consumíveis proporcionais à MP** → **valor de MP**;
  • **Alto mix/complexidade**: considerar granularidade por **centro de custo**.
* **Taxa de CIF fixos**: calcular com **capacidade normal**; **não inflar** a taxa quando a produção cai (diferença vira despesa). ([IFRS Foundation][2])
* **Fechamento**: divulgar política de custo, drivers, e critérios de VRL nas notas (quando aplicável). ([IAS Plus][3])


### 4) Exemplo prático (resumo para mesa) — **Pop** & **Top**

**Passos:** (i) separar custos de **produção** das **despesas**; (ii) classificar **Diretos × Indiretos**; (iii) **alocar diretos** por consumo; (iv) **ratear CIF** por **driver escolhido** (ex.: **unidades** ou **horas**); (v) somar **Diretos + CIF** por produto; (vi) dividir por **unidades produzidas**. *(Base didática alinhada ao vídeo de referência.)* ([YouTube][4])

**Exemplo (esquemático):**

* **Diretos** (Pop/Top): MP e MOD por consumo.
* **CIF** (aluguel fábrica, energia, supervisão): escolher **uma base** (p.ex. unidades) e **aplicar taxa** = CIF ÷ total do driver; **produto absorve** CIF conforme seu uso do driver.
* **Resultado**: **Custo total** por produto = Diretos + CIF alocados; **Custo unitário** = total ÷ unidades.


### 5) Tabela de etapas — quem faz o quê (governança)

| Etapa          | Objetivo                         | Entradas                 | Saída                    | Dono                 | Controle                                              |
| -------------- | -------------------------------- | ------------------------ | ------------------------ | -------------------- | ----------------------------------------------------- |
| 0. Preparar    | Separar custos vs. despesas      | Plano de contas, centros | Lista “custos elegíveis” | Contábil/Custos      | Auditoria SG&A fora do estoque ([IFRS Foundation][1]) |
| 1. Classificar | DI/FV por item                   | Itens de custo           | Mapa DI/FV               | Custos/PCP           | Revisão gerencial                                     |
| 2. Driver CIF  | Escolher base de rateio          | Unid./Horas/Valor MP     | Drivers definidos        | Custos/Operação      | Justificativa técnica                                 |
| 3. Capacidade  | Definir **capacidade normal**    | Eng./PCP                 | Base das taxas           | PCP/Custos           | Ato formal e revisão ([CVM][5])                       |
| 4. Taxas       | Calcular taxas (fixos/variáveis) | CIF e drivers            | Tabelas de taxa          | Custos               | Recalcular por período                                |
| 5. Apropriar   | Alocar por produto               | Consumos por driver      | Custos por SKU           | Custos/Controladoria | Trilha por ordem                                      |
| 6. Fechar      | Custo unit. & VRL                | Unidades/VRL             | Custo & provisões        | Contábil             | Política de VRL ([IFRS Foundation][1])                |

---

### 6) Riscos & controles (para a diretoria)

* **Driver inadequado** → custo distorcido por produto → **política de seleção de drivers** + **testes de sensibilidade**.
* **Capacidade mal estimada** → taxa de CIF fixos errada → **revisar capacidade normal** periodicamente. ([IFRS Foundation][2])
* **SG&A no estoque** → margem artificial → **checagem de contas** (vendas/adm fora do custo). ([IAS Plus][3])
* **Obsolescência/queda de preço** → estoque acima do realizável → **teste de VRL e provisão**. ([IFRS Foundation][1])


### 7) KPIs essenciais

**%CIF no custo**, **Taxa de absorção** (fixa/variável), **Sub/superabsorção (R$)**, **Custo unitário por SKU**, **Margem bruta vs. preço**, **Acurácia de classificação (DI/FV)**. *(Use os cartões da sua tela para leitura rápida.)*



```Ruby
[ CAPÍTULO I — IDENTIDADE & TEMA (CONTEXTO DA PÁGINA) ]
│  Define o propósito e a camada visual/semântica da documentação.
│  ├─ Art.1  ✔ Página corrente (CURRENT_PAGE) identifica o módulo ativo.
│  ├─ Art.2  ✔ Tema com paleta (light/dark) e tokens visuais (background/surface/título/accent).
│  ├─ Art.3  ✔ Layout-base: container, cabeçalho, corpo em grid e painéis.
│  ├─ Art.4  ✔ Tipografia e contraste asseguram leitura e hierarquia.
│  └─ Art.5  ✔ Sombra/bordas para profundidade sem ruído visual.
│
├──▶ (Propósito) → Sustentar legibilidade e identidade semântica da tela.
│
[ CAPÍTULO II — OBJETIVO & USO (ORIENTAÇÃO INICIAL) ]
│  Explica o que o usuário fará e como avançar.
│  ├─ Art.1  ✔ Classificar itens de custo (Direto×Indireto; Fixo×Variável).
│  ├─ Art.2  ✔ Definir base de rateio para Indiretos (Unidades/Horas/Valor MP).
│  ├─ Art.3  ✔ Utilizar ações: Salvar/Carregar/Limpar/Exportar JSON.
│  ├─ Art.4  ✔ Compreender que KPIs resumem a classificação atual.
│  └─ Art.5  ✔ Preparar terreno para a etapa seguinte (Simulação).
│
├──▶ (Diretriz) → “Classifique agora para simular depois”.
│
[ CAPÍTULO III — CONCEITOS ESSENCIAIS (DICIONÁRIO FUNCIONAL) ]
│  Define os eixos conceituais que sustentam a classificação.
│  ├─ Art.1  ✔ Custos Diretos — atribuíveis ao produto sem rateio.
│  ├─ Art.2  ✔ Custos Indiretos (CIF) — exigem base de rateio.
│  ├─ Art.3  ✔ Fixo — não varia no curto prazo com volume.
│  ├─ Art.4  ✔ Variável — acompanha o volume produzido.
│  └─ Art.5  ✔ Bases de rateio — Unidades, Horas MOD, Horas-máquina, Valor de MP.
│
├──▶ (Princípio) → Clareza conceitual antes da ação operacional.
│

[ CAPÍTULO IV — FÓRMULAS BÁSICAS (NUCLEAÇÃO DO CÁLCULO) ]
│  Regras que serão usadas adiante (na simulação).
│  ├─ Art.1  ✔ Custo Total do Produto = Diretos + Quota de Indiretos.
│  ├─ Art.2  ✔ Custo Unitário = Custo Total ÷ Quantidade Produzida.
│  ├─ Art.3  ✔ Quota dos Indiretos depende da base de rateio adotada.
│  ├─ Art.4  ✔ Consistência: mesmo critério do início ao fim.
│  └─ Art.5  ✔ Observabilidade: documentar escolhas de base.
│
├──▶ (Resultado) → Cálculo reprodutível e auditável.
│
[ CAPÍTULO V — CLASSIFICAÇÃO OPERACIONAL (TABELA DE TRABALHO) ]
│  Estrutura onde o usuário efetivamente classifica cada item.
│  ├─ Art.1  ✔ Campos: Item, Centro de Custo, Direto×Indireto, Fixo×Variável, Base, Observações.
│  ├─ Art.2  ✔ Controles binários por grupo (radios) para DI/FV.
│  ├─ Art.3  ✔ Seleção de base apenas quando Indireto.
│  ├─ Art.4  ✔ Observações como trilha de auditoria (justificativas).
│  └─ Art.5  ✔ Acessibilidade de foco e navegação por teclado.
│
├──▶ (Princípio) → Capturar decisão contábil no nível do item.
│
[ CAPÍTULO VI — AÇÕES & FLUXO DO OPERADOR (COMANDOS) ]
│  Operações sequenciais que suportam o preenchimento e persistência.
│  ├─ Art.1  ✔ Adicionar linha para novos itens.
│  ├─ Art.2  ✔ Carregar classificação salva (estado anterior).
│  ├─ Art.3  ✔ Salvar classificação atual (persistência local).
│  ├─ Art.4  ✔ Limpar linhas (reset controlado).
│  └─ Art.5  ✔ Exportar JSON (evidência/portabilidade).
│
├──▶ (Ação) → Habilitar iterações rápidas mantendo rastreabilidade.
│
[ CAPÍTULO VII — KPIs & CHECKLIST (SÍNTESE & QUALIDADE) ]
│  Indicadores e verificação rápida da consistência da classificação.
│  ├─ Art.1  ✔ KPIs: Diretos•Fixos, Diretos•Variáveis, Indiretos•Fixos, Indiretos•Variáveis.
│  ├─ Art.2  ✔ Checklist: separar custos de despesas; DI/FV corretos; base coerente nos Indiretos.
│  ├─ Art.3  ✔ Observações registradas para itens sensíveis.
│  ├─ Art.4  ✔ Feedback visual claro (legibilidade e contraste).
│  └─ Art.5  ✔ Responsividade: leitura estável em diferentes telas.
│
├──▶ (Objetivo) → Confiabilidade antes de seguir para simular.
[ CAPÍTULO VIII — PERSISTÊNCIA & SEED (ESTADO INICIAL E SALVAMENTO) ]
│  Como o estado nasce e é mantido durante/entre sessões.
│  ├─ Art.1  ✔ SEED inicial: lista de itens exemplificativos por área.
│  ├─ Art.2  ✔ Chave de armazenamento local (versão e updatedAt).
│  ├─ Art.3  ✔ Carregar salvo > senão, iniciar pelo SEED.
│  ├─ Art.4  ✔ Exportação JSON preserva versão e conteúdo.
│  └─ Art.5  ✔ Import/Load recompõe tabela e recalcula KPIs.
│
├──▶ (Garantia) → Continuidade do trabalho e reprodutibilidade.
│
[ CAPÍTULO IX — UX, ACESSIBILIDADE & LAYOUT (ENTREGÁVEL VISUAL) ]
│  Convenções de apresentação e navegação para reduzir atrito.
│  ├─ Art.1  ✔ Painéis com títulos claros e seções numeradas.
│  ├─ Art.2  ✔ Grids (2×/3×) com fallback responsivo (mobile-first).
│  ├─ Art.3  ✔ Callouts para regras/fórmulas; tabela operacional com foco.
│  ├─ Art.4  ✔ Botões sólidos/ghost com hierarquia de ação.
│  └─ Art.5  ✔ Estados: padrão, hover, ativo e disabled consistentes.
│
├──▶ (Experiência) → Clareza de leitura e fluxo previsível.
│
[ CAPÍTULO X — FLUXO OPERACIONAL (PIPELINE) ]
│  Sequência linear do começo ao fim da atividade.
│  ├─ Art.1  ✔ Ler orientação → entender conceitos/fórmulas.
│  ├─ Art.2  ✔ Preencher/ajustar classificação item a item.
│  ├─ Art.3  ✔ Conferir KPIs & checklist; corrigir inconsistências.
│  ├─ Art.4  ✔ Salvar/Exportar → criar evidência e portabilidade.
│  └─ Art.5  ✔ Avançar para “Simulação” com a base classificada.
│
├──▶ (Síntese) → ORIENTAR → CLASSIFICAR → VALIDAR → PERSISTIR → SIMULAR.
│
[ CAPÍTULO XI — GOVERNANÇA & AUDITABILIDADE (CONTROLE) ]
│  Regras para confiança, trilha e manutenção do processo.
│  ├─ Art.1  ✔ Versionamento do estado (version, updatedAt).
│  ├─ Art.2  ✔ Observações por item para contexto de auditoria.
│  ├─ Art.3  ✔ Critérios documentados de base de rateio.
│  ├─ Art.4  ✔ Consistência entre KPIs e linhas da tabela.
│  └─ Art.5  ✔ Export JSON como prova e insumo para revisões.
│
├──▶ (Contrato) → Transparência e reprodutibilidade das decisões.
│
[ CAPÍTULO XII — PRÓXIMA ETAPA (SIMULAÇÃO) ]
│  Conecta a classificação com o cálculo por absorção.
│  ├─ Art.1  ✔ Utilizar a base classificada para distribuir CIFs.
│  ├─ Art.2  ✔ Calcular custo total e unitário por produto.
│  ├─ Art.3  ✔ Gerar relatórios para tomada de decisão.
│  ├─ Art.4  ✔ Iterar ajuste de classificação se necessário.
│  └─ Art.5  ✔ Encadear com módulos seguintes (custos/precificação).
│
├──▶ (Encaminhamento) → Da classificação para o cálculo e decisão.

```



## `Módulo__Diretoria:` Relatório Rapído de Tudo o que entrou e saiu

```Ruby
[ CAPÍTULO I — IDENTIDADE & CONTEXTO (PÁGINA ATIVA) ]
│  Define o módulo e o escopo do comparativo Entradas × Saídas.
│  ├─ Art.1  ✔ Página corrente (CURRENT_PAGE) identifica a rota funcional
│  ├─ Art.2  ✔ Container raiz do app (host do dashboard)
│  ├─ Art.3  ✔ Endpoints declarados (data-api-entradas / data-api-saidas)
│  ├─ Art.4  ✔ Tema claro/escuro unificado por tokens (cores, bordas, sombra)
│  └─ Art.5  ✔ Tipografia e densidade padronizadas
│
├──▶ (Propósito) → Situar o usuário e padronizar a experiência visual.
│
[ CAPÍTULO II — CABEÇALHO & METADADOS (ORIENTAÇÃO) ]
│  Identifica o comparativo e informa frescor.
│  ├─ Art.1  ✔ Título do dashboard (Entradas × Saídas — Comparativo)
│  ├─ Art.2  ✔ Indicador de última atualização (last-sync)
│  ├─ Art.3  ✔ Contexto de período (a partir dos filtros)
│  ├─ Art.4  ✔ Acesso rápido a exportações/visualizações
│  └─ Art.5  ✔ Consistência com o restante do sistema
│
├──▶ (Propósito) → Confiança imediata sobre “o que” e “quão recente”.
│
[ CAPÍTULO III — FILTROS & PRESETS (PONTO DE ENTRADA) ]
│  Parametrizam datas, agregação e tipo de visualização.
│  ├─ Art.1  ✔ Presets de período: Hoje / 7d / 30d / Mês atual / Entre datas
│  ├─ Art.2  ✔ Intervalo customizável (De / Até) quando necessário
│  ├─ Art.3  ✔ Agregação: Diário / Semanal / Mensal
│  ├─ Art.4  ✔ Visualização: Linhas / Barras
│  └─ Art.5  ✔ Ações: Aplicar, alternar Modo compacto, Exportar séries/itens
│
├──▶ (Princípio) → Definir o recorte e a granularidade antes de comparar.
│
[ CAPÍTULO IV — ESTADO & PREFERÊNCIAS (MEMÓRIA DO USUÁRIO) ]
│  Mantém escolhas entre sessões e controla densidade.
│  ├─ Art.1  ✔ Estado de presets, agregação, range custom, densidade, view
│  ├─ Art.2  ✔ Persistência local (prefs key) para reabrir no último contexto
│  ├─ Art.3  ✔ Alternância de densidade (padrão × compacto)
│  ├─ Art.4  ✔ Sincronização dos inputs com o estado corrente
│  └─ Art.5  ✔ Observador de tamanho p/ redesenhar gráficos (responsividade)
│
├──▶ (Resultado) → Experiência contínua e adaptável.
│
[ CAPÍTULO V — COLETA & NORMALIZAÇÃO (DADOS DE E/S) ]
│  Obtenção de dados e fallback confiável.
│  ├─ Art.1  ✔ Fetch de Entradas (API_E) e Saídas (API_S)
│  ├─ Art.2  ✔ Normalização de campos: id, date, label, qty, val
│  ├─ Art.3  ✔ Fallback com dados demo em caso de erro
│  ├─ Art.4  ✔ Validação de formato (array esperado)
│  └─ Art.5  ✔ Timestamp de sincronização para rastreabilidade
│
├──▶ (Garantia) → Sempre haverá dados válidos para comparar.
│
[ CAPÍTULO VI — AGREGAÇÃO & CHAVES TEMPORAIS (SÉRIES) ]
│  Constrói buckets e séries comparáveis.
│  ├─ Art.1  ✔ Buckets por Dia / Semana (início ISO) / Mês
│  ├─ Art.2  ✔ Merge de chaves: union ordenada de E e S
│  ├─ Art.3  ✔ Séries paralelas: valores agregados para E e S
│  ├─ Art.4  ✔ Metadados por bucket (val/qty/count) para análises
│  └─ Art.5  ✔ Preparação para KPIs e gráficos
│
├──▶ (Princípio) → Comparar “como com como” no mesmo eixo temporal.
│
[ CAPÍTULO VII — KPIs (SÍNTESE NUMÉRICA) ]
│  Leitura imediata do período filtrado.
│  ├─ Art.1  ✔ Total de Entradas (valor)
│  ├─ Art.2  ✔ Total de Saídas (valor)
│  ├─ Art.3  ✔ Saldo (E − S) com indicação Positivo/Negativo
│  ├─ Art.4  ✔ Razão (E / S) para avaliar cobertura
│  └─ Art.5  ✔ Contagem de itens por lado (E/S) como contexto
│
├──▶ (Objetivo) → Ver “o tamanho da diferença” sem abrir detalhes.
│
[ CAPÍTULO VIII — VISUALIZAÇÃO PRINCIPAL & SECUNDÁRIA (TRADUÇÃO) ]
│  Facilita leitura de tendência e distribuição.
│  ├─ Art.1  ✔ Gráfico principal: Linhas (ou Barras) com legendas E/S
│  ├─ Art.2  ✔ Gráfico lateral: amostra recente (últimos 6 buckets, barras)
│  ├─ Art.3  ✔ Grid/escala consistentes para comparação clara
│  ├─ Art.4  ✔ Redesenho on-resize (responsivo)
│  └─ Art.5  ✔ Alternância de visualização sem perder o estado
│
├──▶ (Valor) → Padrões, picos e inversões ficam evidentes.
│
[ CAPÍTULO IX — RASTREABILIDADE (LISTAS & MODAL) ]
│  Evidência item a item e inspeção pontual.
│  ├─ Art.1  ✔ Listas de últimos lançamentos para Entradas e Saídas
│  ├─ Art.2  ✔ Visual “Ver” por item (modal com detalhes)
│  ├─ Art.3  ✔ “Ver tudo” (consolidação filtrada em modal)
│  ├─ Art.4  ✔ Ordenação temporal para revisão rápida
│  └─ Art.5  ✔ ID + label + data + qty + valor como trilha mínima
│
├──▶ (Diagnóstico) → Do agregado ao particular sem fricção.
│
[ CAPÍTULO X — INSIGHTS & ALERTAS (SINALIZAÇÃO) ]
│  Heurísticas de saúde do período.
│  ├─ Art.1  ✔ Sem anomalias (OK) quando E ≥ S
│  ├─ Art.2  ✔ WARN se S > E (atenção)
│  ├─ Art.3  ✔ CRIT se S > E em >15% (risco elevado)
│  ├─ Art.4  ✔ Mensagem contextual para próxima ação
│  └─ Art.5  ✔ Classe visual por severidade (ok/warn/crit)
│
├──▶ (Ação) → Priorizar verificação de caixa/estoque conforme severidade.
│
[ CAPÍTULO XI — EXPORTAÇÃO (EVIDÊNCIAS) ]
│  Saídas para auditoria e análise externa.
│  ├─ Art.1  ✔ Exportar séries agregadas (CSV: bucket, entradas, saídas)
│  ├─ Art.2  ✔ Exportar itens filtrados (CSV: tipo, id, data, label, qty, val)
│  ├─ Art.3  ✔ Identificação clara de arquivos (nomenclatura)
│  ├─ Art.4  ✔ Compatibilidade com planilhas
│  └─ Art.5  ✔ Preservação de separadores/aspas
│
├──▶ (Prova) → Reprodutibilidade fora do sistema.
│
[ CAPÍTULO XII — RESPONSIVIDADE & DENSIDADE (USABILIDADE) ]
│  Ajustes de layout e legibilidade.
│  ├─ Art.1  ✔ Grade principal colapsa em 1 coluna ≤1100px
│  ├─ Art.2  ✔ KPIs em 2×1 colunas em telas menores
│  ├─ Art.3  ✔ Modo compacto: altura/espacamento reduzidos
│  ├─ Art.4  ✔ Tipos e chips adaptáveis (toques)
│  └─ Art.5  ✔ Contrast ratio preservado no tema escuro
│
├──▶ (Experiência) → Leitura confortável em qualquer dispositivo.
│
[ CAPÍTULO XIII — FLUXO OPERACIONAL (PIPELINE) ]
│  Sequência linear do uso ao insight.
│  ├─ Art.1  ✔ Usuário escolhe preset ou datas (e agregação/visualização)
│  ├─ Art.2  ✔ Sistema carrega dados (API E/S) ou usa fallback
│  ├─ Art.3  ✔ Filtra pelo range → agrega por bucket → monta séries
│  ├─ Art.4  ✔ Calcula KPIs → desenha gráficos → atualiza listas
│  ├─ Art.5  ✔ Emite insights/alertas → permite exportar/inspecionar
│  └─ Art.6  ✔ Persiste preferências → usuário itera (novo filtro)
│
├──▶ (Síntese) → FILTRAR → COLETAR → AGREGAR → RENDERIZAR → DIAGNOSTICAR → EXPORTAR/ITERAR.
│
[ CAPÍTULO XIV — GOVERNANÇA & OBSERVABILIDADE (CONFIABILIDADE) ]
│  Garantias de qualidade e rastreio.
│  ├─ Art.1  ✔ Registro de última sincronização (last-sync)
│  ├─ Art.2  ✔ Consistência entre KPIs, séries e listas
│  ├─ Art.3  ✔ Tratamento de erro/fallback sem travar a análise
│  ├─ Art.4  ✔ Logs mínimos de ação (presets, export, ver tudo)
│  └─ Art.5  ✔ Documentação de limites (demo vs produção)
│
├──▶ (Contrato) → Transparência sobre origem, frescor e limites dos dados.
│
├──▶ (Fluxo Geral) → IDENTIDADE → FILTROS → DADOS → AGREGAÇÃO → KPIs/GRÁFICOS → RASTREIO/INSIGHTS → EXPORTAÇÃO → GOVERNANÇA
│

```





