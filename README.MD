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



## `Módulo__Comercial:` Onde o ciclo do Contrato Nasce


```Ruby
[ CAPÍTULO I — IDENTIDADE & ESCOPO ]
│  Quadro Kanban para gestão de contratos de locação, focado em operação diária e visão por status.
│  ├─ Art.1  ✔ Módulo: “Kanban — Contratos de Locação”
│  ├─ Art.2  ✔ Unidades de tempo: meses (status e prazos)
│  ├─ Art.3  ✔ Clientes e tipos de equipamentos visíveis no cabeçalho
│  └─ Art.4  ✔ Pág. preparada para reunião: leitura rápida e ações diretas
│
├──▶ (Propósito) → Centralizar o ciclo de vida dos contratos em listas (estilo Trello) com filtros e exportação.
│

[ CAPÍTULO II — COLUNAS (LISTAS) DO BOARD ]
│  Estrutura operacional do funil de contratos.
│  ├─ Art.1  ✔ Propostas — negociações em curso
│  ├─ Art.2  ✔ Ativos — contratos vigentes
│  ├─ Art.3  ✔ Renovação — próximos do vencimento / decisão
│  └─ Art.4  ✔ Encerrados — concluídos ou cancelados
│
├──▶ (Leitura) → Cada coluna exibe contagem dinâmica de cartões.
│

[ CAPÍTULO III — CARTÕES (RECURSOS POR CONTRATO) ]
│  O que cada cartão mostra e permite.
│  ├─ Art.1  ✔ Título: Código • Cliente (ex.: C-2407 • Fortes)
│  ├─ Art.2  ✔ Subtítulo: Equipamento(s) • período (início → fim) • local (Cidade–UF)
│  ├─ Art.3  ✔ Metadados: status em meses (Ativo / Renovação / Atrasado / Encerrado)
│  ├─ Art.4  ✔ Badges: valor mensal (R$), cliente, etiquetas (labels de cor)
│  └─ Art.5  ✔ Ação rápida: abrir/inspecionar o contrato
│
├──▶ (Rastreio) → Identificação visual imediata de valor, prazo e contexto.
│

[ CAPÍTULO IV — STATUS & PRAZOS (INTELIGÊNCIA DE PRAZO EM MESES) ]
│  Cálculo automático do status com base em início + duração.
│  ├─ Art.1  ✔ Ativo: “vence em X meses” (ou este mês)
│  ├─ Art.2  ✔ Renovação: “vence em X meses” ou “vence este mês”
│  ├─ Art.3  ✔ Atrasado: “venceu há X meses” (quando aplicável)
│  └─ Art.4  ✔ Encerrado / Cancelado: indicação de conclusão ou cancelamento
│
├──▶ (Benefício) → Priorização clara para decisão de renovação e follow-up comercial.
│

[ CAPÍTULO V — FILTROS & TOOLBAR ]
│  Foco em recorte por carteira e por frota.
│  ├─ Art.1  ✔ Filtro por cliente (dropdown populado pelos cartões)
│  ├─ Art.2  ✔ Filtro por tipo de equipamento (ex.: Módulo Habitacional, Container)
│  ├─ Art.3  ✔ Ação “Limpar filtros” (retoma visão geral)
│  └─ Art.4  ✔ Exportações rápidas (JSON / CSV) para análise externa
│
├──▶ (Objetivo) → “Ver só o que me importa” em 2 cliques.
│

[ CAPÍTULO VI — CRIAÇÃO RÁPIDA (MODAL “NOVO CONTRATO”) ]
│  Cadastre o contrato já na coluna correta.
│  ├─ Art.1  ✔ Escolha da coluna (Propostas / Ativos / Renovação / Encerrados)
│  ├─ Art.2  ✔ Campos-chave: Código, Cliente, Valor mensal, Tipo/Modelo, Qtde
│  ├─ Art.3  ✔ Prazos: Início (mês) + Duração (meses) → fim calculado
│  ├─ Art.4  ✔ Local: Cidade / UF
│  └─ Art.5  ✔ Etiquetas (tags) para segmentação e buscas futuras
│
├──▶ (Ganho) → Menos planilha; mais operação viva e padronizada.
│

[ CAPÍTULO VII — MOVIMENTAÇÃO (DRAG & DROP) ]
│  Atualização de status por arrastar entre colunas.
│  ├─ Art.1  ✔ Arraste o cartão para mudar a etapa (funil)
│  ├─ Art.2  ✔ Recalcula status e prazos ao reposicionar
│  ├─ Art.3  ✔ Mantém contadores e filtros consistentes
│  └─ Art.4  ✔ Feedback visual de área de soltar (drag-over)
│
├──▶ (Agilidade) → Gestão de pipeline fluida e colaborativa.
│

[ CAPÍTULO VIII — EXPORTAÇÃO & COMPARTILHAMENTO ]
│  Saídas para auditoria, BI e reuniões.
│  ├─ Art.1  ✔ Exportar JSON: estrutura completa do board (colunas + cartões)
│  ├─ Art.2  ✔ Exportar CSV: coluna, código, cliente, equipamento, qtde, cidade/UF, início, fim, duração, valor, status
│  ├─ Art.3  ✔ Nome de arquivo padronizado (kanban-contratos.*)
│  └─ Art.4  ✔ Compatível com planilhas e ingestão em BI
│
├──▶ (Evidência) → Facilita prestação de contas e prognósticos.
│

[ CAPÍTULO IX — PERSISTÊNCIA & BOOTSTRAP ]
│  Como os dados aparecem e permanecem.
│  ├─ Art.1  ✔ Persistência local (localStorage) — mantém o board do usuário
│  ├─ Art.2  ✔ Restauração automática ao reabrir
│  ├─ Art.3  ✔ Fallback: seed interno se não houver fonte externa
│  └─ Art.4  ✔ Opcional: carregar JSON externo (quando disponível)
│
├──▶ (Confiabilidade) → O time não perde o contexto entre sessões.
│

[ CAPÍTULO X — ACESSIBILIDADE, RESPONSIVIDADE & UX ]
│  Garantias de uso em desktop e dispositivos móveis.
│  ├─ Art.1  ✔ Colunas com cabeçalho “sticky” (título + contagem + novo)
│  ├─ Art.2  ✔ Scroll horizontal suave, cartões compactos e legíveis
│  ├─ Art.3  ✔ Temas claro/escuro; cores por severidade/estado
│  └─ Art.4  ✔ Controles de formulário com rótulos e foco bem definidos
│
├──▶ (Experiência) → Visão limpa para reunião e operação diária.
│

[ CAPÍTULO XI — KPIs IMPLÍCITOS (NA PRÓPRIA TELA) ]
│  Sinais para discussão de performance durante a reunião.
│  ├─ Art.1  ✔ Contagem por coluna (propostas, ativos, renovação, encerrados)
│  ├─ Art.2  ✔ “Vence em X meses” (pipeline de risco de churn)
│  ├─ Art.3  ✔ Valor mensal por contrato (visão rápida de receita recorrente)
│  └─ Art.4  ✔ Etiquetas/locais (mix por cliente, praça, tipo)
│
├──▶ (Leitura de gestão) → Oportunidades de renovação e gargalos ficam visíveis.
│

[ CAPÍTULO XII — FLUXO OPERACIONAL (PIPELINE) ]
│  Sequência do uso ao resultado para a reunião.
│  ├─ Art.1  ✔ Selecionar filtros (cliente / equipamento) → visão segmentada
│  ├─ Art.2  ✔ Arrastar contratos para refletir estágio real
│  ├─ Art.3  ✔ Registrar novos contratos pelo modal (dados mínimos)
│  ├─ Art.4  ✔ Conferir prazos (“vence em X meses”) e priorizar renovação
│  ├─ Art.5  ✔ Exportar CSV/JSON para ata e acompanhamento
│  └─ Art.6  ✔ Persistir estado; retomar na próxima reunião do mesmo ponto
│
├──▶ (Síntese) → FILTRAR → ATUALIZAR (DnD/novo) → PRIORIZAR → EXPORTAR → PERSISTIR.
│

[ CAPÍTULO XIII — GOVERNANÇA & AUDITABILIDADE ]
│  Controles para confiança nas informações apresentadas.
│  ├─ Art.1  ✔ Dados mínimos obrigatórios no cadastro (cliente, início, duração)
│  ├─ Art.2  ✔ Cálculo determinístico de fim e status (base mês)
│  ├─ Art.3  ✔ Exportações como “prova” do estado do pipeline
│  └─ Art.4  ✔ Padronização de labels e tipos de equipamento
│
├──▶ (Contrato) → Transparência para decisões comerciais e operacionais.
│

├──▶ (Fluxo Geral) → COLUNAS (funil) → CARTÕES (dados) → FILTROS (recorte) → DnD (atualização) → EXPORTS (evidência) → PERSISTÊNCIA (continuidade)
│

```


