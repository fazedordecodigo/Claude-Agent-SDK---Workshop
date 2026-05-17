# Claude Agent SDK — Documentação do Workshop

> **Fonte:** Workshop de ~2h sobre o Claude Agent SDK (Anthropic)
> **Apresentador:** Engenheiro da Anthropic
> **Tópico:** Construção de Agentes com o Claude Agent SDK
> **Tradução:** Transcrição traduzida para português brasileiro e formatada como documentação técnica

---

## Sumário

1. [Introdução: O que é o Claude Agent SDK](#1-introdução-o-que-é-o-claude-agent-sdk)
2. [Evolução das Capacidades de IA](#2-evolução-das-capacidades-de-ia)
3. [Componentes de um Harness de Agente](#3-componentes-de-um-harness-de-agente)
4. [Por que o Claude Agent SDK?](#4-por-que-o-claude-agent-sdk)
5. [O Bash como Ferramenta Mais Poderosa](#5-o-bash-como-ferramenta-mais-poderosa)
6. [Workflows vs Agentes](#6-workflows-vs-agentes)
7. [Projetando o Loop de um Agente](#7-projetando-o-loop-de-um-agente)
8. [Ferramentas vs Bash vs Geração de Código](#8-ferramentas-vs-bash-vs-geração-de-código)
9. [Skills (Habilidades)](#9-skills-habilidades)
10. [Sub-Agentes e Gerenciamento de Contexto](#10-sub-agentes-e-gerenciamento-de-contexto)
11. [Segurança e Guardrails](#11-segurança-e-guardrails)
12. [Verificação e Hooks](#12-verificação-e-hooks)
13. [Exemplo de Design: Agente de Planilhas](#13-exemplo-de-design-agente-de-planilhas)
14. [Live Coding: Agente Pokémon](#14-live-coding-agente-pokémon)
15. [Hospedagem e Sandbox](#15-hospedagem-e-sandbox)
16. [Monetização de Agentes](#16-monetização-de-agentes)
17. [Considerações sobre Contexto](#17-considerações-sobre-contexto)
18. [Melhores Práticas Gerais](#18-melhores-práticas-gerais)

---

## 1. Introdução: O que é o Claude Agent SDK

O **Claude Agent SDK** é um kit de desenvolvimento construído sobre o **Claude Code** que permite criar agentes de IA de forma opinativa e estruturada.

### 1.1 Motivação

Ao construir agentes internamente na Anthropic, a equipe percebeu que precisava reconstruir as mesmas peças repetidamente. O SDK empacota todas essas peças para que possam ser reutilizadas.

### 1.2 Para quem é

- Engenheiros de software construindo agentes de código
- Profissionais de finanças, dados e marketing automatizando tarefas
- Qualquer pessoa que precise criar agentes não relacionados a código (non-coding agents)

### 1.3 Aplicações Populares

- Agentes de engenharia de software (confiabilidade, segurança)
- Agentes de triagem de bugs
- Construtores de dashboards
- Agentes de escritório (Office agents)
- Agentes para finanças, jurídico e saúde

---

## 2. Evolução das Capacidades de IA

O palestrante traça uma linha evolutiva clara:

### 2.1 Chamada Única de LLM (Single LLM Call)

- **Exemplo:** "Classifique isto em categorias e retorne no formato JSON"
- Uso mais básico — prompt único, resposta única
- Sem estado, sem loop, sem ferramentas

### 2.2 Workflows (Fluxos de Trabalho)

- **Exemplo:** "Pegue este e-mail, rotule-o e, com base no resultado, indexe no RAG"
- Mais estruturados, com etapas bem definidas
- Entradas e saídas fixas
- Similar a GitHub Actions

### 2.3 Agentes

- **Exemplo:** Claude Code — você não restringe o que ele pode fazer
- Conversa-se em linguagem natural
- O agente constrói seu próprio contexto
- Decide sua própria trajetória de ações
- Trabalha de forma autônoma por longos períodos (10, 20, 30 minutos)
- **O Claude Code foi descrito como o primeiro agente verdadeiro**

---

## 3. Componentes de um Harness de Agente

O SDK empacota todos estes componentes:

### 3.1 Modelos (Models)

- A base do sistema — o cérebro do agente

### 3.2 Ferramentas (Tools)

- Primeiro passo óbvio: adicionar ferramentas ao modelo
- Ferramentas customizadas ou pré-construídas (sistema de arquivos, busca, etc.)

### 3.3 Loop de Execução

- As ferramentas rodam em um loop contínuo
- O agente decide qual ferramenta chamar a cada passo

### 3.4 Prompts

- O prompt central do agente e prompts auxiliares
- Engenharia de prompt para orientar o comportamento

### 3.5 Sistema de Arquivos (File System)

- **Principal mecanismo de engenharia de contexto**
- O agente usa o sistema de arquivos para armazenar e recuperar informações
- Contexto não está apenas no prompt — está nos arquivos disponíveis

### 3.6 Skills (Habilidades)

- Funcionalidade recentemente lançada
- Coleções de instruções e expertise que o agente pode carregar

### 3.7 Outros Componentes

- Sub-agentes
- Busca na web
- Compressão de contexto
- Memória
- Hooks

---

## 4. Por que o Claude Agent SDK?

### 4.1 Construído sobre Claude Code

- Quando o Claude Code foi lançado, engenheiros começaram a usá-lo
- Depois vieram finanças, dados, marketing
- Pessoas usavam Claude Code para tarefas não relacionadas a código
- Ao construir agentes não relacionados a código, a equipe sempre voltava ao Claude Code

### 4.2 Princípio do Bash

> "Spoiler: o bash é a razão pela qual podemos usar Claude Code para tarefas não relacionadas a código."

O bash permite qualquer ação em um sistema Unix — o agente pode fazer literalmente qualquer coisa através dele.

### 4.3 Lições Aprendidas em Escala

- Erros de uso de ferramentas (tool use errors)
- Compressão de contexto
- Melhores práticas descobertas com milhões de execuções
- O SDK é **opinativo** — incorpora as melhores práticas da Anthropic

### 4.4 Analogia: React vs jQuery

> "O Agent SDK é como o React dos frameworks de agente — nós construímos nossas próprias coisas em cima dele, então sabemos que é real. E todas as partes chatas são coisas que nós mesmos enfrentamos."

---

## 5. O Bash como Ferramenta Mais Poderosa

### 5.1 Por que Bash?

Uma das principais opiniões do SDK é que **o bash é a ferramenta mais poderosa que um agente pode ter**.

### 5.2 O que o Bash Permite

- Armazenar resultados de chamadas de ferramentas em arquivos
- Persistir memória
- Gerar scripts dinamicamente e executá-los
- Compor funcionalidades (grep, jq, ffmpeg, curl)
- Usar software existente (npm, pip, pacotes do sistema)
- Processamento em pipeline

### 5.3 Bash vs Tools Tradicionais

Se você estivesse projetando um harness de agente sem bash:
- Criaria uma ferramenta de busca, uma ferramenta de leitura, uma ferramenta de execução
- Cada novo caso de uso exigiria uma nova ferramenta
- Com bash: grep já faz busca, npm já faz instalação, curl já faz HTTP

### 5.4 Exemplo Prático: Agente de E-mail

**Cenário:** O usuário pergunta "Quanto gastei em corridas de Uber/Lyft esta semana?"

**Sem bash:**
- O agente recebe 100 e-mails em json
- Tenta entender manualmente cada um
- Alta carga cognitiva, baixa precisão

**Com bash:**
- Usa API de busca do Gmail para filtrar
- Salva resultados em arquivo
- Usa grep para extrair preços
- Soma valores com script
- Verifica o trabalho conferindo cada preço
- Armazena em arquivo para referência futura

### 5.5 Bash para Agentes não relacionados a código

- Gerar scripts para buscar APIs de clima
- Usar ffmpeg para processar vídeo
- Usar jq para analisar JSON
- Compor pipelines de dados

### 5.6 Custom Scripts no Bash

- Colocar scripts no sistema de arquivos
- Instruir o agente com `--help` para descoberta progressiva
- O agente descobre subcomandos progressivamente

---

## 6. Workflows vs Agentes

### 6.1 Quando usar Workflows

- Fluxos pré-definidos com entradas e saídas bem definidas
- Processos repetitivos (ex: triagem de issues no GitHub)
- Usar com Structured Outputs (lançamento recente)

### 6.2 Quando usar Agentes

- Quando você quer conversar em linguagem natural
- Quando o agente precisa agir com flexibilidade
- Exemplos: análise de dados de negócios, dashboards, responder perguntas

### 6.3 Ambos são possíveis no SDK

- O SDK suporta tanto workflows quanto agentes
- Foco principal da apresentação: agentes

---

## 7. Projetando o Loop de um Agente

### 7.1 As Três Partes do Loop

```
┌─────────────────┐
│  Coletar        │
│  Contexto       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Executar       │
│  Ação           │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Verificar      │
│  Trabalho       │
└─────────────────┘
```

### 7.2 Coletar Contexto

- Para Claude Code: encontrar os arquivos necessários
- Para um agente de e-mail: encontrar os e-mails relevantes
- **Passo frequentemente subestimado ou ignorado**

### 7.3 Executar Ação

- O agente tem as ferramentas certas?
- Geração de código, bash — são mais flexíveis
- O agente decide qual ação tomar

### 7.4 Verificar Trabalho

- **Pergunta chave:** "Você consegue verificar o trabalho do agente?"
- Se sim, é um excelente candidato para agente
- Código → pode compilar, executar e testar → verificável
- Pesquisa profunda → mais difícil → usar citações de fontes

### 7.5 Dica Prática

> "Leia os transcripts repetidamente. Toda vez que o agente rodar, leia o que ele fez. Pergunte-se: 'O que ele está fazendo? Por que está fazendo isso? Como posso ajudá-lo?'"

---

## 8. Ferramentas vs Bash vs Geração de Código

### 8.1 Três Formas de Ação

| Característica | Tools (Ferramentas) | Bash | Code Gen |
|---|---|---|---|
| **Estrutura** | Extremamente estruturada | Compostável | Altamente dinâmica |
| **Confiabilidade** | Alta | Média | Variável |
| **Uso de contexto** | Alto (muitas tools) | Baixo | Baixo |
| **Latência** | Baixa | Média | Alta |
| **Descoberta** | Nenhuma (definida) | Progressiva (`--help`) | Dinâmica |
| **Composição** | Não compostável | Altamente compostável | Altamente compostável |

### 8.2 Quando usar cada um

#### Ferramentas (Tools)
- Ações atômicas que precisam de alto controle
- Ações não-destrutivas ou irreversíveis
- **Exemplo:** escrever arquivo (o usuário vê e aprova)
- **Exemplo:** enviar e-mail

#### Bash
- Ações compostáveis
- **Exemplo:** navegar em diretórios, git, checar erros
- **Exemplo:** sistema de memória via arquivos

#### Geração de Código
- Lógica altamente dinâmica e flexível
- Composição de APIs
- Análise de dados
- Busca avançada

### 8.3 Recomendação Prática

```
Database sensível com dados de usuário?  → Use Tool (proteção + validação)
Database com SQL dinâmico?               → Use Bash ou Code Gen (mais flexível)
Enviar e-mail?                            → Use Tool (ação irreversível)
Analisar planilha?                        → Use Code Gen + Bash
```

---

## 9. Skills (Habilidades)

### 9.1 O que são Skills

Skills são coleções de arquivos que fornecem instruções especializadas para o agente. São uma forma de **divulgação progressiva de contexto** (progressive context disclosure).

### 9.2 Como funcionam

- São simplesmente pastas com arquivos que o agente pode ler
- O agente faz `cd` no diretório da skill e lê as instruções
- Permitem que o agente execute tarefas longas e complexas

### 9.3 Exemplos de Skills

- **Skill de Design Front-end:** Instruções detalhadas de um dos melhores engenheiros de front-end da Anthropic
- **Skills de Geração de Documentos:** Como usar code gen para criar arquivos específicos

### 9.4 Skills vs API

- Skills são úteis quando o agente precisa descobrir como fazer algo
- Se o agente sempre consulta uma API específica, um arquivo `.ts` ou `.py` pode ser melhor
- Skills são uma introdução ao pensamento de "sistema de arquivos como contexto"

### 9.5 Futuro das Skills

- Claude Code tem um marketplace de plugins
- Está evoluindo (ainda V0)
- Mais um sistema de descoberta do que de monetização
- Com o tempo, modelos farão mais tarefas sozinhos; skills preenchem lacunas para tarefas fora da distribuição

---

## 10. Sub-Agentes e Gerenciamento de Contexto

### 10.1 Por que usar Sub-Agentes

- Gerenciar contexto de forma eficiente
- Tarefas que não precisam poluir o contexto do agente principal
- Processamento paralelo

### 10.2 Padrões de Uso

- **Sub-agente de busca:** recebe a pergunta, pesquisa, retorna resultados resumidos
- **Sub-agente de verificação:** analisa adversarialmente o trabalho do agente principal
- **Processamento paralelo:** ler múltiplas planilhas simultaneamente

### 10.3 Como o Claude Code Implementa

- Claude Code tem a melhor experiência para sub-agentes, especialmente com bash
- Sub-agentes paralelos com bash exigem cuidado com race conditions
- O harness gerencia toda a engenharia de sistema (spawn, comunicação, coleta)

### 10.4 Analogia do Quarto Trancado

> "É como se alguém te trancasse em uma sala e te desse tarefas. Você preferiria uma pilha de papéis ou um computador com Google, ferramentas e liberdade? O agente também prefere o computador."

---

## 11. Segurança e Guardrails

### 11.1 Defesa em Camadas (Swiss Cheese Defense)

Cada camada tem algumas defesas; juntas, espera-se que bloqueiem tudo.

### 11.2 Camada do Modelo

- Alinhamento do modelo (safety training)
- Paper recente sobre reward hacking (recomendado)

### 11.3 Camada do Harness

- Permissões e prompting
- **AST Parser no bash:** sabe confiavelmente o que o bash está fazendo
- Não é algo que você queira construir do zero

### 11.4 Camada de Sandbox

- Mesmo se o agente for comprometido, o que ele pode realmente fazer?
- Sandbox de rede
- Sandbox de sistema de arquivos
- Isolamento de operações

### 11.5 A Tríade de Segurança

> "Se alguém assume o controle do seu agente, ainda precisa extrair informação. Se você isolar em sandbox — rede, arquivos, execução — fica muito mais difícil."

### 11.6 Provedores de Sandbox

- Modal, E2B, Fly.io — todos têm níveis de segurança integrados
- Não hospede no seu computador pessoal ou com secrets de produção

---

## 12. Verificação e Hooks

### 12.1 Importância da Verificação

- Quanto mais verificável o trabalho, melhor o candidato a agente
- Comece com o máximo de regras **determinísticas** possível
- Depois, adicione verificação baseada em sub-agentes

### 12.2 Exemplos de Verificação Determinística

- **Null pointers:** o código compila? Tem erros óbvios?
- **Claude Code:** se o agente tenta escrever em um arquivo que não leu, o harness rejeita
- **Regras:** "não busque mais de 10.000 linhas por vez"

### 12.3 Hooks

- Forma de fazer verificação determinística em eventos específicos
- Disparam após cada chamada de ferramenta
- **Exemplo:** a cada chamada, verificar se a planilha foi alterada pelo usuário
- **Exemplo:** forçar o agente a sempre escrever scripts ao invés de responder diretamente

### 12.4 Verificação em Sub-Agentes

- Use sub-agentes em contexto separado para verificação adversarial
- "Aja como um analista júnior da McKinsey revisando este trabalho"
- **Cuidado:** evitar poluição de contexto — use uma sessão nova

---

## 13. Exemplo de Design: Agente de Planilhas

### 13.1 Problema

Construir um agente que possa:
- Buscar dados em planilhas
- Executar ações (inserir, atualizar)
- Verificar o trabalho

### 13.2 Como Buscar (Coletar Contexto)

Várias abordagens criativas foram discutidas:

| Abordagem | Descrição |
|---|---|
| **CSV + grep** | Converter para CSV e usar grep/busca textual |
| **SQLite** | Converter planilha em banco SQL — o agente sabe SQL muito bem |
| **Fórmulas (A1:B5)** | Usar a sintaxe de range que o agente já conhece |
| **XML** | Arquivos .xlsx são XML — o agente entende XML search |
| **APIs Google** | Usar APIs nativas da ferramenta de planilhas |
| **Metadados** | Pré-processar a planilha para adicionar metadados de busca |

> "Pense em transformação. Se puder converter uma fonte de dados em uma interface que o agente conhece bem (como SQL), você ganhou."

### 13.3 Como Executar Ação

- Inserir linhas via SQL ou APIs
- Editar células via fórmulas ou scripts
- Abordagens similares à coleta de contexto

### 13.4 Como Verificar

- Verificar null pointers
- Checkpoints de estado (undo/redo)
- Armazenar estado entre checkpoints
- Permitir que o usuário "volte no tempo"

### 13.5 Desafios com Planilhas Grandes

- Milhões de linhas → acuracidade diminui
- Não carregar tudo no contexto de uma vez
- O agente deve navegar como um humano: ver primeiras linhas, depois buscar
- Usar scratch pad (uma nova aba) para anotações

---

## 14. Live Coding: Agente Pokémon

### 14.1 Objetivo

Construir um agente que pode conversar sobre Pokémon, montar times competitivos e consultar a PokéAPI.

### 14.2 Abordagem 1: Claude Code + Geração de Código

1. Prompt para o Claude Code: "Busque a PokéAPI e crie uma biblioteca TypeScript"
2. Claude Code gera interfaces, tipos e funções automaticamente
3. O agente escreve scripts no diretório `examples/` e os executa
4. Exemplo: "Me mostre todos os Pokémon de água da geração 2"

### 14.3 Abordagem 2: Apenas Ferramentas (Tools)

- Definir ferramentas: getPokemon, getAbility, getType, getMove
- Criar uma interface de chat simples
- O agente decide qual ferramenta chamar
- **Limitação:** ferramentas demais confundem o modelo

### 14.4 Exemplo de Time Competitivo

- Dados de um arquivo texto com sinergias entre Pokémon
- Claude Code escreve script para analisar os dados
- Busca counters, teammates, base stats
- Gera sugestões de time e moveset

### 14.5 Claude Code Jogando Pokémon

- Acesso à memória interna do emulador GBA
- Busca em RAM para encontrar party, mapa, itens
- Pokémon Red é um jogo bem "in-distribution" (muito reverse engineered)

---

## 15. Hospedagem e Sandbox

### 15.1 Opções de Deployment

#### Aplicação Local
- Claude Code funciona no computador do usuário
- Ideal para distribuição inicial
- "Aplicações locais podem se tornar o padrão com IA"

#### Sandbox na Nuvem
- Provedores: E2B, Modal, Fly.io
- Basta `sandbox.start()` e comunicar com ele
- Cada usuário tem seu próprio sandbox

### 15.2 Exemplo com Agent.ts

```typescript
// Exemplo conceitual
import { Sandbox } from 'sandbox-provider'
const sandbox = await Sandbox.start()
// O agente roda dentro do sandbox
// Comunicacao via API
```

### 15.3 UI Adaptativa

- Dentro do sandbox, rodar um dev server
- O agente edita o código e o servidor faz live refresh
- O usuário interage com o site gerado
- Como Lovable e outros site builders funcionam

---

## 16. Monetização de Agentes

### 16.1 Considerações Iniciais

- Agentes são custosos (modelos, computação)
- Projete a monetização **desde o início** — é difícil voltar atrás

### 16.2 Modelos de Precificação

| Modelo | Quando usar |
|---|---|
| **Assinatura** | Uso frequente e previsível |
| **Por token/uso** | Uso esporádico |
| **Misto** | Rate limits + cobrança por excesso |

### 16.3 Como Claude Code Faz

- Rate limits para usuários moderados
- Usage-based pricing para quem excede

### 16.4 Conselho

> "Número um: certifique-se de que está resolvendo um problema pelo qual as pessoas queiram pagar. Depois, escolha o modelo de precificação."

---

## 17. Considerações sobre Contexto

### 17.1 Quando Compactar

- No Claude Code, o palestrante raramente precisa de compressão
- Estratégia: limpar o contexto frequentemente
- O estado está nos arquivos — `git diff` mostra as mudanças

### 17.2 Para Agentes com Usuários Não Técnicos

- Usuários não sabem o que é "context window"
- A cada nova pergunta, faça compressão automática
- Armazene preferências do usuário
- O estado da aplicação (ex: planilha) já contém muito contexto

### 17.3 Dica Prática

> "Não precisa de 1 milhão de tokens de contexto. Precisa de **bom gerenciamento de contexto**."

---

## 18. Melhores Práticas Gerais

### 18.1 Prototipagem Rápida

1. Comece com Claude Code — dê a ele as APIs e instruções
2. Observe o que Claude Code faz
3. Itere em cima do comportamento
4. Quando estiver bom, transforme em agente com o SDK

### 18.2 Simplicidade vs Facilidade

> "Construir um agente deve ser **simples**, mas simples não é o mesmo que **fácil**."

- O código do agente não precisa ser enorme
- Precisa ser elegante
- Precisa ser o que o **modelo quer** — trabalhe com o modelo, não contra ele

### 18.3 Reescrita Constante

- Reveja e reescreva o código do agente a cada ~6 meses
- As capacidades mudam rápido
- "Você escreve código 10x mais rápido — jogue fora código 10x mais rápido também"
- Startups têm vantagem: ciclos de incubação curtos vs empresas estabelecidas

### 18.4 Trabalhando com o Modelo

- Leia os transcripts
- Descubra o que o modelo prefere
- Adapte a interface para estar "in-distribution" (dentro da distribuição de treinamento do modelo)
- Transforme problemas fora da distribuição em algo que o modelo conhece bem

### 18.5 Reutilização entre Agentes

- Paradigma diferente de web apps (1 app → 1M usuários)
- Agentes geralmente são 1:1 (um container por usuário)
- Comunicação entre agentes: HTTP requests, não reinventar
- Exemplo criativo: fórum virtual onde agentes postam tópicos e respostas

### 18.6 Busca Semântica vs grep

- O modelo é melhor com grep do que com busca semântica
- O modelo **não foi treinado** para busca semântica
- Para bases de código grandes: bom gerenciamento de contexto + hooks + linting

---

## Glossário

| Termo | Significado |
|---|---|
| **Agent Harness** | Infraestrutura que envolve o modelo (ferramentas, loop, prompts) |
| **Divulgação Progressiva de Contexto** | Técnica de revelar informações conforme necessário (bash `--help`, skills, sistema de arquivos) |
| **In-Distribution** | Tarefas que o modelo conhece bem do treinamento |
| **Code Gen** | O agente escreve e executa código dinamicamente |
| **Hooks** | Funções determinísticas que disparam em eventos do loop do agente |
| **Swiss Cheese Defense** | Múltiplas camadas de segurança que juntas protegem o sistema |
| **AST Parser** | Parser de árvore sintática abstrata — usado para entender o que o bash está fazendo |
| **Context Pollution** | Quando informações desnecessárias sobrecarregam o contexto do agente |

---

> **Nota:** Esta documentação foi gerada a partir da transcrição de um workshop de ~2 horas sobre o Claude Agent SDK, apresentado por um engenheiro da Anthropic. A transcrição foi feita com Whisper (modelo tiny) e traduzida para português brasileiro. Pequenas imprecisões podem existir devido à qualidade da transcrição automática.
