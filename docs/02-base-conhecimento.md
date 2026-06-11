# Base de Conhecimento

## Dados Utilizados

| Arquivo                        | Formato | Para que serve no Agente                                                  |
| ------------------------------ | ------- | ------------------------------------------------------------------------- |
| `historico_atendimento.csv`    | CSV     | Contextualizar interações anteriores e manter continuidade no atendimento |
| `perfil_investidor.json`       | JSON    | Identificar objetivos financeiros, tolerância a risco e perfil do usuário |
| `produtos_financeiros.json`    | JSON    | Sugerir produtos compatíveis com o perfil do cliente                      |
| `transacoes.csv`               | CSV     | Analisar receitas, despesas e comportamento financeiro                    |
| `categorias_gastos.json`       | JSON    | Classificar automaticamente despesas por categoria                        |
| `metas_financeiras.json`       | JSON    | Armazenar objetivos financeiros e acompanhar progresso                    |
| `orcamento_mensal.csv`         | CSV     | Comparar gastos atuais com o orçamento planejado                          |
| `indicadores_financeiros.json` | JSON    | Disponibilizar métricas financeiras e índices econômicos                  |
| `alertas_financeiros.csv`      | CSV     | Registrar alertas gerados pelo sistema                                    |
| `anomalias_detectadas.csv`     | CSV     | Armazenar movimentações consideradas atípicas                             |
| `faq_financeiro.json`          | JSON    | Responder dúvidas frequentes sobre finanças pessoais                      |
| `regras_negocio.json`          | JSON    | Definir limites, políticas e critérios de recomendação                    |
| `score_financeiro.csv`         | CSV     | Histórico da evolução da saúde financeira do usuário                      |
| `objetivos_investimento.json`  | JSON    | Registrar metas de investimento de curto, médio e longo prazo             |
| `simulacoes_financeiras.json`  | JSON    | Armazenar resultados de simulações realizadas pelo agente                 |

---

# Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Sim. A base original foi expandida para permitir análises financeiras mais completas e personalizadas.

Principais melhorias:

* Inclusão de categorias automáticas de gastos.
* Criação de base de metas financeiras.
* Adição de histórico de alertas financeiros.
* Registro de anomalias detectadas.
* Inclusão de regras de negócio para validação das recomendações.
* Adição de score de saúde financeira.
* Inclusão de objetivos de investimento.
* Estruturação de dados para simulações financeiras.
* Padronização de datas e categorias.
* Inclusão de indicadores financeiros para contextualização das análises.

Também foram criados relacionamentos entre as bases para permitir consultas cruzadas e recomendações mais assertivas.

---

# Estratégia de Integração

## Como os dados são carregados?

Os arquivos são carregados automaticamente durante a inicialização do agente utilizando bibliotecas Python como Pandas e JSON.

Fluxo de carregamento:

1. Inicialização do agente.
2. Leitura dos arquivos CSV e JSON.
3. Validação de integridade dos dados.
4. Conversão para DataFrames e objetos estruturados.
5. Armazenamento em memória para consultas rápidas.
6. Atualização periódica dos dados conforme novas transações são registradas.

Exemplo:

```python
import pandas as pd
import json

transacoes = pd.read_csv("transacoes.csv")

with open("perfil_investidor.json") as f:
    perfil = json.load(f)
```

---

## Como os dados são usados no prompt?

O agente utiliza uma abordagem híbrida:

### Dados Fixos (System Prompt)

São incluídos permanentemente no contexto:

* Regras financeiras.
* Políticas de recomendação.
* Limitações do agente.
* Glossário financeiro.
* Estratégias de análise.

### Dados Dinâmicos (RAG)

São consultados conforme a necessidade:

* Histórico de transações.
* Perfil do cliente.
* Metas financeiras.
* Alertas.
* Produtos financeiros.
* Histórico de atendimento.

Antes de responder, o agente realiza consultas na base para recuperar apenas as informações relevantes para a pergunta do usuário.

Fluxo:

```mermaid
flowchart LR
A[Pergunta do Usuário]
--> B[Busca na Base de Conhecimento]
--> C[Recuperação de Dados]
--> D[Montagem do Contexto]
--> E[LLM]
--> F[Resposta Personalizada]
```

---

# Estrutura da Base de Conhecimento

## Perfil do Investidor

```json
{
  "id_cliente": 1001,
  "nome": "João Silva",
  "idade": 35,
  "perfil": "Moderado",
  "objetivo": "Aposentadoria",
  "horizonte_investimento": "15 anos",
  "tolerancia_risco": "Média",
  "renda_mensal": 8000
}
```

---

## Exemplo de Transações

```csv
data,categoria,descricao,valor
2026-01-05,Alimentação,Supermercado,-450.00
2026-01-08,Transporte,Combustível,-300.00
2026-01-10,Salário,Empresa XYZ,8000.00
2026-01-15,Lazer,Cinema,-60.00
```

---

## Exemplo de Metas Financeiras

```json
{
  "meta": "Reserva de Emergência",
  "valor_alvo": 30000,
  "valor_atual": 18000,
  "prazo": "12 meses",
  "percentual_concluido": 60
}
```

---

# Exemplo de Contexto Montado

> Exemplo enviado ao modelo antes da geração da resposta.

```text
DADOS DO CLIENTE

Nome: João Silva
Idade: 35 anos
Perfil de Investidor: Moderado
Renda Mensal: R$ 8.000
Saldo Atual: R$ 12.500

METAS FINANCEIRAS

1. Reserva de Emergência
Meta: R$ 30.000
Atual: R$ 18.000
Progresso: 60%

2. Compra de Imóvel
Meta: R$ 100.000
Atual: R$ 25.000
Progresso: 25%

ÚLTIMAS TRANSAÇÕES

05/01 - Supermercado - R$ 450
08/01 - Combustível - R$ 300
15/01 - Streaming - R$ 55
20/01 - Restaurante - R$ 180

ALERTAS GERADOS

- Gastos com alimentação aumentaram 22% no último mês.
- Assinaturas recorrentes somam R$ 280 mensais.
- Meta de reserva de emergência está dentro do cronograma.

PRODUTOS ELEGÍVEIS

- Tesouro Selic
- CDB Liquidez Diária
- Fundo Multimercado Moderado

INSTRUÇÕES AO AGENTE

1. Utilizar apenas dados presentes na base.
2. Não inventar informações.
3. Explicar cálculos realizados.
4. Não recomendar investimentos incompatíveis com o perfil.
5. Apresentar nível de confiança das análises.
6. Informar quando os dados forem insuficientes.
```

---

# Métricas Financeiras Calculadas pelo Agente

O agente também gera indicadores automaticamente:

| Indicador                | Fórmula                                |
| ------------------------ | -------------------------------------- |
| Taxa de Poupança         | (Receitas - Despesas) / Receitas       |
| Comprometimento de Renda | Dívidas / Renda                        |
| Reserva de Emergência    | Saldo Disponível / Gastos Mensais      |
| Evolução Patrimonial     | Patrimônio Atual - Patrimônio Anterior |
| Crescimento de Gastos    | Gasto Atual / Gasto Anterior           |
| Score Financeiro         | Índice composto calculado pelo agente  |
| Liquidez Imediata        | Recursos Disponíveis / Obrigações      |
| Taxa de Investimento     | Valor Investido / Renda                |

Essas métricas permitem que o agente forneça análises financeiras consistentes, personalizadas e baseadas em dados reais do usuário.

```