## `Módulo__Juridico:` Ciclo de Vída (Aprovado/Reprovado)


```RUBY
[ CAPÍTULO I — IDENTIDADE & ESCOPO (JURÍDICO) ]
│  Board Kanban estático para ciclo de contratos (wireframe — Parte 1).
│  ├─ Art.1  ✔ Página: “Jurídico — Contratos • Board Kanban”
│  ├─ Art.2  ✔ Tema compatível claro/escuro (tokens de cor e tipografia)
│  ├─ Art.3  ✔ Layout responsivo (2/4/6 colunas por breakpoint)
│  └─ Art.4  ✔ Sem JS nesta fase; foco em estrutura visual e recursos da página
│
├──▶ (Propósito) → Visão unificada do fluxo contratual para reunião e priorização.
│
[ CAPÍTULO II — ORIENTAÇÃO (HEAD & SUBTÍTULO) ]
│  Contextualiza o fluxo e como ler o board.
│  ├─ Art.1  ✔ Título com trilha “Jurídico — Contratos”
│  ├─ Art.2  ✔ Descrição: colunas = estágios; cartões = dados-chave
│  └─ Art.3  ✔ Separadores e sombras para hierarquia visual
│
├──▶ (Leitura) → Entendimento imediato do que o board cobre.
│
[ CAPÍTULO III — FILTROS & AÇÕES DE TOPO (ESTÁTICOS) ]
│  Controles presentes na página (placeholders funcionais).
│  ├─ Art.1  ✔ Busca textual (contrato/cliente/número/tag)
│  ├─ Art.2  ✔ Chips indicativos: Período, Unidade, Tipo de contrato
│  ├─ Art.3  ✔ Botão “Novo Contrato” (entrada no funil)
│  └─ Art.4  ✔ Botão “Exportar” (preparado para saída de dados)
│
├──▶ (Objetivo) → Preparar recorte e ações para a reunião, mesmo sem JS.
│
[ CAPÍTULO IV — COLUNAS DO FLUXO (CORE DO BOARD) ]
│  Esteira principal de status exibida na página.
│  ├─ Art.1  ✔ Pedidos de contrato (requests)
│  ├─ Art.2  ✔ Em elaboração (draft)
│  ├─ Art.3  ✔ Em revisão e negociação (review)
│  ├─ Art.4  ✔ Aguardando aprovação (approval)
│  ├─ Art.5  ✔ Enviado para assinatura (sent)
│  ├─ Art.6  ✔ Assinado — aguardando vigência (signed_pending)
│  └─ Art.7  ✔ Vigentes (active)
│
├──▶ (Indicadores) → Contador por coluna (badge) visível na página.
│
[ CAPÍTULO V — CARTÕES (INFORMAÇÃO DISPONÍVEL) ]
│  O que cada card entrega para discussão na reunião.
│  ├─ Art.1  ✔ Título do contrato (ex.: “SLA de suporte — Gamma”)
│  ├─ Art.2  ✔ Metadados: cliente, modelo, owner, prazo, e-sign, etc.
│  ├─ Art.3  ✔ Badges visuais: OK / Aviso / Crítico (com cores)
│  ├─ Art.4  ✔ Barra de progresso (percentual do avanço do caso)
│  └─ Art.5  ✔ Tags rápidas (risco, pendências, tipo, aprovação)
│
├──▶ (Rastreio) → O essencial para decidir próximos passos sem abrir detalhe.
│
[ CAPÍTULO VI — OPCIONAIS / BACKLOG (COLUNAS SUPLEMENTARES) ]
│  Áreas já previstas pela página para estados paralelos.
│  ├─ Art.1  ✔ Em renovação (renewal_due)
│  ├─ Art.2  ✔ Vencidos (expired)
│  ├─ Art.3  ✔ Suspensos (on_hold)
│  ├─ Art.4  ✔ Encerrados/Rescindidos (terminated)
│  ├─ Art.5  ✔ Arquivados (archived)
│  └─ Art.6  ✔ Cancelados (canceled)
│
├──▶ (Cobertura) → Históricos e exceções mapeados no próprio board.
│
[ CAPÍTULO VII — DESIGN DE USABILIDADE (O QUE A PÁGINA PROVÊ) ]
│  Recursos visuais que favorecem leitura na reunião.
│  ├─ Art.1  ✔ Head “sticky” por coluna (título + contagem)
│  ├─ Art.2  ✔ Grid fluido; cartões compactos e legíveis
│  ├─ Art.3  ✔ Hierarquia por sombras/gradientes discretos
│  └─ Art.4  ✔ Cores semânticas para risco e urgência
│
├──▶ (Experiência) → Escaneabilidade alta para decisões rápidas.
│
[ CAPÍTULO VIII — PRONTIDÃO PARA INTEGRAÇÕES (FUTURO) ]
│  A página já reserva ganchos para dados e ações.
│  ├─ Art.1  ✔ Botões “Novo” e “Exportar” prontos para ativação
│  ├─ Art.2  ✔ Chips e busca preparados para filtrar/consultar
│  └─ Art.3  ✔ Âncoras para seed JSON e JS (Parte 2/3) já previstas no HTML
│
├──▶ (Evolução) → Ativar filtros, criação e export com mínima intervenção.
│
[ CAPÍTULO IX — FLUXO OPERACIONAL (PIPELINE) ]
│  Sequência linear do uso, do topo ao resultado na reunião.
│  ├─ Art.1  ✔ Filtrar/Buscar (chips + campo de texto)
│  ├─ Art.2  ✔ Percorrer colunas centrais (requests ▸ draft ▸ review ▸ approval ▸ sent ▸ signed_pending ▸ active)
│  ├─ Art.3  ✔ Ler cartões (progresso, risco, pendências, prazos)
│  ├─ Art.4  ✔ Endereçar exceções nos opcionais (renewal_due / expired / on_hold / terminated / archived / canceled)
│  ├─ Art.5  ✔ Registrar decisão (via “Novo Contrato” ou marcação para ação)
│  └─ Art.6  ✔ Preparar material de saída (via “Exportar”)
│
├──▶ (Síntese) → FILTRAR → PERCORRER → AVALIAR → PRIORIZAR → REGISTRAR → EXPORTAR.
│
[ CAPÍTULO X — GOVERNANÇA VISUAL & PADRONIZAÇÃO ]
│  Garantias que a própria página já entrega.
│  ├─ Art.1  ✔ Consistência de componentes (lanes/cards/badges)
│  ├─ Art.2  ✔ Padrões de cor para risco (ok/warn/danger)
│  ├─ Art.3  ✔ Contagem por coluna como KPI imediato
│  └─ Art.4  ✔ Sem dependência de script para a apresentação (wireframe confiável)
│
├──▶ (Confiança) → A reunião acontece com estabilidade, mesmo “sem back-end”.
│
├──▶ (Fluxo Geral) → IDENTIDADE → FILTROS → COLUNAS → CARTÕES → OPCIONAIS → DECISÕES → EXPORT
│
```

