# 10. Producao, Hooks, Hosting e Monetizacao

> Faixa de tempo: 99:31-112:02

## Do prototipo ao SDK

Depois de prototipar em Claude Code, o proximo passo e transformar o comportamento em uma aplicacao. No exemplo, o arquivo do agente tem poucas linhas: define diretorio de trabalho, chama o SDK, configura ferramentas permitidas e executa o loop.

A parte dificil nao e escrever muito codigo. E descobrir:

- quais arquivos o agente precisa;
- quais scripts devem existir;
- que permissoes sao seguras;
- como aprovar ferramentas;
- como guardar sessoes;
- que verificacoes rodam em cada etapa;
- como expor resultado ao usuario.

Nota oficial: a documentacao mostra uso programatico via CLI, Python e TypeScript, incluindo `claude -p` para execucao nao interativa e opcoes como ferramentas permitidas e formato de saida. Fonte: [Run Claude Code programmatically](https://code.claude.com/docs/en/headless).

## Hooks para controle deterministico

Hooks sao pontos de extensao para validar, bloquear, logar ou inserir contexto durante o ciclo do agente.

No workshop, hooks aparecem como resposta para varios problemas:

- impedir resposta sem executar script quando a tarefa exige dados;
- verificar planilha apos cada acao;
- inserir mudancas feitas pelo usuario entre tool calls;
- reforcar regra "leia antes de escrever";
- criar trilha de auditoria;
- rodar validacoes intermediarias.

Nota oficial: a documentacao lista hooks como callbacks em eventos do ciclo do agente, como antes/depois de uso de ferramenta, inicio/fim de sessao e envio de prompt. Fonte: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).

## Hosting local, sandbox e UI adaptativa

Ha duas formas citadas para distribuir agentes:

1. Aplicacao local: o usuario instala e o agente opera no computador dele.
2. Sandbox remoto: cada usuario recebe ambiente isolado com filesystem e comandos.

Para interfaces adaptativas, o workshop sugere rodar um dev server dentro do sandbox. O agente pode editar codigo, a UI recarrega, e o usuario interage com uma experiencia feita sob medida. Esse padrao lembra builders de sites: sandbox + servidor + edicao ao vivo.

Essa arquitetura muda o produto. A UI pode deixar de ser fixa e virar algo que o agente ajusta conforme a tarefa: time builder de Pokemon, painel de planilhas, visualizacao de dados ou ferramenta de revisao.

## Custo e produto

Agentes podem ser caros porque modelos fortes trabalham por mais tempo, usam ferramentas e fazem varias iteracoes. O workshop recomenda pensar monetizacao cedo.

Perguntas de produto:

- O problema e valioso o bastante para pagar pelo custo?
- O uso e frequente ou ocasional?
- Faz sentido assinatura, limite de uso ou cobranca por consumo?
- O agente precisa de modelos mais caros para todos os passos?
- Quais verificacoes evitam retrabalho?

Promessas de preco e uso sao dificeis de desfazer. Portanto, o design tecnico e o modelo comercial precisam conversar desde o inicio.

## Encerramento: principios praticos

- Comece pelo problema que o usuario pagaria para resolver.
- Prototipe no Claude Code antes de cristalizar arquitetura.
- De ao agente filesystem, Bash e dados bem organizados.
- Use ferramentas atomicas para acoes sensiveis.
- Verifique deterministicamente sempre que possivel.
- Use subagentes para trabalho paralelo e QA.
- Isole com sandbox e credenciais escopadas.
- Leia transcricoes de execucao como principal instrumento de melhoria.
