# 4. Ferramentas, Bash e Geracao de Codigo

> Faixa de tempo: 25:32-48:16

## Ferramentas atomicas

Ferramentas estruturadas sao otimas quando a acao precisa ser controlada, auditada e pouco ambigua. O workshop cita escrita de arquivo e envio de e-mail como exemplos.

Uma boa ferramenta atomica:

- tem entrada e saida claras;
- limita o espaco de erro;
- permite aprovacao humana quando necessario;
- representa uma acao dificil de desfazer ou sensivel.

Enviar e-mail, aprovar pagamento, apagar registro ou alterar banco de producao nao devem depender de uma cadeia livre de comandos. Nesses casos, uma ferramenta com schema e permissoes explicitas e mais segura.

## Bash para composicao

Bash brilha quando a tarefa exige compor varias acoes. Exemplos:

- trocar de pasta;
- usar GitHub CLI;
- rodar linter e testes;
- chamar uma CLI interna;
- encadear `grep`, `jq`, `sort`, `uniq`;
- guardar memoria em arquivos;
- descobrir subcomandos via `--help`.

O trade-off e latencia e descoberta. Uma ferramenta estruturada ja vem descrita no contexto. Uma CLI descoberta via Bash precisa ser explorada, o que custa tempo, mas economiza tokens e aumenta flexibilidade.

## Geracao de codigo para tarefas nao codigo

"Geracao de codigo para nao-codigo" significa criar scripts para resolver tarefas de negocio, dados ou pesquisa. O usuario pede algo em linguagem natural; o agente escreve codigo auxiliar para buscar API, limpar dados, analisar arquivos ou montar relatorios.

Exemplo do workshop: pedir clima em Sao Francisco e recomendacao de roupa. Um agente pode escrever um script para chamar API de clima, detectar localizacao, combinar com dados de guarda-roupa e gerar resposta. O codigo nao e o produto final; e a ferramenta temporaria.

Isso tambem vale para:

- analise de planilhas;
- pesquisa em documentos;
- criacao de PowerPoint;
- limpeza de dados;
- conversao de formatos;
- consultas SQL dinamicas.

## Workshop: escolher a interface certa

Use esta matriz:

| Necessidade | Melhor candidato |
| --- | --- |
| Acao sensivel, irreversivel ou com aprovacao | Ferramenta atomica |
| Composicao de comandos existentes | Bash |
| Logica dinamica, analise ou transformacao | Geracao de codigo |
| Consulta altamente estruturada e segura | API/ferramenta com schema |
| Exploracao livre em dominio complexo | Bash + scripts + arquivos |

Pergunta de design: "o agente precisa de poder ou de garantia?"

Se precisa de garantia, reduza liberdade. Se precisa de poder, de ambiente e verificacao. Em bancos de dados, por exemplo, uma ferramenta e boa quando a consulta precisa mascarar dados sensiveis ou restringir acesso. Para analise exploratoria, SQL via script pode ser melhor, desde que credenciais e permissoes estejam bem escopadas.

Nota oficial: o SDK oferece ferramentas embutidas, mas tambem permite ferramentas customizadas e integracao com MCP. Fonte: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).