## `Módulo__Gestão:` Integração entre Comercial e Juridico com a Fabrica/Produção


```Ruby

[ CAPÍTULO I — IDENTIDADE & ESCOPO (FÁBRICA) ]
│  Modelo de PIPELINE (EL-01) para gestão integrada de chão de fábrica.
│  ├─ Art.1  ✔ Domínios: Contratos • Lotes • Estoque (MP/PA) • 5S/Compliance • Produção • Frota
│  ├─ Art.2  ✔ Visões: Kanban por estágio + KPIs + Relatórios/Export
│  ├─ Art.3  ✔ Unidades de tempo: dia/semana/mês • Responsáveis por área
│  └─ Art.4  ✔ Preparado para reunião: leitura rápida, priorização e decisão
│
├──▶ (Propósito) → Unificar o ciclo “VENDER → PRODUZIR → ENTREGAR → SUSTENTAR” num fluxo único.
│
[ CAPÍTULO II — FILTROS & PARÂMETROS GLOBAIS ]
│  Como recortar o contexto para análise.
│  ├─ Art.1  ✔ Período (Hoje • 7d • 30d • Mês/Trimestre/Ano • Entre datas)
│  ├─ Art.2  ✔ Unidades/Fábricas • Linhas • Turnos • Clientes/Projetos
│  ├─ Art.3  ✔ Dimensões: Produto/Modelo • Lote • Contrato • Equipe
│  └─ Art.4  ✔ Exports (CSV/JSON/PDF) e densidade (padrão/compacto)
│
├──▶ (Princípio) → Primeiro filtrar, depois comparar e agir.
│
[ CAPÍTULO III — MONITORAMENTO DE CONTRATOS (PÓS-JURÍDICO) ]
│  Contratos aprovados e vigentes mapeados em funil operacional.
│  ├─ Art.1  ✔ Colunas: Vigentes → Renovação (≤60d) → Atrasados → Encerrados/Cancelados
│  ├─ Art.2  ✔ Card: Cliente • Valor mensal • Início→Fim (meses) • Local • Etiquetas
│  ├─ Art.3  ✔ Status automático: “vence em X meses” / “venceu há X”
│  └─ Art.4  ✔ Ações: renovar • reajustar • encerrar • exportar carteira
│
├──▶ (Indicador) → Receita recorrente, contratos a renovar, risco de churn.
│
[ CAPÍTULO IV — LOTES DE FABRICAÇÃO (MÓDULOS DO ZERO) ]
│  Do desenho ao FAT/Despacho.
│  ├─ Art.1  ✔ Colunas: Eng. & Liberação → Planejado → Em Produção → QA/FAT → Pronto p/ Expedição → Entregue
│  ├─ Art.2  ✔ Card: Lote • Modelo • Qtd • Prazo • BOM travado • Responsáveis • “% concluído”
│  ├─ Art.3  ✔ Gate de qualidade por etapa (checklist/pareto de falhas)
│  └─ Art.4  ✔ Bloqueios: falta de MP • capacidade/máquina • pendência de cliente
│
├──▶ (Indicador) → OTIF por lote, lead time, gargalos por célula/estação.
│
[ CAPÍTULO V — ESTOQUE (MATÉRIAS-PRIMAS) ]
│  Reposição e disponibilidade para produção.
│  ├─ Art.1  ✔ Níveis: Mín/Máx • Cobertura (dias) • Giro
│  ├─ Art.2  ✔ Alertas: Abaixo do mínimo • Itens críticos do dia • Itens sem movimentação
│  ├─ Art.3  ✔ Integração: Reservas por Lote (alocação) • MRP simplificado
│  └─ Art.4  ✔ Ações: sugerir compra • substituir item • reclassificar
│
├──▶ (Indicador) → Quebra por MP faltante evitada, compras com antecedência.
│
[ CAPÍTULO VI — ESTOQUE (PRODUTO ACABADO) ]
│  Pronto para expedição vs pedido/contrato.
│  ├─ Art.1  ✔ Mapa: PA disponível • PA reservado • PA em QA • PA despachado
│  ├─ Art.2  ✔ SLA de expedição (dias) e janelas de carregamento
│  ├─ Art.3  ✔ Rastreio: número de série • contrato/lote vinculado
│  └─ Art.4  ✔ Ações: liberar • reter • retrabalhar • expedir
│
├──▶ (Indicador) → Fill rate, atrasos de embarque, backlog de entrega.
│
[ CAPÍTULO VII — 5S & COMPLIANCE (EXTINTORES • LICENÇAS • ALVARÁS) ]
│  Rotina legal e de segurança sob controle visual.
│  ├─ Art.1  ✔ Calendário de vencimentos (mensal/trimestral/anual)
│  ├─ Art.2  ✔ Colunas: Em dia → Vence em 30d → Vence em 7d → Vencido
│  ├─ Art.3  ✔ Itens: Extintores (selo/inspeção), Licenças Ambientais, Alvarás
│  └─ Art.4  ✔ Ações: agendar inspeção • subir comprovantes • abrir não-conformidade
│
├──▶ (Indicador) → % conformidade por site/área, zero vencidos críticos.
│
[ CAPÍTULO VIII — RELATÓRIOS DE PRODUÇÃO ]
│  Produção diária/mensal com custos e insumos.
│  ├─ Art.1  ✔ KPIs: Quantidade produzida • Horas-homem • Horas-máquina
│  ├─ Art.2  ✔ Consumo: MP por lote (Q/V) • Custo médio (CMP) • Desvios vs plano
│  ├─ Art.3  ✔ Eficiência: OEE simplificado (disponibilidade • performance • qualidade)
│  └─ Art.4  ✔ Export: diário de produção • custo por unidade • relatório por turno
│
├──▶ (Indicador) → Custo real × orçado, produtividade por célula.
│
[ CAPÍTULO IX — FROTA (MONITORAMENTO) ]
│  Disponibilidade e utilização de veículos/equipamentos.
│  ├─ Art.1  ✔ Status: Disponível • Em rota • Manutenção • Pátio
│  ├─ Art.2  ✔ Metas: Utilização (%) • Km/L • Manutenção preventiva (vencimentos)
│  ├─ Art.3  ✔ Vínculos: OS/Entrega • Motorista/Equipe • Rota
│  └─ Art.4  ✔ Ações: agendar manutenção • bloquear uso • abrir ocorrência
│
├──▶ (Indicador) → Disponibilidade técnica, custo por km, aderência à preventiva.
│
[ CAPÍTULO X — VISÕES TRANSVERSAIS (PARA REUNIÃO) ]
│  Cortes que combinam módulos.
│  ├─ Art.1  ✔ “Hoje”: quebras de material, lotes críticos, entregas previstas, licenças 7d
│  ├─ Art.2  ✔ “Semana”: OTIF, renovação de contratos, backlog de QA/expedição
│  ├─ Art.3  ✔ “Mês”: Produtividade por linha, consumo MP, custos, disponibilidade da frota
│  └─ Art.4  ✔ “Cliente/Projeto”: Contrato ↔ Lotes ↔ Entregas ↔ Cobrança
│
├──▶ (Valor) → Um roteiro único para a pauta da reunião.
│
[ CAPÍTULO XI — GOVERNANÇA & RASTREABILIDADE ]
│  Regras para confiança dos dados (prontas para ativação).
│  ├─ Art.1  ✔ Responsável por etapa (owner) e carimbo de data/hora
│  ├─ Art.2  ✔ Checklists padrão (QA, segurança, expedição)
│  ├─ Art.3  ✔ Logs de mudança (quem moveu o quê/quando)
│  └─ Art.4  ✔ Versão/export como evidência (CSV/JSON/PDF)
│
├──▶ (Garantia) → Decisão auditável e repetível.
│
[ CAPÍTULO XII — FLUXO OPERACIONAL (PIPELINE GERAL) ]
│  Sequência linear do uso à decisão.
│  ├─ Art.1  ✔ FILTRAR contexto (período • unidade • cliente/produto)
│  ├─ Art.2  ✔ CONTRATOS: revisar vigentes/renovação e riscos
│  ├─ Art.3  ✔ LOTES: checar gating, bloqueios e liberar próximos passos
│  ├─ Art.4  ✔ ESTOQUES: garantir MP crítica e PA p/ janelas de entrega
│  ├─ Art.5  ✔ 5S/COMPLIANCE: resolver vencimentos (30d/7d) e pendências
│  ├─ Art.6  ✔ PRODUÇÃO: validar volumes, HH/HM, consumo e desvios
│  ├─ Art.7  ✔ FROTA: alinhar disponibilidade/rotas/manutenção
│  ├─ Art.8  ✔ DECIDIR: priorizar ações, designar responsáveis e prazos
│  └─ Art.9  ✔ EXPORTAR: atas, KPIs e planos (CSV/JSON/PDF)
│
├──▶ (Síntese) → FILTRAR → CONFERIR (módulos) → PRIORIZAR → DESIGNAR → EXPORTAR.
│
[ CAPÍTULO XIII — QUADRO DE KPIs (RESUMO PARA CAPA) ]
│  Painel de abertura da reunião (uma linha por domínio).
│  ├─ Art.1  ✔ Contratos: #vigentes • #renovar ≤60d • valor mensal
│  ├─ Art.2  ✔ Lotes: #em produção • OTIF • gargalos (top-3 causas)
│  ├─ Art.3  ✔ MP: itens < mínimo • dias de cobertura • compras sugeridas
│  ├─ Art.4  ✔ PA: pronta-entrega • backlog de expedição • lead de QA
│  ├─ Art.5  ✔ 5S/Compliance: % em dia • itens 30d/7d/vencidos
│  ├─ Art.6  ✔ Produção: Qtd (dia/mês) • HH/HM • custo unitário real vs plano
│  └─ Art.7  ✔ Frota: disponibilidade • preventiva vencendo • custo/km
│
└──▶ (Objetivo) → 1 slide para abrir a reunião e guiar a pauta.

```


