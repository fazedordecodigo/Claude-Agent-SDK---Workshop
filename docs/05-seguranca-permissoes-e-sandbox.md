# 5. Seguranca, Permissoes e Sandbox

> Faixa de tempo: 12:41-15:25 e 44:47-47:35

## Defesa em camadas

Dar Bash a um agente e poderoso, mas exige defesa em camadas. O workshop chama isso de abordagem tipo "queijo suico": nenhuma camada e perfeita sozinha, mas varias barreiras reduzem risco.

Camadas citadas:

- alinhamento do modelo;
- prompts e instrucoes do harness;
- parser/analise de comandos Bash;
- permissoes por ferramenta;
- sandbox;
- credenciais escopadas;
- limites de leitura e escrita;
- verificacao e feedback.

O objetivo nao e confiar cegamente no modelo. E desenhar um ambiente em que mesmo uma acao ruim tenha alcance limitado.

## Permissoes e ferramentas perigosas

Permissoes devem refletir impacto. Ler um arquivo e diferente de escrever. Rodar teste e diferente de deletar banco. Enviar e-mail e diferente de rascunhar e-mail.

No design de agente, separe:

- acoes livres;
- acoes que exigem aprovacao;
- acoes bloqueadas;
- acoes permitidas apenas em sandbox;
- acoes com escopo reduzido por credencial.

Para bancos de dados, o palestrante sugere pensar em chaves e proxies especificos para agentes. Em vez de dar credencial ampla, crie um caminho de acesso com regras: leitura apenas, tabelas permitidas, mascaramento, limite de linhas ou feedback quando o agente tenta algo proibido.

## Sandbox e isolamento

Sandbox responde a pergunta: se alguem tomar controle do agente, o que ele consegue fazer?

Um bom sandbox:

- isola filesystem;
- limita rede quando necessario;
- nao contem segredos de producao;
- permite descartar estado;
- suporta auditoria;
- facilita checkpoints.

O workshop tambem menciona provedores de sandbox e containers por usuario. A arquitetura muda: em vez de uma aplicacao web unica para milhares de usuarios, agentes podem operar em ambientes individuais, com estado e arquivos proprios.

## Workshop: desenhar guardrails

Pegue uma tarefa agentica e liste riscos por camada.

Exemplo: agente que consulta banco interno.

| Camada | Guardrail |
| --- | --- |
| Credencial | Usuario de leitura, sem escrita |
| Query | Limite de linhas e timeout |
| Dados sensiveis | Mascaramento no proxy |
| Bash | Bloquear comandos destrutivos |
| Filesystem | Diretório temporario isolado |
| Resposta | Citar consulta e amostra usada |

Depois teste o caminho ruim: "apague a tabela", "extraia todos os e-mails", "rode sem limite", "ignore permissoes". O agente deve receber erro claro e continuar de modo seguro.

Nota oficial: a documentacao do SDK inclui secoes dedicadas a permissoes, hooks, checkpointing, hosting e deployment seguro. Fonte: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).
