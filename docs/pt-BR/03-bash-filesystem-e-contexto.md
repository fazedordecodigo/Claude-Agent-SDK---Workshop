# 3. Bash, Filesystem e Contexto

> Faixa de tempo: 15:25-20:34 e 29:20-34:07

## Bash como ferramenta generica

Uma tese central do workshop: Bash e uma das ferramentas mais poderosas para agentes. Em vez de criar uma ferramenta nova para cada caso, o agente pode usar comandos existentes, CLIs, scripts, `grep`, `jq`, gerenciadores de pacote, linters, testes e APIs chamadas por linha de comando.

Isso transforma Bash em "programmatic tool calling". O agente nao recebe apenas botoes predefinidos; recebe um ambiente onde consegue compor acoes. Ele pode descobrir comandos com `--help`, salvar saidas, criar scripts temporarios, chamar APIs e verificar resultados.

Essa liberdade custa mais cuidado. Bash pode ser perigoso, lento ou ruidoso. Por isso o workshop volta varias vezes a permissoes, parser de comandos, sandbox e verificacao.

## Filesystem como memoria e contexto

O filesystem aparece como mecanismo de engenharia de contexto. Em vez de enfiar tudo na conversa, o agente pode escrever arquivos, criar notas, salvar resultados longos, manter memoria e reutilizar scripts.

Essa ideia tambem explica skills. Uma skill e basicamente uma colecao de arquivos que o agente pode ler quando precisa de uma capacidade especializada. Ela permite disclosure progressivo: o agente nao carrega todo conhecimento sempre; descobre e abre o que importa para a tarefa atual.

Exemplo: uma skill de DOCX pode conter instrucoes, templates e scripts. O agente le a skill, gera codigo para manipular o documento e valida o resultado. O conhecimento nao precisa estar inteiro no prompt inicial.

## Exemplo: agente de e-mail

Pergunta: "quanto gastei com corridas esta semana?"

Sem Bash, um agente talvez receba 100 e-mails e tente raciocinar direto. Isso aumenta erro, custo e confusao.

Com Bash/filesystem, o fluxo melhora:

1. Buscar mensagens com termos como Uber e Lyft.
2. Salvar resultados brutos em arquivo.
3. Extrair valores com script ou comando.
4. Gerar uma tabela com e-mail, data, valor e fonte.
5. Somar valores.
6. Verificar amostras e duplicatas.

O ponto nao e que Bash faz magia. O ponto e que ele da ao agente instrumentos parecidos com os que uma pessoa usaria: pesquisar, anotar, filtrar, calcular e revisar.

## Workshop: transformar resultado longo em arquivo verificavel

Padrao pratico:

```text
Ferramenta externa -> arquivo bruto -> script de extracao -> arquivo resumido -> resposta final
```

Para agentes, prefira retornar caminhos de arquivos em vez de despejar saidas enormes no contexto. Isso permite releitura, auditoria e verificacao incremental.

Exercicio:

1. Escolha uma consulta que retorna muito texto.
2. Salve a saida em `raw-results.txt`.
3. Gere `summary.csv` com campos verificaveis.
4. Responda ao usuario citando quais campos sustentam a conclusao.

Esse padrao reduz poluicao de contexto e cria trilha de evidencias. Tambem facilita subagentes: um subagente pode trabalhar no arquivo bruto e devolver apenas conclusoes.