## `Módulo__COMPRAS:` Regras para Compras


```RUBY
[ CAPÍTULO I — OBJETIVO & ESCOPO (FAQ QTD. SUGERIDA) ]
│  Explicar e operacionalizar “quanto comprar agora” para repor estoque.
│  ├─ Art.1  ✔ Público-alvo: Compras • PCP • Estoques
│  ├─ Art.2  ✔ Entregáveis: entendimento rápido + simulador + exemplos + checklist
│  ├─ Art.3  ✔ Acesso: busca por termos, navegação por chips, impressão
│  └─ Art.4  ✔ Política base: Alvo = 1,5×Mínimo; Sugerido = ceil(max(0, Alvo − Atual))
│
├──▶ (Propósito) → Padronizar cálculo e reduzir decisões ad-hoc.
│
[ CAPÍTULO II — NAVEGAÇÃO & FERRAMENTAS ]
│  Como o usuário encontra e consome a informação.
│  ├─ Art.1  ✔ Barra de busca (abre seções que batem com o termo)
│  ├─ Art.2  ✔ Chips de atalho (Objetivo, Conceitos, Alvo, Sugerido, Exemplos, etc.)
│  ├─ Art.3  ✔ Expandir/Recolher tudo • Imprimir com tudo aberto
│  └─ Art.4  ✔ Copiar fórmula (clipboard) • Limpar busca
│
├──▶ (Princípio) → Facilitar a leitura em reunião e o estudo individual.
│
[ CAPÍTULO III — CONCEITOS-CHAVE ]
│  Vocabulário comum do cálculo.
│  ├─ Art.1  ✔ Mínimo (nível de segurança)
│  ├─ Art.2  ✔ Atual (estoque disponível)
│  ├─ Art.3  ✔ Alvo (meta de reposição = 1,5×Mínimo)
│  └─ Art.4  ✔ Sugerido (quanto comprar agora, com arredondamento ↑)
│
├──▶ (Resultado) → Mesma regra mental para toda a equipe.
│
[ CAPÍTULO IV — SIMULADOR RÁPIDO ]
│  Entrada → Saída para decisão imediata.
│  ├─ Art.1  ✔ Entradas: Mínimo • Atual • Arredondamento (ceil/none)
│  ├─ Art.2  ✔ Política “Estoque Livre” (permitir ou forçar 0 quando Mínimo=0,001)
│  ├─ Art.3  ✔ Saídas: Alvo • Diferença • Sugerido • Situação (badge)
│  └─ Art.4  ✔ Ação: copiar fórmulas, aplicar presets de exemplo
│
├──▶ (Diagnóstico) → Mostra impacto de políticas antes de comprar.
│
[ CAPÍTULO V — REGRAS DO CÁLCULO ]
│  Passo a passo formalizado.
│  ├─ Art.1  ✔ Alvo = 1,5 × Mínimo
│  ├─ Art.2  ✔ Diferença = Alvo − Atual
│  ├─ Art.3  ✔ Sugerido = ceil(max(0, Diferença))
│  └─ Art.4  ✔ Filtro de envio: só aparece se Sugerido > 0
│
├──▶ (Padronização) → Todos calculam igual, sem variações locais.
│
[ CAPÍTULO VI — EXEMPLOS TABELADOS ]
│  Casos típicos para validação rápida.
│  ├─ Art.1  ✔ Reposição clara (Atual < Mínimo)
│  ├─ Art.2  ✔ Ainda precisa (Atual < Alvo, porém > Mínimo)
│  ├─ Art.3  ✔ Acima do alvo (Sugerido = 0)
│  ├─ Art.4  ✔ No limite (Sugerido = 0)
│  └─ Art.5  ✔ Estoque Livre (0,001) — respeita política escolhida
│
├──▶ (Confiança) → Exemplos replicáveis para checagem em reunião.
│
[ CAPÍTULO VII — LEGENDAS & ESTADOS DE ESTOQUE ]
│  Leitura visual padronizada do risco.
│  ├─ Art.1  ✔ Sem estoque • Abaixo do mínimo • No mínimo • Próximo do mínimo
│  ├─ Art.2  ✔ Adequado • Alto • Estoque Livre
│  ├─ Art.3  ✔ Cores/badges: crit • warn • ok • neutro
│  └─ Art.4  ✔ Regra de classificação exibida junto ao simulador
│
├──▶ (Clareza) → Todos interpretam o mesmo sem ambiguidade.
│
[ CAPÍTULO VIII — CHECKLIST DE QUALIDADE ]
│  Itens que garantem a aplicação correta.
│  ├─ Art.1  ✔ Confirmar Alvo = 1,5×Mínimo
│  ├─ Art.2  ✔ Confirmar Sugerido = ceil(max(0, Alvo − Atual))
│  ├─ Art.3  ✔ Conferir política de Estoque Livre
│  ├─ Art.4  ✔ Verificar campos vazios/zeros e arredondamento por unidade
│  └─ Art.5  ✔ Persistência local (marcações salvas)
│
├──▶ (Controle) → Evita erro operacional recorrente.
│
[ CAPÍTULO IX — LIMITAÇÕES & POLÍTICAS FUTURAS ]
│  O que o modelo NÃO cobre (explícito para alinhamento).
│  ├─ Art.1  ✔ Lead time do fornecedor (dias)
│  ├─ Art.2  ✔ Consumo/demanda futura (MRP/OP)
│  ├─ Art.3  ✔ Pedidos abertos (abatimento do sugerido)
│  ├─ Art.4  ✔ MOQ e múltiplos de compra
│  └─ Art.5  ✔ Preço/descontos (foco é quantidade física)
│
├──▶ (Roadmap) → Pauta para evoluções do cálculo corporativo.
│
[ CAPÍTULO X — RESUMO EM UMA FRASE ]
│  “Compre o suficiente para atingir 1,5× o Mínimo; se já estiver lá, não compre.”
│
├──▶ (Mensagem-chave) → Orienta a decisão em 5 segundos.
│
[ CAPÍTULO XI — GOVERNANÇA & RASTREIO ]
│  Evidência e auditoria do uso.
│  ├─ Art.1  ✔ Versão das fórmulas e data/hora de consulta
│  ├─ Art.2  ✔ Registro de política ativa (Estoque Livre • arredondamento)
│  ├─ Art.3  ✔ Export/print como anexo de decisão
│  └─ Art.4  ✔ Links diretos (#hash) para seções específicas
│
├──▶ (Confiabilidade) → Decisão documentada e repetível.
│
[ CAPÍTULO XII — FLUXO OPERACIONAL (PIPELINE EL-01) ]
│  Sequência linear para uso em reunião ou no dia-a-dia.
│  ├─ Art.1  ✔ BUSCAR termo → abrir seções relevantes
│  ├─ Art.2  ✔ REVISAR Conceitos → alinhar vocabulário
│  ├─ Art.3  ✔ CALCULAR no Simulador (definir políticas)
│  ├─ Art.4  ✔ VALIDAR com Exemplos e Checklist
│  ├─ Art.5  ✔ DECIDIR comprar (se Sugerido > 0) ou aguardar
│  └─ Art.6  ✔ REGISTRAR evidência (imprimir/exportar) e próximos passos
│
└──▶ (Síntese) → BUSCAR → REVISAR → CALCULAR → VALIDAR → DECIDIR → REGISTRAR.

```


