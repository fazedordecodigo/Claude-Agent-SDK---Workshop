# 9. Workshop: Agente Pokemon

> Faixa de tempo: 86:00-105:20

## Objetivo do prototipo

O workshop usa Pokemon como dominio de exemplo porque combina API rica, muitos dados, regras especificas e perguntas abertas. O usuario pode perguntar sobre tipos, geracoes, habilidades, movimentos ou montagem de time competitivo.

O objetivo e construir um agente capaz de conversar sobre Pokemon e usar dados reais, nao apenas conhecimento memorizado pelo modelo.

## Ferramentas vs Claude Code

O palestrante compara duas abordagens.

Na primeira, uma aplicacao tradicional usa ferramentas especificas: `getPokemon`, `getSpecies`, `getAbility`, `getMove` e outras. Isso funciona para perguntas previstas, mas cresce mal se a API tem muitas entidades. O modelo tambem precisa escolher entre varias ferramentas parecidas.

Na segunda, Claude Code recebe uma pequena biblioteca TypeScript ou acesso a dados e escreve scripts para responder. Para a pergunta "quais sao os Pokemon de agua da geracao 2?", ele pode buscar listas, iterar, checar tipos e produzir a resposta.

O ponto pedagogico: codigo gerado permite combinar chamadas, filtrar e verificar em escala, sem criar manualmente uma ferramenta para cada caminho.

## Analisar dados competitivos

O exemplo avanca para dados textuais de Pokemon competitivo. A base contem informacoes sobre quem combina com quem, counters, papeis e relacoes.

O agente precisa:

- procurar `Venusaur`;
- encontrar entradas relacionadas;
- identificar companheiros comuns;
- separar counters de parceiros;
- criar script para analisar coocorrencias;
- sugerir time com justificativa.

Esse caso e interessante porque nao e apenas consulta de API. E analise de texto semi-estruturado. O agente usa busca, scripts e sintese.

## Workshop: time ao redor de Venusaur

Prompt de exemplo:

```text
Quero montar um time competitivo ao redor de Venusaur. Use os dados disponiveis, identifique parceiros recorrentes, ameacas comuns e sugira uma composicao inicial com justificativa.
```

Fluxo esperado:

1. Buscar mencoes a Venusaur.
2. Extrair trechos proximos.
3. Identificar Pokemon citados como parceiros.
4. Identificar counters e ameacas.
5. Gerar tabela com papel, motivo e fonte textual.
6. Propor composicao.
7. Avisar incertezas e proximos testes.

Resposta boa nao deve apenas listar nomes. Deve explicar papeis: suporte, cobertura de tipo, pressao ofensiva, resposta a counters e lacunas do time.

Aprendizado central: quando dados estao em arquivos, o agente pode construir a propria ferramenta de analise. O design deve facilitar isso com dados acessiveis, scripts permitidos e verificacao.
