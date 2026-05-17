# 1. Introducao ao Claude Agent SDK

> Faixa de tempo: 00:00-10:00

## O que o workshop cobre

O workshop apresenta o Claude Agent SDK como uma forma de construir agentes de IA usando o mesmo tipo de harness que impulsiona o Claude Code. A proposta nao e apenas mostrar uma demo pronta, mas pensar em voz alta sobre como um agente e desenhado: quais ferramentas recebe, como cria contexto, como age, como verifica o proprio trabalho e como isso muda quando o dominio nao e programacao.

A agenda central passa por quatro perguntas:

- O que e um agente?
- Por que usar um framework agentico?
- Como projetar um agente com o SDK?
- Como prototipar um agente real, observando o loop e ajustando o ambiente?

O tom do workshop e pratico. A ideia nao e vender uma receita fechada. Projetar loops de agente ainda tem bastante intuicao: ler execucoes, notar onde o modelo se perde, ajustar ferramentas, criar feedback deterministico e repetir.

## Da feature unica ao agente autonomo

O palestrante descreve uma evolucao em tres etapas.

Primeiro vieram features isoladas com LLM: classificar um texto, resumir uma mensagem, retornar uma resposta em formato estruturado. O escopo era pequeno e o caminho de execucao era definido pela aplicacao.

Depois vieram workflows: fluxos mais estruturados, como rotular e-mails, indexar uma base com RAG, gerar uma completacao de codigo ou executar um processo previsivel. Ainda ha liberdade do modelo, mas a aplicacao conhece bem entrada, saida e etapas.

Agentes entram quando a aplicacao deixa o modelo construir parte relevante da trajetoria. Em vez de dizer "chame esta ferramenta e depois aquela", voce entrega uma meta em linguagem natural e um ambiente de trabalho. O agente decide quais arquivos ler, quais comandos executar, quais scripts criar, quais evidencias coletar e quando pedir ajuda.

Claude Code aparece como exemplo canonico: um agente capaz de trabalhar por muitos minutos, explorar uma base, editar arquivos, rodar comandos, testar e ajustar.

## Por que construir sobre Claude Code

O Claude Agent SDK nasce da observacao de que varias equipes acabavam reconstruindo as mesmas pecas:

- modelos;
- ferramentas;
- loop de execucao;
- prompts de sistema;
- filesystem;
- compactacao;
- memoria;
- skills;
- subagentes;
- busca;
- hooks;
- permissoes;
- sandbox.

Empacotar essas pecas reduz o trabalho repetitivo e traz opinioes acumuladas em producao. A tese do workshop e forte: para muitos agentes, Bash e filesystem nao sao detalhes tecnicos; eles sao parte do design do produto.

Nota oficial: a documentacao atual descreve o Agent SDK como uma biblioteca para criar agentes que leem arquivos, executam comandos, pesquisam, editam codigo e usam o mesmo loop/contexto do Claude Code em Python ou TypeScript. Fonte: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).

## Workshop: reconhecer um problema agentico

Use este filtro antes de escolher o SDK:

1. O usuario consegue expressar a meta em linguagem natural?
2. O caminho ate a resposta varia conforme o contexto?
3. O agente precisa buscar informacao antes de agir?
4. Ha uma forma razoavel de verificar o resultado?
5. A acao pode ser protegida por permissoes, sandbox ou revisao humana?

Se a resposta for "sim" para a maior parte, o problema tem cara de agente. Se entrada e saida forem fixas e o caminho for previsivel, talvez seja melhor com workflow tradicional ou ferramenta atomica.

Exemplo pratico: "quanto gastei com corridas esta semana?" parece simples, mas exige buscar e-mails, identificar Uber/Lyft, extrair valores, somar, lidar com recibos duplicados e verificar se cada valor realmente e uma corrida. O agente se beneficia de ferramentas para pesquisar, salvar resultados intermediarios e auditar a propria resposta.