## `Módulo__Almoxarifado:` (INVENTÁRIO GERAL)

```RUBY
[ CAPÍTULO I — PROPÓSITO & ESCOPO (INVENTÁRIO GERAL) ]
│  Garantir acurácia do estoque por contagem integral controlada.
│  ├─ Público-alvo: Almoxarifado • PCP • Auditoria • Controladoria • TI/ERP
│  ├─ Resultado: sessão criada → contagens → reconciliação → ajustes → relatórios
│  └─ Integrações: Compras/NF-e • Produção • Fiscal/Financeiro • MDM/Cadastros
│
├──▶ Mensagem-chave: “Conte certo, explique o delta, ajuste com trilha de auditoria.”
[ CAPÍTULO II — PRÉ-REQUISITOS & PARÂMETROS ]
│  ├─ Endereçamento (depósito/rua/coluna/nível/posição) e SKUs (UM, lote/série).
│  ├─ Cut-off de movimentações (lock por sessão) e janela de baixa controlada.
│  ├─ Tolerâncias (qtd/valor) por item/grupo e regra de 2ª contagem automática.
│  └─ Coleta: etiquetas, coletores/QR, anexos (foto/justificativa).
[ CAPÍTULO III — TIPOS DE INVENTÁRIO SUPORTADOS ]
│  Geral • Por localização • ABC • Cego • Dupla contagem (reconfirmação).
│
├──▶ Escolha o tipo antes de “lockar” a sessão para evitar retrabalho.
[ CAPÍTULO IV — FLUXO (ESTADOS & GATES) ]
│  planned → locked → counting → recount* → reconciling → approved → posted → closed
│  ├─ *recount: divergência > tolerância
│  ├─ approved: segregação de função (quem conta ≠ quem aprova)
│  └─ posted: ajustes lançados (refletem em custo médio/valorização)
[ CAPÍTULO V — PAPÉIS & PERMISSÕES ]
│  ├─ Contador: coleta + anexos
│  ├─ Conciliador: análise de divergências + proposta de ajuste (com causa)
│  ├─ Aprovador: valida ajustes e encerra
│  └─ Admin: parametrização, desbloqueios, trilha de auditoria
[ CAPÍTULO VI — EXECUÇÃO DE CONTAGEM ]
│  ├─ Roteirização por endereço com confirmação (geografia → item/lote/série/validade)
│  ├─ Bloqueio momentâneo do endereço até recontagem quando aplicável
│  └─ Evidências: foto, nota de avaria, observações operacionais
[ CAPÍTULO VII — RECONCILIAÇÃO & CAUSAS ]
│  ├─ SIS × Físico: delta (qtd/valor), ranking por impacto
│  ├─ Causas: cadastro, perda/quebra, apont. produção, nota pendente, etc.
│  ├─ Tolerância: aprova auto dentro do limite; fora do limite → recontar/aprovar
│  └─ Proposta de ajuste: justificativa obrigatória + anexos
[ CAPÍTULO VIII — AJUSTES & IMPACTOS ]
│  ├─ Ajustes por item/endereço, com usuário, data, motivo
│  ├─ Reflexo em CM/valorização + lançamento contábil (CFOP/contas do ERP)
│  └─ Observação fiscal: variações por UF/regime — mapear previamente
[ CAPÍTULO IX — UX DA TELA (AÇÕES & FILTROS) ]
│  ├─ Botões: Criar sessão • Iniciar • Recontagem • Reconciliar • Aprovar • Lançar • Encerrar
│  ├─ Filtros: depósito • endereço • classe ABC • responsável • situação
│  └─ Exports: CSV/PDF (por sessão, divergência, ajuste) + anexos
[ CAPÍTULO X — KPIs PARA REUNIÃO ]
│  ├─ Acurácia (qtd/valor)
│  ├─ % itens divergentes • Valor total divergente
│  ├─ Tempo médio de contagem & reconciliação
│  └─ % recontagem • % aprovações automáticas
[ CAPÍTULO XI — MODELO DE DADOS (RESUMO TÉCNICO) ]
│  inventory_sessions • inventory_tasks • inventory_counts
│  inventory_discrepancies • inventory_adjustments • inventory_logs • inventory_attachments
│
├──▶ Ponto de auditoria: quem/quando/onde/porquê da alteração.
[ CAPÍTULO XII — ERROS PADRÃO & MENSAGENS ]
│  ├─ “Informe ao menos um depósito/endereço…”
│  ├─ “Não é possível iniciar: movimentações em andamento…”
│  ├─ “Recontagem obrigatória: divergência acima da tolerância.”
│  └─ “Justificativa obrigatória para aprovar ajuste.”
[ CAPÍTULO XIII — CHECKLIST RÁPIDO (DO/KILL SWITCH) ]
│  ☐ Parâmetros revisados (planned)  ☐ Lock aplicado (locked)
│  ☐ 1ª contagem e recontagem concluídas (counting → recount)
│  ☐ Reconciliação/causas documentadas (reconciling)
│  ☐ Aprovação segregada (approved)  ☐ Lançamentos (posted)
│  ☐ Relatórios/anexos publicados (closed)
[ CAPÍTULO XIV — RISCOS & MITIGAÇÕES ]
│  ├─ Risco: contagem com movimentação ativa → Mitigação: lock + janela controlada
│  ├─ Risco: tolerância mal calibrada → Mitigação: piloto por ABC e ajuste fino
│  ├─ Risco: erro de cadastro → Mitigação: MDM + validação de UM/lote/série
│  └─ Risco: impacto contábil indevido → Mitigação: CFOP/contas homologadas
[ CAPÍTULO XV — PIPELINE OPERACIONAL (INV-01) ]
│  1) PLANEJAR — definir escopo, tolerâncias, papéis → criar sessão (planned)
│  2) BLOQUEAR — aplicar cut-off/lock (locked)
│  3) CONTAR — execução 1ª contagem (counting) + recontagem automática (recount)
│  4) RECONCILIAR — analisar deltas, causas, proposta de ajustes (reconciling)
│  5) APROVAR — segregação de função e aceite (approved)
│  6) LANÇAR — ajustes no ERP (posted), atualização de custo/valor
│  7) ENCERRAR — relatórios, anexos, trilha e fechamento (closed)
│
└──▶ Saída da reunião: sessão com cronograma, donos, tolerâncias e metas de acurácia.

```


