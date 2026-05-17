# 2. Agentes vs Workflows

> Faixa de tempo: 11:28-24:59

## Quando usar workflow

Workflow e a melhor escolha quando entrada, saida e processo sao bem definidos. Um exemplo citado e automacao no estilo GitHub Actions: receber um PR, gerar um resumo, rodar validacoes, publicar resultado.

O Claude Agent SDK tambem pode ser usado em workflows, especialmente quando ha partes variaveis no meio. Um bot de triagem de issues parece workflow, mas pode precisar clonar repositorio, procurar arquivos, subir container, reproduzir bug e so entao gerar uma classificacao estruturada. Nesse caso, o contorno externo e workflow; o miolo e agentico.

## Quando usar agente

Agente e indicado quando o usuario quer conversar em linguagem natural e deixar o sistema agir de modo flexivel. Exemplos:

- consultar dados de negocio;
- montar dashboards;
- investigar bugs;
- automatizar Slack ou GitHub;
- explorar bases internas;
- criar documentos, planilhas ou scripts.

A diferenca nao e "tem LLM" vs "nao tem LLM". A diferenca e quem escolhe a trajetoria. Em um workflow classico, a aplicacao conduz. Em um agente, a aplicacao entrega ferramentas, contexto inicial e limites; o agente conduz uma parte do percurso.

## O loop: contexto, acao e verificacao

O loop apresentado tem tres fases:

1. Reunir contexto.
2. Tomar acao.
3. Verificar o trabalho.

Para Claude Code, reunir contexto significa localizar arquivos relevantes. Para um agente de e-mail, significa encontrar mensagens certas. Para um agente financeiro, pode significar descobrir a aba, tabela, periodo e metrica corretos.

Tomar acao envolve ferramentas, Bash, scripts, edicoes ou chamadas de API. Verificar e o ponto que separa uma demo bonita de um agente confiavel. Codigo e um dominio forte porque compila, roda testes e permite diff. Pesquisa profunda e mais dificil: citacoes ajudam, mas nao sao equivalentes a um compilador.

Planos entram entre contexto e acao quando o problema pede raciocinio passo a passo. Eles aumentam controle e auditabilidade, mas tambem aumentam latencia.

## Workshop: desenhar o loop de um agente

Escolha uma tarefa e preencha:

| Fase | Pergunta | Exemplo |
| --- | --- | --- |
| Contexto | O que o agente precisa encontrar? | E-mails de recibo da semana |
| Acao | O que ele precisa fazer? | Extrair valores, normalizar moeda, somar |
| Verificacao | Como saber se esta certo? | Arquivo com cada recibo, valor e linha de origem |

Depois pergunte: o agente precisa mesmo receber todo o contexto pronto? No workshop, a recomendacao e deixar o agente buscar contexto sempre que possivel. Em vez de jogar uma pilha de documentos na janela de contexto, de ao agente instrumentos para pesquisar, filtrar, salvar evidencias e voltar nelas.

Nota oficial: a documentacao do SDK lista ferramentas nativas para leitura, edicao, Bash, glob, grep, busca web e outras capacidades, alem de hooks, subagentes, MCP, permissoes e sessoes. Fonte: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).
