# 8. Subagentes, Verificacao e Estado

> Faixa de tempo: 66:03-82:14

## Subagentes como gestao de contexto

Subagentes sao uteis quando uma tarefa exige muito trabalho intermediario, mas o agente principal so precisa do resultado final.

Exemplos:

- um subagente busca em uma aba da planilha;
- outro resume documentos;
- outro verifica seguranca;
- outro faz QA adversarial;
- outro pesquisa fontes externas.

O beneficio e reduzir poluicao de contexto. O agente principal nao precisa carregar todos os logs, tentativas e resultados brutos. Ele recebe uma sintese com evidencias.

Em planilhas grandes, o agente principal pode dividir leitura por abas. Cada subagente retorna achados, incertezas e referencias. O agente principal consolida e decide se precisa investigar mais.

## Verificacao deterministica primeiro

O workshop enfatiza: verifique deterministicamente sempre que possivel.

Antes de pedir a outro modelo para revisar, use regras:

- compila?
- testes passam?
- a query tem limite?
- o arquivo foi lido antes de editar?
- a faixa excede tamanho maximo?
- ha valores nulos?
- o formato de saida bate com schema?

Depois, quando regras nao bastam, use subagente de QA. A verificacao por modelo deve preferencialmente rodar em contexto separado, para evitar simpatia excessiva pelo trabalho produzido.

## Estado, checkpoints e undo

Estado e uma das dificuldades centrais de agentes. Em codigo, Git ajuda. Em uma UI ou planilha, erro pode criar estado confuso: adicionar item errado no carrinho, apagar celula, mover dado, alterar configuracao.

Boas praticas:

- criar checkpoint antes de edicoes;
- registrar cada acao;
- permitir rollback;
- separar leitura de escrita;
- pedir aprovacao para acoes irreversiveis;
- transformar sistemas complexos em operacoes reversiveis quando possivel.

Nota oficial: a documentacao atual inclui checkpointing para reverter alteracoes de arquivo no Agent SDK. Fonte: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).

## Workshop: dividir leitura e QA

Exemplo: agente responde pergunta sobre uma planilha com tres abas.

Divisao:

1. Subagente A: ler aba 1 e devolver candidatos.
2. Subagente B: ler aba 2 e devolver candidatos.
3. Subagente C: ler aba 3 e devolver candidatos.
4. Agente principal: consolidar evidencias.
5. Subagente QA: revisar conclusao sem ver a conversa completa.

Contrato de retorno dos subagentes:

```markdown
## Achado
- Resposta candidata:
- Fonte:
- Confianca:
- Lacunas:
- Proxima verificacao sugerida:
```

Esse contrato evita resposta solta e torna a consolidacao mais confiavel.