## `Módulo__Almoxarifado:` (Inventário Ciclico)


```Ruby
[ CAPÍTULO I — PROPÓSITO & ESCOPO (CONTAGEM CÍCLICA) ]
│  Garantir acurácia contínua com pequenas contagens recorrentes e dirigidas.
│  ├─ Foco: itens/endereços críticos (ABC, valor, giro, risco, histórico)
│  ├─ Impacto: mínima interrupção operacional (sem parar o armazém inteiro)
│  └─ Saída: plano cíclico ativo → tarefas geradas → contagens → reconciliações → ajustes
│
├──▶ Mensagem-chave: “Conte pouco, conte sempre, corrija cedo.”

[ CAPÍTULO II — DIFERENÇAS VS. INVENTÁRIO GERAL ]
│  ├─ Escopo: subconjuntos (cíclico) × 100% do estoque (geral)
│  ├─ Frequência: recorrente (cíclico) × pontual (geral)
│  ├─ Operação: baixa interferência (cíclico) × geralmente com cut-off (geral)
│  └─ Métrica: tendência de acurácia (cíclico) × acurácia do marco (geral)

[ CAPÍTULO III — ESTRATÉGIAS DE AGENDAMENTO ]
│  ├─ ABC/Valor: A diário/semana • B semanal/quinzenal • C mensal/trimestral
│  ├─ Giro/Movimentação: mais movimento = mais frequência
│  ├─ Risco/Controle: seriados, perecíveis, críticos primeiro
│  ├─ Endereços: rotas por rua/posição (cobertura semanal de zonas quentes)
│  └─ Histórico: reforçar onde houve divergência recente

[ CAPÍTULO IV — FLUXO (ESTADOS & GATES) ]
│  scheduled → counting → recount* → reconciling → posted → done
│  ├─ scheduled: tarefas emitidas pelo plano
│  ├─ counting: coleta (qtd, lote/série/validade quando aplicável)
│  ├─ recount*: recontagem automática fora de tolerância
│  ├─ reconciling: causa raiz + proposta de ajuste
│  ├─ posted: ajuste lançado (reflete em custo/valorização no ERP)
│  └─ done: tarefa encerrada com trilha e anexos

[ CAPÍTULO V — CONFIGURAÇÃO DO PLANO ]
│  ├─ Critérios: ABC • valor • giro • risco • histórico
│  ├─ Frequências: diária • semanal • quinzenal • mensal
│  ├─ Escopo: depósitos • ruas/posições • famílias • lotes/séries
│  ├─ Tolerâncias por item/grupo (qtd/valor) + 2ª contagem automática
│  ├─ Modo: cego (sem saldo SIS) × assistido
│  └─ Janelas operacionais e time responsável

[ CAPÍTULO VI — EXECUÇÃO & COLETA ]
│  ├─ Roteiro por endereço com confirmação (código de barras/QR)
│  ├─ Bloqueio temporário do endereço até finalizar contagem/recontagem
│  ├─ Captura de avarias e anexos (foto/observação)
│  └─ Usabilidade: leitor móvel/coletor + validação de localização

[ CAPÍTULO VII — RECONCILIAÇÃO & AJUSTES ]
│  ├─ Comparação SIS × físico: delta em quantidade e valor
│  ├─ Causas: cadastro, processo, apontamento, nota pendente, perda/quebra
│  ├─ Workflow: dentro da tolerância → auto-aprova • fora → aprovação superior
│  └─ Lançamentos: impacto em custo médio e valorização segundo regras do ERP

[ CAPÍTULO VIII — INTEGRAÇÕES ]
│  Compras/NF-e (notas pendentes) • Produção (consumo/sobras) • Manutenção (OS)
│  Financeiro/Fiscal (valorização/provisões) • MDM/Cadastros (SKU/UM/endereços)

[ CAPÍTULO IX — MODELO DE DADOS (RESUMO TÉCNICO) ]
│  cc_plans • cc_tasks • cc_counts • cc_discrepancies • cc_adjustments • cc_logs
│
├──▶ Ponto de auditoria: quem, quando, onde, o quê e por quê (ID da tarefa e do endereço).

[ CAPÍTULO X — PAPÉIS & PERMISSÕES ]
│  ├─ Contador: executa tarefas e coleta
│  ├─ Conciliador: trata divergências e propõe ajustes
│  ├─ Aprovador: valida ajustes fora da tolerância
│  └─ Admin: cria/edita planos, frequências e tolerâncias

[ CAPÍTULO XI — KPIs (VISÃO DE REUNIÃO) ]
│  ├─ Acurácia parcial (qtd/valor) e tendência por ABC/deposito/endereço
│  ├─ % divergência e valor total divergente (heatmap por localização)
│  ├─ Lead time: execução e reconciliação por tarefa
│  ├─ % tarefas com recontagem
│  └─ % auto-aprovação dentro da tolerância

[ CAPÍTULO XII — MENSAGENS PADRÃO ]
│  ├─ “Plano cíclico sem critérios definidos.”
│  ├─ “Endereço inválido ou bloqueado por outra tarefa.”
│  ├─ “Recontagem obrigatória: divergência acima da tolerância.”
│  └─ “Permissão insuficiente para ajuste fora da tolerância.”

[ CAPÍTULO XIII — CHECKLIST RÁPIDO ]
│  ☐ Plano com critérios & frequências  ☐ Tarefas atribuídas (scheduled)
│  ☐ Contagens feitas e recontagens quando necessário (counting → recount)
│  ☐ Reconciliação concluída (reconciling)  ☐ Ajustes lançados (posted)
│  ☐ Tendência de acurácia acompanhada e ações corretivas em curso

[ CAPÍTULO XIV — RISCOS & MITIGAÇÕES ]
│  ├─ Falta de cobertura de áreas críticas → Rotas semanais por endereço
│  ├─ Tolerância mal calibrada → Piloto por ABC + ajuste fino
│  ├─ Falhas de cadastro → Governança MDM (UM/lote/série)
│  └─ Ajuste contábil indevido → matriz CFOP/contas homologada + aprovação

[ PIPELINE OPERACIONAL (CC-01) — “DA REGRA À CORREÇÃO” ]
│  1) DEFINIR → critérios, frequências, escopo e tolerâncias (plano cíclico)
│  2) GERAR → tarefas (scheduled) por calendário/regra
│  3) EXECUTAR → coleta guiada por endereço (counting)
│  4) RECONTAR → automático quando fora da tolerância (recount)
│  5) RECONCILIAR → causa raiz + proposta de ajuste (reconciling)
│  6) APROVAR/LANÇAR → ajustes no ERP (posted)
│  7) FEEDBACK → atualizar KPIs, rotas e parâmetros (melhoria contínua)
│
└──▶ Saída da reunião: plano cíclico aprovado (ABC/giro/risco), calendário por área,
tolerâncias por grupo, donos definidos e painel de KPIs para acompanhamento semanal.

```


# `Módulo__Almoxarifado:` · Almoxarifado — Movimentação Operacional & Governança

> **Escopo:** desenho ponta-a-ponta para entradas, saídas, devoluções e retornos (5S/Simples), com monitoramento de estoque, geração de pedidos semi-automáticos e relatórios operacionais/gerenciais. **Foco:** execução rápida (atalhos/teclado), rastreabilidade e disciplina de uso.


## 0) Objetivo do pipeline

* Garantir **agilidade operacional** no balcão do almoxarifado (sem mouse, com atalhos).
* Assegurar **rastreabilidade** por item, pessoa, contrato/OP e motivo.
* Sustentar **acurácia de estoque** e **compliance** (devoluções, 5S, EPIs).
* Alimentar **reposição inteligente** (pedido semi-automático) e **analytics**.


## 1) Macrofluxo (fases e gates)

