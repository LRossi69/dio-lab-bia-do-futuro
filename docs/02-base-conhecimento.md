# Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `transacoes.csv` | CSV | Registrar e analisar as despesas realizadas pelo usuário, identificando valores, categorias, datas e padrões de consumo. |
| `orcamento.json` | JSON | Armazenar os limites de gastos definidos pelo usuário para cada categoria e para o período. |
| `perfil_financeiro.json` | JSON | Armazenar informações básicas necessárias para personalizar o acompanhamento, como renda mensal, orçamento disponível e objetivos de controle de gastos. |
| `historico_alertas.csv` | CSV | Registrar alertas emitidos anteriormente, permitindo acompanhar situações recorrentes e verificar a evolução dos gastos. |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Sim. Os dados mockados foram adaptados para representar uma situação realista de controle de gastos pessoais.

A base transacoes.csv foi estruturada para conter informações como:

- Data da transação;
- Categoria do gasto;
- Descrição da despesa;
- Valor;
- Forma de pagamento;
- Tipo de despesa, como fixa ou variável.

Também foi criada a base orcamento.json, contendo os limites definidos para cada categoria de gasto.

Exemplo:

{
  
  "mes": "09/2026",
  
  "limites": 
  
  {
    
    "Alimentação": 1200,
    
    "Transporte": 800,
    
    "Lazer": 500,
    
    "Moradia": 1800,
    
    "Saúde": 400
  }
  
}

A base perfil_financeiro.json foi adaptada para conter somente informações necessárias ao controle financeiro, evitando dados desnecessários para o funcionamento do AG.

Exemplo:

{
  "renda_mensal": 5000,
  "orcamento_mensal": 4000,
  "objetivo": "reduzir gastos variáveis"
}

Também foi criada a base historico_alertas.csv para registrar os alertas gerados pelo agente, permitindo identificar situações recorrentes.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os arquivos CSV e JSON são carregados pelo sistema durante a execução do agente.

Os dados de transações são processados utilizando Python e Pandas para calcular indicadores como:

- Total de gastos;
- Gastos por categoria;
- Percentual do orçamento utilizado;
- Média de gastos;
- Comparação com períodos anteriores;
- Categorias próximas ou acima do limite;
- Variação dos gastos.

Os arquivos JSON são utilizados para consultar informações relacionadas ao orçamento e às características financeiras definidas pelo usuário.

O agente não precisa carregar todas as informações diretamente no prompt. Os dados relevantes são consultados e processados conforme a solicitação do usuário, reduzindo o volume de informações enviado ao modelo de linguagem.

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Os dados são consultados dinamicamente.

O LLM recebe somente as informações relevantes para responder à solicitação do usuário. Os cálculos e regras de negócio são realizados previamente pelo sistema, e o resultado é encaminhado ao LLM para que seja transformado em uma resposta clara e natural.

Por exemplo:

1 - O usuário pergunta: "Como estão meus gastos este mês?"

2 - O sistema consulta transacoes.csv.

3 - Os gastos são agrupados por categoria.

4 - O sistema consulta orcamento.json.

5 - Os valores gastos são comparados com os limites definidos.

6 - O sistema identifica possíveis alertas.

7 - O resultado é enviado ao LLM.

8 - O LLM apresenta a análise ao usuário em linguagem simples.

Essa estratégia reduz o risco de o agente inventar informações ou realizar cálculos incorretos.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
CONTEXTO DO AG — ALERTA DE GASTOS

Período analisado: Setembro/2026

Perfil financeiro:
- Renda mensal: R$ 5.000,00
- Orçamento mensal: R$ 4.000,00
- Objetivo: Reduzir gastos variáveis

ORÇAMENTO POR CATEGORIA:
- Alimentação: R$ 1.200,00
- Transporte: R$ 800,00
- Lazer: R$ 500,00
- Moradia: R$ 1.000,00
- Saúde: R$ 500,00

GASTOS REALIZADOS:

Alimentação:
- Supermercado: R$ 450,00
- Restaurante: R$ 180,00
- Delivery: R$ 150,00
Total: R$ 780,00
Utilização do orçamento: 65%

Transporte:
- Combustível: R$ 350,00
- Aplicativo de transporte: R$ 180,00
Total: R$ 530,00
Utilização do orçamento: 66,25%

Lazer:
- Cinema: R$ 80,00
- Streaming: R$ 120,00
- Restaurante: R$ 150,00
Total: R$ 350,00
Utilização do orçamento: 70%

RESUMO:
- Total gasto: R$ 1.660,00
- Orçamento mensal: R$ 4.000,00
- Orçamento utilizado: 41,5%
- Saldo do orçamento: R$ 2.340,00

ALERTAS IDENTIFICADOS:
- Lazer atingiu 70% do orçamento disponível.
- Alimentação atingiu 65% do orçamento disponível.
- Não há categorias acima do limite neste momento.

INSTRUÇÃO AO AG:
- Analise os dados acima e responda de forma objetiva.
- Não invente informações.
- Utilize somente os dados fornecidos.
- Caso os dados não sejam suficientes para responder, informe a limitação.
- Priorize alertas preventivos e explique de forma simples quais categorias merecem atenção.

Exemplo de resposta esperada do AG

"Seu orçamento está sob controle neste momento."

"Você já utilizou 41,5% do orçamento mensal e ainda possui
R$ 2.340,00 disponíveis."

"⚠️ Pontos de atenção:
- Lazer: 70% do limite já utilizado.
- Alimentação: 65% do limite já utilizado."

"Nenhuma categoria ultrapassou o limite definido até o momento."

"Recomendo acompanhar principalmente Lazer e Alimentação
nos próximos dias para evitar que essas categorias ultrapassem
o orçamento planejado."

Regra principal da Base de Conhecimento

O AG — Alerta de Gastos deve utilizar os dados financeiros como fonte principal para suas análises. O LLM interpreta e comunica os resultados, mas não deve inventar dados nem substituir os cálculos realizados pelo sistema.
...
```
