# 7. Agente para Planilhas

> Faixa de tempo: 50:42-65:17 e 72:53-77:53

## Buscar em planilhas e bases grandes

O exemplo de planilha pergunta: qual e a receita em 2026? Parece simples, mas uma planilha real pode ter varias abas, cabecalhos irregulares, anos em colunas, metricas em linhas, formulas, totais parciais e dados ocultos.

O erro comum e carregar tudo no contexto. O workshop recomenda pensar como uma pessoa: primeiro olhar a area visivel, procurar termos relevantes, navegar por abas, anotar referencias e aprofundar apenas onde ha sinal.

Para bases grandes, a acuracia cai se o agente tenta ler tudo. Melhor e criar interfaces de busca:

- procurar texto;
- listar abas;
- ler faixas;
- converter CSV em SQLite;
- consultar via SQL;
- expor XML do XLSX quando util;
- manter scratchpad com referencias.

## Interfaces conhecidas pelo modelo

Modelos tendem a operar melhor com formatos familiares. Por isso, uma parte criativa do design e converter o problema para uma interface "em distribuicao".

Exemplos:

- planilha para SQL;
- intervalo `B3:F20`;
- CSV normalizado;
- XML estruturado;
- tabela com metadados;
- resumo por aba.

Se o agente sabe SQL, aproveite isso. Se sabe ranges de planilha, exponha ranges. Se sabe ler XML, use XML quando fizer sentido. O objetivo e diminuir a distancia entre dados proprietarios e formatos que o modelo entende bem.

## Verificacao e reversibilidade

Para planilhas, verificacao pode incluir:

- checar celulas de origem;
- comparar soma com total;
- detectar valores nulos;
- validar formulas;
- limitar faixas consultadas;
- manter copia/checkpoint antes de editar;
- registrar cada alteracao.

Reversibilidade importa. Codigo e facil de desfazer com Git. Planilha, UI e sistemas externos acumulam estado. Se o agente muda uma celula errada, precisa haver checkpoint, historico ou operacao de undo.

## Workshop: responder receita em 2026

Fluxo recomendado:

1. Listar abas.
2. Buscar por `receita`, `revenue`, `2026` e sinonimos.
3. Ler faixas proximas aos resultados.
4. Identificar se anos estao em colunas e metricas em linhas.
5. Salvar scratchpad com candidatos: aba, range, celula, valor.
6. Verificar se o valor e total, subtotal ou item.
7. Responder com valor e referencia.

Exemplo de resposta boa:

```text
A receita de 2026 parece estar em `Forecast!E12`, valor R$ 1.203.000. Verifiquei que a linha representa "Receita total" e que a coluna E corresponde a 2026. Tambem comparei com o subtotal da faixa E8:E11.
```

Esse formato deixa claro o caminho de verificacao. O usuario pode auditar.