1. **Planejar & Parametrizar**
   Definições de catálogo, políticas (devolução obrigatória, 5S), atalhos, integrações e perfis.

2. **Capturar Evento de Estoque** *(frontline)*
   Entrada | Saída | Devolução | Retorno 5S | Retorno Simples — todos via **modo PDV** (teclado/scan) com validações.

3. **Validar & Registrar**
   Checagens (saldo, obrigatoriedades), vinculação (cliente/contrato/OP/centro de custo), **auditoria** e anexos (quando necessário).

4. **Consolidar & Reconciliar**
   Atualização de saldos, movimentação contábil (se aplicável), pendências de devolução e fila de exceções.

5. **Repor & Monitorar**
   Cálculo de reposição (mínimo/ideal, lead time, múltiplos, pedidos em trânsito), **pedido semi-automático** e alertas.

6. **Analisar & Melhorar**
   Relatórios (tempo real e períodos), KPIs, inspeções 5S, ações corretivas (treino/cadastro/processo), auditorias.

**Gates críticos:**
G1 Parametrização ok → G2 Validações ok → G3 Movimentação registrada → G4 Pendências tratadas → G5 Reposição gerada/avaliada.


## 2) Cenários operacionais (nível de detalhe)

### 2.1 Entrada de Materiais

* **Modos de captura:**
  a) **Bipar chave** (QR/Code128) · b) **Digitar chave** · c) **Manual** (cadastro rápido) · d) **Apontamento** (sem documento fiscal).
* **Dados mínimos:** chave/doc, fornecedor (ou origem interna), item, UM, Qtd, Lote/Série/Validade (se aplicável), **finalidade** (contrato/OP/CC), anexos.
* **Regras:** bloqueio de duplicidade de chave; tolerâncias de recebimento; quarentena (qualidade) opcional.
* **Saída:** movimentação **ENTRADA** registrada, saldo atualizado, vínculo com compra (se houver).

### 2.2 Saída de Material (modo PDV/teclado)

* **UX:** pesquisa por **código/scan** → selecionar → informar **contrato/OP/cliente** → **confirmar** (Enter).
* **Atalhos:** F1 Buscar | F2 Qtd | F3 Alternar UM | F4 Motivo | F5 Trocar destino | F9 Confirmar | ESC Cancelar.
* **Regras:** verificação de saldo; política de **devolução obrigatória** por família (ex.: discos, EPIs); bloqueio por validade/lote.
* **Saída:** movimentação **SAÍDA**, com solicitante (**quem pegou**), liberador (**quem entregou**), local de consumo.

### 2.3 Devolução de Material

* **Uso típico:** troca de consumível (ex.: **disco de corte** — obrigatório devolver o usado).
* **Regras:** configuração por item/família (**Obrigatório**, **Opcional**, **Dispensado**).
* **Efeito:** movimentação **DEVOLUÇÃO** (retorno ao estoque adequado ou descarte 5S), abertura/fecho de **pendência** quando não devolvido.

### 2.4 Retorno 5S (sucata, quebrado, EPI inutilizado)

* **Classificação:** **não reaproveitável** → baixa para **perda/consumo 5S**; **reaproveitável** → retorno a estoque secundário.
* **Registro:** motivo 5S (avaria, desgaste, vencimento), foto opcional, responsável.
* **Saída:** movimentação **RETORNO 5S** (mov. contábil/fiscal conforme regras). Indicadores de desperdício.

### 2.5 Retorno Simples

* **Uso:** pegou por engano; reabastecer o mesmo endereço.
* **Efeito:** movimentação **RETORNO** com referencial à saída original (quando houver), sem carimbar 5S.

---

## 3) Relatórios & Visões (tempo real e períodos)

* **Filtros (todas as visões):** período (hoje, 15d, mês, tri/semestre/ano, custom), material, família, contrato, OP, cliente, solicitante, liberador, centro de custo, tipo de movimento, unidade/deposito/endereço.
* **Operacional (tempo real):**

  * **Fila de balcão:** últimas saídas/devoluções com badges de pendência.
  * **Pendências de devolução:** por colaborador/item/prazo.
* **Consumo por destino:** “O que saiu” por **cliente/contrato/OP** (quantidade, valor, top N itens).
* **Produtividade de colaborador:** **quem solicitou** (freq./dia, itens por visita, desvio padrão de consumo).
* **Conformidade:** **quem liberou** (conferente/almoxarife/encarregado), violações de política.
* **Mix & finalidade:** tipo de material (**venda/locação/outros**), mapa de uso por família.
* **Ciclo de reposição:** sugestão x pedido gerado x recebido; lead time médio; ruptura evitada.
* **Export:** CSV/PDF; API para BI.


## 4) Monitoramento de Estoque & Reposição

### 4.1 Governança de Notas/Finalidade

* **Ação ao Compras:** exigir **discriminação de itens e finalidade** (para quê/qual contrato/OP/centro de custo).
* **Validação:** bloqueio de entrada sem finalidade quando política ativa.

### 4.2 Pedido Semi-Automático

* **Lógica base:** Alvo = 1,5×Mínimo; Sugerido = ceil(max(0, Alvo − (Atual + Em trânsito))).
* **Aprimoradores (opcionais):** lead time, **múltiplos de compra**/MOQ, sazonalidade, mínimos por embalagem, estoque de segurança dinâmico.
* **Workflow:** Sugestões → revisão (Compras/Almox) → **gerar pedido** → integração ERP.

### 4.3 Catálogo & Uso

* **Catálogo mestre:** código, descrição, UM, famílias, **política de devolução**, 5S, validade/lote, destinos permitidos.
* **Regras de uso:** por **tipo** (venda/locação/obra/serviço), **centro de custo**, **contrato**, **OP**.

### 4.4 Ferramentas (viabilidade de controle)

* **Empréstimo x Devolução:** vínculo a colaborador/OS, prazo, estado.
* **Manutenção preventiva/ciclo:** check-in/out; bloqueio por manutenção/inspeção.
* **Perdas/avarias:** canal 5S e responsabilização.


## 5) Papéis, SLAs e Controles

| Papel              | Responsabilidades principais                                        | SLAs típicos                                         |
| ------------------ | ------------------------------------------------------------------- | ---------------------------------------------------- |
| **Solicitante**    | Retirar/devolver conforme política; informar contrato/OP            | Atender em **≤ 2 min** no balcão                     |
| **Almoxarife**     | Registrar movimentos; zelar por atalhos/PDV; checar saldos/validade | 100% de movimentos com **responsáveis** e **motivo** |
| **Conferente**     | Conferir entradas; checar pendências de devolução                   | Entrada registrada **no dia**                        |
| **Compras**        | Discriminar finalidade e revisar pedidos semi-automáticos           | Revisão em **≤ 24 h**                                |
| **Gestor**         | Monitorar KPIs, aprovar exceções e políticas                        | Exceções tratadas **≤ 48 h**                         |
| **SSMA/Qualidade** | Acompanhar 5S, EPIs, descarte correto                               | Pendências 5S **0 > 5 dias**                         |

**Controles/alertas:**

* **Pendência de devolução** por colaborador/item com SLA (ex.: 48 h).
* **Saída sem contrato/OP** (bloqueio ou justificativa).
* **Validade/lote** — bloqueio para itens vencidos.
* **Ruptura** iminente (estoque < mínimo + lead time).
* **Uso anômalo** (padrão fora da faixa por colaborador/obra).

## 6) Dados & Integrações

**Entidades-chave:**

* `items` (catálogo), `stock_lots` (lote/série/validade), `locations` (depósito/endereço),
  `movements` (entrada/saída/devolução/retorno/5S), `pendings` (devolução),
  `orders` (pedidos de compra), `invoices` (NF/sem doc), `contracts`, `production_orders`, `people`.

**Integrações:**

