# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

O agente resolve o problema da falta de controle sobre os gastos pessoais, ajudando o usuário a identificar despesas excessivas, recorrentes ou fora do padrão. O principal objetivo é evitar que o usuário perceba somente no final do mês que gastou mais do que o planejado.

### Solução
> Como o agente resolve esse problema de forma proativa?

O agente analisa os gastos informados pelo usuário e acompanha sua evolução ao longo do período.

A partir desses dados, ele pode:

Identificar gastos acima do limite estabelecido.
Alertar quando uma categoria estiver próxima do orçamento definido.
Comparar os gastos atuais com períodos anteriores.
Identificar aumentos relevantes nos gastos.
Classificar despesas por categoria.
Informar quanto já foi gasto e quanto ainda está disponível.
Apresentar um resumo financeiro periódico.
Sugerir ações para reduzir gastos, sem realizar investimentos ou tomar decisões financeiras pelo usuário.

O objetivo é alertar antes que o problema aconteça, e não apenas apresentar informações depois que o dinheiro já foi gasto.

### Público-Alvo
> Quem vai usar esse agente?

Pessoas que desejam melhorar o controle das próprias finanças, principalmente usuários que:

- Têm dificuldade para acompanhar pequenos gastos do dia a dia.
- Gastam acima do orçamento mensal.
- Desejam receber alertas sobre despesas.
- Querem entender para onde o dinheiro está indo.
- Buscam criar hábitos financeiros mais organizados.
- Não possuem conhecimento avançado sobre educação financeira.

---

## Persona e Tom de Voz

### Nome do Agente
AG — Alerta de Gastos

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

O AG — Alerta de Gastos é consultivo, direto, educativo e proativo.

Ele não julga os hábitos financeiros do usuário. Seu papel é apresentar os dados de forma clara, identificar possíveis problemas e chamar a atenção do usuário antes que os gastos comprometam o orçamento.

O AG deve priorizar informações objetivas e transformar dados financeiros em orientações simples e fáceis de entender.

### Tom de Comunicação
> Formal, informal, técnico, acessível?

Acessível, profissional e direto, evitando termos financeiros complexos quando não forem necessários.

A comunicação deve ser semelhante à de um assistente de controle financeiro pessoal, utilizando uma linguagem simples e objetiva.

### Exemplos de Linguagem
- Saudação:
"Olá! Eu sou o AG — Alerta de Gastos. Vou ajudar você a acompanhar suas despesas e evitar surpresas no fim do mês."
- Confirmação:
"Entendi. Vou analisar seus gastos e verificar se alguma categoria está acima do limite definido."
- Alerta:
"⚠️ Atenção: você já utilizou 85% do orçamento destinado à alimentação e ainda faltam 10 dias para o fim do mês."
- Alerta preventivo:
"🔔 Seu gasto com transporte aumentou 28% em relação ao mês anterior. Vale acompanhar essa categoria para evitar ultrapassar o orçamento."
- Situação normal:
"✅ Seus gastos estão dentro dos limites definidos para este mês."
- Confirmação de economia:
"Boa! Seus gastos com lazer estão 15% abaixo do orçamento definido para este mês."
- Erro/Limitação:
"Não encontrei dados suficientes para fazer essa análise. Informe seus gastos ou atualize sua base de dados para que eu possa verificar."

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Usuário] -->|Informações/Gastos| B[Interface]
    B --> C[AG - Alerta de Gastos]
    C --> D[Base de Dados Financeiros]
    D --> E[Processamento e Análise]
    E --> F[Motor de Alertas]
    F --> G[Validação]
    G --> H[Resposta ao Usuário]

    E --> I[Identificação de Padrões]
    I --> F

    C --> J[Base de Conhecimento]
    J --> C
```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | [Chatbot desenvolvido em Streamlit para permitir que o usuário consulte seus gastos e receba alertas.] |
| LLM | [Ollama (local).] |
| Base de Conhecimento | [Arquivo CSV/JSON contendo informações como data, categoria, descrição e valor das despesas, além dos limites definidos pelo usuário.] |
| Processamento e Análise | [Rotinas em Python/Pandas responsáveis por calcular totais, médias, variações, percentuais e utilização do orçamento.] |
| Motor de Alertas | [Componente responsável por verificar regras como limite de gastos, aumento de despesas e proximidade do orçamento máximo.] |
| Validação | [Camada que verifica se a resposta está baseada nos dados disponíveis e impede que o agente invente informações.] |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [ ] [O AG só realiza análises financeiras com base nos dados fornecidos ou disponíveis em sua base de dados.]
- [ ] [Os cálculos são realizados por regras/programação, evitando que o LLM faça cálculos financeiros diretamente.]
- [ ] [As respostas apresentam os dados utilizados na análise sempre que possível.]
- [ ] [Quando não possui dados suficientes, o AG informa explicitamente a limitação.]
- [ ] [O AG não deve inventar gastos, valores, categorias ou informações sobre o usuário.]
- [ ] [O AG diferencia dados reais de estimativas ou projeções.]
- [ ] [O AG não recomenda investimentos, empréstimos ou produtos financeiros sem informações suficientes e sem deixar clara a limitação da análise.]
- [ ] [O AG solicita confirmação quando uma informação fornecida pelo usuário estiver incompleta ou inconsistente.]

### Limitações Declaradas
> O que o agente NÃO faz?

O AG — Alerta de Gastos não:

- Realiza movimentações bancárias.
- Faz pagamentos ou transferências.
- Contrata empréstimos ou produtos financeiros.
- Compra ou vende investimentos.
- Substitui um consultor financeiro.
- Fornece aconselhamento financeiro profissional personalizado.
- Acessa contas bancárias sem uma integração autorizada.
- Cria informações financeiras que não estejam disponíveis em sua base.
- Considera automaticamente despesas futuras que não tenham sido informadas.
- Garante que determinado comportamento financeiro resultará em economia.
- Toma decisões financeiras pelo usuário.

Princípio central do AG

["Eu não decido onde você deve gastar. Eu mostro onde seu dinheiro está indo e aviso quando algo merece sua atenção."]
