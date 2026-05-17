# 6. Design de Agentes

> Faixa de tempo: 21:46-43:44 e 83:40-86:00

## Ler transcricoes de execucao

A principal recomendacao de design e simples: leia as transcricoes do agente repetidamente.

Ao observar uma execucao, pergunte:

- O que ele tentou primeiro?
- Onde buscou contexto?
- Que ferramenta escolheu?
- Onde se confundiu?
- Que feedback teria ajudado?
- Ele verificou algo ou apenas respondeu?
- A resposta final tem evidencias?

Esse diagnostico vale mais que teoria generica. Cada dominio tem atritos proprios. Um agente de suporte, planilha, seguranca ou e-mail erra de formas diferentes.

## Progressive disclosure com skills

Skills sao apresentadas como uma forma de disclosure progressivo. Em vez de despejar um manual inteiro no prompt, voce coloca conhecimento em arquivos e deixa o agente abrir quando precisar.

Uma skill funciona bem quando:

- a tarefa e recorrente;
- exige conhecimento especializado;
- contem passos ou scripts reutilizaveis;
- nao precisa estar sempre no contexto;
- ajuda o agente a operar em um formato especifico.

A conversa tambem compara skills, APIs e arquivos auxiliares. Nao existe regra universal. Se o agente sempre precisa de uma API, talvez um `api.ts` ou `client.py` com exemplos seja melhor. Se precisa de instrucoes especializadas e templates, skill faz sentido.

## APIs, scripts e descoberta

Para scripts e CLIs customizadas, o workshop recomenda descoberta pelo proprio ambiente. Coloque scripts no filesystem, diga ao agente que existem e implemente `--help`.

Um bom CLI agentico:

- explica subcomandos;
- mostra exemplos;
- retorna erros claros;
- salva artefatos em arquivos;
- usa saida estruturada quando possivel;
- evita despejar resultado gigante no terminal.

Essa abordagem combina com o Agent SDK porque Bash e filesystem estao no mesmo ambiente. Se a ferramenta salva arquivo em um lugar e Bash roda em outro container, o agente perde composabilidade.

## Workshop: prototipar antes de produzir

O caminho sugerido:

1. Prototipe no Claude Code.
2. Dê ao agente uma API, scripts e dados de exemplo.
3. Peca tarefas reais.
4. Leia a transcricao.
5. Ajuste scripts, instrucoes, permissoes e formato dos dados.
6. So entao transforme em agente via SDK.

O agente final pode ter pouco codigo. "Simples" nao quer dizer "facil": a dificuldade esta em descobrir a interface certa entre modelo, dados e ferramentas.

Exemplo: transformar uma planilha em SQL pode ser o insight que torna o agente confiavel. Nao e muito codigo, mas e uma boa escolha de representacao.

Nota oficial: a pagina de uso programatico mostra que o Agent SDK pode ser usado via CLI, Python ou TypeScript, inclusive em modo nao interativo com `claude -p`. Fonte: [Run Claude Code programmatically](https://code.claude.com/docs/en/headless).