* **ERP/Compras/Fiscal:** notas, pedidos, CFOP/contábil.
* **Produção:** OP/baixas; **Projetos/Contratos**: vinculação de consumo.
* **SSMA/Qualidade:** EPIs, inspeções, descarte.
* **BI/Datamart:** fatos de movimento e saldos diários.


## 7) Regras & Políticas (essenciais)

* **Devolução obrigatória por família** (ex.: disco, broca, EPI reutilizável).
* **Retornos 5S** com motivo padronizado e, quando aplicável, **foto/anexo**.
* **Sem documento fiscal:** permitido via **Apontamento** com justificativa e aprovação (trilha).
* **Modo PDV/teclado:** operações críticas devem ser possíveis **sem mouse**; atalhos publicados no balcão.
* **Auditoria:** todo movimento tem **quem pediu**, **quem liberou**, **onde será usado** e **por quê**.


## 8) KPIs & Painéis

* **Acurácia de estoque** (quantidade/valor).
* **Rupturas evitadas** e **nível de serviço** do balcão.
* **Pendências de devolução** (qtde, SLA, reincidência por colaborador).
* **Consumo por destino** (cliente/contrato/OP/CC) e por **família**.
* **Tempo médio de atendimento** no balcão (solicitação→liberação).
* **Reposição:** sugestão x pedido x entrega; **lead time** e **aderência a múltiplos/MOQ**.
* **5S/Descarte:** volume e valor por motivo (avaria, desgaste, vencimento).
* **Produtividade:** visitas/dia por colaborador, itens/visita, top consumidores.


## 9) Exceções & Tratamento

* **Sem saldo / item bloqueado:** fila de exceções (substituto, adiantamento, compra urgente).
* **Não devolveu item obrigatório:** aviso imediato, bloqueio de novas retiradas (política opcional), notificação ao líder.
* **Entrada divergente** (sem finalidade/documento): quarentena até regularização.
* **Lote/validade inválida:** impedir uso e propor **retorno 5S**.


## 10) Roadmap de implantação (sugestão)

1. **Semana 1–2**: Catálogo, políticas (devolução/5S), atalhos PDV, integrações mínimas (Contratos/OP).
2. **Semana 3–4**: Relatórios tempo real, pendências de devolução, KPIs básicos, auditoria.
3. **Semana 5–6**: Reposição semi-automática (mínimo/ideal + em trânsito), múltiplos/MOQ.
4. **Semana 7–8**: 5S avançado (anexos), anomalias de consumo, BI.

---

## 11) RACI (resumo)

* **R (Responsible):** Almoxarife (movimento), Conferente (entrada), Compras (reposição).
* **A (Accountable):** Gestor de Suprimentos/Operações.
* **C (Consulted):** Produção, SSMA, Financeiro/Contábil.
* **I (Informed):** Contratos/Projetos, RH (EPI).


## 12) Quick wins & Boas práticas

* **Cartão de atalho** fixo no balcão (F1…F9).
* **Pendência de devolução** visível por colaborador ao logar no PDV.
* **Etiquetas com QR** por endereço e por item crítico.
* **Motivos padronizados** (dropdown) e **finalidade obrigatória**.
* **Treino de 15 min** para novos usuários (fluxo PDV + devoluções).


### Resultado esperado (EL-01)

Pipeline operante com **execução rápida**, **rastreável** e **orientada a políticas**, alimentando **reposição semi-automática** e **governança de estoque** — pronto para apresentação e decisão de implementação.


```Ruby
[ Entrada de Materiais + Captura ágil e auditável ]
│ ├─ Identifica origem (NF-e/chave bipada, digitação, manual, apontamento sem documento)
│ ├─ Valida item/lote/validade e destino (depósito/endereço)
│ ├─ Registra evidências (foto/anexo) e quem lançou
│ └─ Atualiza saldo + integrações (compras/fiscal) com trilha de auditoria
│
├──▶ Conclusão com frase curta: Entrada registrada e estoque atualizado
[ Saída de Material (PDV) + 100% via teclado/scan ]
│ ├─ Busca rápida (código/QR), define quantidade e destino (Contrato/OP/CC)
│ ├─ Aplica política (ex.: devolução obrigatória / lote exigido)
│ ├─ Baixa estoque com “quem pegou / quem liberou / motivo”
│ └─ Gera comprovante e, se obrigatório, pendência de devolução
│
├──▶ Conclusão com frase curta: Saída confirmada em segundos
[ Devolução de Material + Pendência rastreável ]
│ ├─ Vincula ao movimento de saída (ID/colaborador/item)
│ ├─ Confere integridade (lote/condição) e fecha pendência
│ ├─ Retorna ao estoque ou direciona para descarte/reparo
│ └─ Mantém histórico de cumprimento (SLA por colaborador)
│
├──▶ Conclusão com frase curta: Pendência fechada, saldo reconciliado
[ Retorno 5S + Destinação correta de itens inservíveis ]
│ ├─ Classifica retorno (inservível, quebrado, EPI vencido)
│ ├─ Registra causa e destino (descarte, reciclagem, conserto)
│ ├─ Não repõe saldo de “consumo final” (apenas evidencia)
│ └─ Gera alertas para compras/segurança se houver padrão de falha
│
├──▶ Conclusão com frase curta: Conformidade 5S evidenciada
[ Retorno Simples + Correção de retirada por engano ]
│ ├─ Seleciona saída original e confirma devolução íntegra
│ ├─ Restitui saldo ao endereço/lote de origem
│ ├─ Registra quem devolveu e quem conferiu
│ └─ Atualiza indicadores de acurácia e recorrência
│
├──▶ Conclusão com frase curta: Estoque reposto sem ruído
[ Relatórios + Visão operacional e gerencial ]
│ ├─ Tempo real, diário, quinzenal, mensal, semestral, anual e período customizado
│ ├─ Por material/cliente/contrato/OP e por colaborador/conferente/encarregado
│ ├─ Frequência de visitas ao almoxarifado por colaborador (produtividade)
│ └─ Classificação por tipo (venda, locação, outros) e custos associados
│
├──▶ Conclusão com frase curta: Decisão com dados, não suposição
[ Monitoramento de Estoque + Reposição semi-automática ]
│ ├─ Coleta finalidade por nota/ítem (para quê, demanda, centro de custo)
│ ├─ Calcula sugerido: alvo = 1,5×mínimo; ajusta MOQ/múltiplos/lead time
│ ├─ Gera pedido semi-automático para revisão de compras
│ └─ Emite alertas de ruptura/excesso e acompanha em trânsito
│
├──▶ Conclusão com frase curta: Repor certo, na hora certa
[ Catálogo de Materiais + Governança de uso ]
│ ├─ Padroniza cadastro (UOM, família, políticas de devolução e 5S)
│ ├─ Define finalidade típica por item (produção, manutenção, EPI, venda)
│ ├─ Bloqueia itens críticos sem destino (Contrato/OP/CC)
│ └─ Mantém trilha MDM para auditoria e BI
│
├──▶ Conclusão com frase curta: Catálogo limpo evita desperdício
[ Monitoramento de Ferramentas + Viabilidade e controle ]
│ ├─ Empréstimo/retorno com responsável e prazo
│ ├─ Estado de conservação, manutenção preventiva e calibração
│ ├─ Consumo de peças (baixas automáticas por OS)
│ └─ Indicadores de disponibilidade e custo por hora
│
├──▶ Conclusão com frase curta: Ferramenta certa, sempre pronta
[ Segurança, Atalhos e Auditoria + Operação fluida e confiável ]
│ ├─ Atalhos de PDV (F1…F9) para zero-mouse e alto giro
│ ├─ Perfis e segregação (solicitante, liberador, aprovador)
│ ├─ Logs completos: quem, quando, onde, por quê, com anexo
│ └─ Integrações ERP/Fiscal/Produção para consistência ponta a ponta
│
├──▶ Conclusão com frase curta: Rápido no balcão, sólido na auditoria

```