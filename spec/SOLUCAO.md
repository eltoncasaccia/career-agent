# Sistema de Carreira — Solução

> **Solution space.** O problema está em [`../PROBLEMA.md`](../PROBLEMA.md) e
> não deve ser rediscutido aqui. Este documento descreve **como** resolvê-lo.
> Critérios de aceite em [`CRITERIOS-DE-ACEITE.md`](CRITERIOS-DE-ACEITE.md).
>
> Status: **v11 — IDEIA FECHADA.** Pronto para a fase técnica (arquitetura,
> linguagem, frameworks), que é escopo de outro documento.
> Última atualização: 2026-10-01

---

## 1. O objetivo central

**Colocar o usuário empregado.**

Currículo, LinkedIn, portfólio e repositório **não são o produto**. São meios.
O produto é o sistema que leva uma pessoa de "não sei nem por onde começar" até
"contratado", arrumando tudo o que precisa ser arrumado no caminho.

O sistema **age**, não só gera:

- repositório bagunçado → **edita** e deixa do jeito que recrutador gosta;
- não existe repositório → orienta a criar e pede o link;
- LinkedIn fraco → mostra na sua própria tela o que corrigir e entrega o texto;
- competência que o mercado pede e o usuário não tem → **plano de desenvolvimento**;
- competência que ele tem mas não mostra → arruma a apresentação;
- vaga compatível → avisa e prepara a candidatura;
- entrevista marcada → treina.

**Elton é o usuário #1, não o único.** O sistema é genérico desde o schema.

## 2. O motor: rubricas

O conceito central. Para perguntar as coisas certas **antes** de gerar
qualquer coisa, o sistema precisa saber **o que um bom artefato tem**. Isso é
uma rubrica — e ela serve para três coisas de uma vez.

```
                    ┌──────────────┐
                    │   RUBRICA    │   "o que um bom X tem"
                    └──────┬───────┘
           ┌───────────────┼───────────────┐
           ▼               ▼               ▼
      1. AVALIAR      2. PERGUNTAR    3. REVALIDAR
      o que existe    o que falta     o resultado
                      (antes de       (usuário manda o link,
                       gerar)          o sistema nota de novo)
```

Uma rubrica por superfície: **GitHub, LinkedIn, CV, portfólio**.

É o "sistema especialista" do pedido original: o conhecimento de domínio sobre
o que recrutador espera ver, escrito uma vez e usado nas três pontas.

**Corolário (pedido do usuário):** as perguntas vêm **antes** da geração.
O sistema não gera para depois descobrir que faltou — ele roda a rubrica,
descobre o buraco e pergunta primeiro.

## 3. As três lacunas

O sistema existe para detectar e fechar lacunas. São três, com remédios
diferentes:

| Lacuna | O que é | Detectada comparando | Remédio |
|---|---|---|---|
| **de dados** | O sistema não **sabe** algo | perfil × rubrica | pergunta ao usuário |
| **de mercado** | O usuário não **tem** a competência | perfil × vagas-alvo | plano de desenvolvimento |
| **de apresentação** | Ele tem, mas está **mal mostrado** | material × rubrica | o sistema arruma |

**A distinção que mais importa.** "Docker está no seu currículo" significa duas
coisas opostas:

```
   domina Docker  +  README não mostra   →  APRESENTAÇÃO  →  resolve numa tarde
   não domina Docker                     →  MERCADO       →  resolve em 3 meses
```

**É a escala de domínio (§4) que distingue as duas.** Sem ela, o sistema daria
o conselho errado metade das vezes.

## 4. Escala de domínio

Cada competência do perfil carrega um nível **verificado pela entrevista**, com
a evidência que o sustenta anexada:

| Nível | Significa |
|---|---|
| **sem conhecimento** | Aparece no material, mas não se sustenta |
| **pouco conhecimento** | Contato real, mas raso ou não profissional |
| **domina** | Usou de verdade, sabe explicar e defender |

> Exemplo: *"Docker — pouco conhecimento (1 projeto pessoal, nunca em produção)"*

**Alimenta quatro coisas:**

1. o plano de desenvolvimento (o que estudar)
2. o que destacar — e o que **não** destacar — no CV e no LinkedIn
3. o simulador (onde a entrevista vai pegar)
4. a **auditoria** (RN-15)

## 5. O ciclo

Não é pipeline. É ciclo — não converteu, volta ao diagnóstico com o aprendizado.

```
   1. INVENTÁRIO    o que você tem? qual seu alvo?
   2. INGESTÃO      lê as fontes
   3. PERFIL        fonte única da verdade
          ↓
   4. DIAGNÓSTICO   ◄──────────────────────────────┐
      rubricas + vagas → as três lacunas (§3)      │
          ↓                                        │
   5. ENTREVISTA    fecha lacuna de dados,         │
                    classifica domínio (§4)        │
          ↓                                        │
      ┌────────────┴────────────┐                  │
      ▼                         ▼                  │
   6. PLANO                  7. EXECUÇÃO           │
      lacuna de mercado         lacuna de          │
      + acompanhamento          apresentação       │
      da evolução               (repo, LinkedIn,   │
      │                          CV, spec do       │
      │                          portfólio)        │
      └────────────┬────────────┘                  │
                   ▼                               │
   8. RADAR      vagas com match  ◄── roda em paralelo ao plano,
                                      não depois dele            │
   9. CANDIDATURA  tailoring, carta, rastreio                    │
  10. PREPARAÇÃO   simulação de entrevista                       │
          ↓                                                      │
      contratado? ── não ────────────────────────────────────────┘
          │
         sim → perfil segue vivo (RN-8)
```

## 6. Regras de negócio

### RN-1 — As perguntas são dirigidas por lacunas e vêm antes

O agente não aplica questionário fixo. Ele roda as rubricas (§2), descobre o que
falta e pergunta **antes de gerar**. Quem chega com GitHub rico e LinkedIn
completo responde ~5 perguntas; quem chega do zero responde ~50.

### RN-2 — O portfólio responde "o quê"; o GitHub responde "como"

O portfólio é **vitrine**, não documentação técnica. Recrutador e cliente querem
a mesma coisa dela; quem quer profundidade vai ao repositório. **Um site, não
dois.**

### RN-3 — O alvo é declarado pelo usuário, não inferido

O usuário diz o que quer ("vaga de dev júnior"). A lacuna de mercado é sempre
medida **contra o alvo declarado** — não contra um ideal abstrato. Sem alvo, não
há como saber se o perfil está fraco ou forte: fraco *para quê*?

### RN-4 — Nada entra no perfil sem procedência

Cada evidência marcada como **confirmada**, **estimada** ou **a verificar**. O
sistema não inventa métrica. Número inventado destrói o candidato na entrevista.

### RN-5 — Grade curricular gera perguntas, não competências

Ter visto na grade **não** é competência. A grade gera a pergunta certa: *"a
grade diz que você viu Kubernetes — usou em projeto real ou parou na
disciplina?"*. A resposta vira nível na escala do §4.

### RN-6 — Acesso às fontes exige permissão explícita

O sistema **lê e escreve** no GitHub; no LinkedIn, **só lê** — pelo que o
usuário exporta ou envia (RN-22) — e nunca escreve (RN-10). Precisa de acesso
autorizado pelo usuário. Escrita confirmada por ação, nunca autorização em
bloco — o usuário sempre sabe o que vai ser alterado antes de acontecer.

### RN-7 — Uma URL, sempre completa

O portfólio é um site só, todos os projetos, todos os destinatários. Link curado
**envelhece**; URL única sobre perfil vivo se atualiza para quem já a recebeu.

### RN-8 — O sistema é vivo; o perfil é permanente

Projeto novo → sessão curta → perfil atualizado → tudo re-renderizado. Habilita
radar, candidatura, simulador e o acompanhamento de evolução (RN-18).

### RN-9 — O perfil pertence ao usuário

Exportável e legível fora do sistema (YAML, não banco opaco).

### RN-10 — O modo de ação varia por superfície

**Substitui a versão errada da v5.** A fronteira não é "propor vs. aplicar" — é
**conteúdo novo vs. apresentação do existente**.

| Superfície | O sistema faz | Por quê |
|---|---|---|
| **GitHub** (repos existentes) | **Edita direto**: README, descrição, topics, pinned, perfil | É apresentação, não código de projeto. API oficial suporta |
| **Projetos novos** | Não cria — sugere e o usuário constrói | RN-11 |
| **LinkedIn** | **Lê por screenshot e anota**; entrega o texto para o usuário colar | Escrita automatizada arrisca a conta (risco 9.3) |
| **CV** | Gera o arquivo | Documento do usuário, sem plataforma no meio |
| **Portfólio** | Gera a **especificação**; o usuário constrói | RN-17 |
| **Todas** | Reavalia quando recebe o link | Rubrica, uso 3 (§2) |

### RN-11 — O sistema não constrói projetos

**A regra que protege o usuário na entrevista.** Nunca escreve o código dos
projetos do portfólio.

| Pode | Não pode |
|---|---|
| Apresentar, documentar e organizar trabalho real | Escrever o código de um projeto do portfólio |
| Sugerir qual projeto vale construir | Construir e apresentar como sendo do usuário |
| Redigir o texto que descreve o projeto | Inventar métrica ou responsabilidade |

Tudo no portfólio foi construído pela pessoa — por isso ela defende em qualquer
entrevista técnica.

### RN-12 — O simulador treina narrativa

Como tudo é construído pelo usuário (RN-11), o simulador não é portão
anti-fraude. Vale por outro motivo: **saber fazer não é saber explicar**. Se a
pessoa trava defendendo projeto próprio, o problema é de narrativa — volta para
a entrevista de extração refinar a história.

### RN-13 — O radar gera plano, não lista

Buscar vagas não é feature de listagem: é o que **produz a lacuna de mercado**
(§3) contra o alvo declarado (RN-3). Agregador sem diagnóstico não serve.

### RN-14 — O nível é norte, não veredito

O sistema é **conselheiro, não auditor**. A escala do §4 existe para orientar o
usuário, não para julgá-lo nem para bloquear nada.

- O sistema **não remove** competência do currículo por conta própria
- Ele **informa e recomenda**; a decisão do que fica é sempre do usuário
- Se a pessoa mantém no CV algo que não domina e falha na entrevista real, a
  responsabilidade é dela — o sistema avisou

**Tom de referência**, definido pelo usuário:

> *"Você disse que sabe Docker, mas na entrevista errou tudo. Recomendo um plano
> de estudo."*

Isso rebaixa o risco 9.2: nível errado custa uma recomendação ruim, não um
currículo mutilado.

### RN-15 — Auditoria do material existente

O sistema varre CV e LinkedIn atuais e aponta **o que está listado mas não
comprovado**. É o primeiro valor entregue a quem já tem material — antes de
gerar qualquer coisa nova, mostra o que hoje não se sustenta.

### RN-16 — Toda superfície tem rubrica

GitHub, LinkedIn, CV e portfólio têm rubrica explícita e versionada (§2). É o
conhecimento de domínio do sistema — o que o torna especialista em vez de
gerador de texto.

### RN-17 — Portfólio: o sistema especifica, o usuário constrói

O sistema **não monta o site**. Ele gera o briefing a partir do perfil,
produtos e repositórios: estrutura, conteúdo por seção, sugestões de nome,
paleta, tipografia — um mini design system. O usuário constrói e manda o link;
o sistema audita pela rubrica.

> **Efeito colateral valioso:** o site vira **um projeto do próprio portfólio** —
> construído pelo usuário, defensável em entrevista, e prova de entrega.

### RN-18 — Plano de desenvolvimento com acompanhamento

Se o perfil está fraco **para o alvo declarado** (RN-3), o sistema monta plano
de desenvolvimento e **acompanha a evolução** ao longo do tempo — o usuário vê
o progresso, e cada competência que sobe de nível (§4) re-renderiza o material.

**O radar roda em paralelo, não depois.** Não se espera o plano terminar para
procurar vaga: o match melhora conforme a evolução acontece.

### RN-19 — Feedback visual, ancorado na tela real

Onde o sistema não pode (ou não deve) editar, ele **mostra**. Recebe o print
que o usuário tira da própria tela (RN-22) e **anota a imagem**: caixa no
headline vazio, número na seção fraca, seta onde falta conteúdo — com o texto
pronto ao lado de cada marcação.

```
   LinkedIn    ─┐
   Portfólio   ─┼─►  print da tela real ──► rubrica anota ──► o que corrigir,
   GitHub      ─┘                                             onde, e o texto
```

Resolve o que a lista de sugestões não resolve: **"onde eu colo isso?"**.
É o modo de feedback padrão do sistema, e transforma a auditoria (RN-15) e a
revalidação pelo link (RN-17, §2 uso 3) de textuais em visuais.

### RN-20 — Relatório antes/depois

O sistema **congela o estado inicial** de cada superfície antes de qualquer
mudança e, depois, gera um comparativo.

```
   ANTES              AÇÃO                DEPOIS            RELATÓRIO
   (baseline    ──►  o usuário     ──►  (novo         ──►  comparativo
    congelado)        aplica              retrato)          medido
```

**O baseline é perecível.** No instante em que a mudança acontece, o "antes"
desaparece para sempre — então capturá-lo é **pré-condição de qualquer ação**,
não etapa opcional.

Serve a três propósitos:

1. **Prova de valor** — o sistema demonstra que funcionou, com números
2. **Motivação** — o usuário vê a evolução, o que sustenta a RN-18
3. **Peça de portfólio** — *"meu GitHub estava assim; rodei meu sistema; ficou
   assim"* é a demonstração mais convincente que o produto pode ter

### RN-21 — O sistema nunca pede credenciais

**Regra dura, não limitação técnica.** O sistema não pede, não armazena e não
manipula senha de nenhuma plataforma.

O login é sempre feito **pelo próprio usuário**, na própria plataforma, como
ele sempre faz. Para ler, o sistema usa o que a pessoa exporta ou envia
(RN-22) — nunca entra na conta no lugar dela.

> Um sistema de carreira que pede a senha do seu LinkedIn é um sistema que você
> não deveria usar. Vale para o produto e vale como sinal de confiança.

### RN-22 — O usuário exporta, o sistema ingere

**Extensão de navegador nunca é requisito.** Exigir instalação mata a adoção de
um produto de consumo — e é desnecessário, porque o LinkedIn já oferece ao
próprio usuário duas saídas oficiais:

| Caminho | Como | O que rende |
|---|---|---|
| **Salvar em PDF** | Perfil → "Mais" → "Salvar como PDF" | O perfil inteiro, formatado |
| **Baixar seus dados** | Config. → Privacidade de dados → "Obter uma cópia dos seus dados" | CSVs estruturados: experiências, formação, competências, **conexões** |

Melhor que raspagem em todos os aspectos:

- **Zero atrito de termos de uso** — é funcionalidade oficial, feita para isso
- **Zero credenciais** — a RN-21 fica intacta
- **Dados estruturados** em vez de HTML raspado
- **Funciona em qualquer navegador e dispositivo**, sem instalar nada
- O arquivo traz **as conexões** — insumo direto do módulo `rede` (#15)

A RN-19 sobrevive inteira: **o usuário tira o print e envia**; o sistema anota e
devolve. Extensão só automatiza o print — conveniência, nunca requisito.

> Extensão do princípio da RN-10, agora na entrada:
> **o sistema propõe, o usuário aplica → o usuário exporta, o sistema ingere.**

**Descartado — abrir navegador próprio e pedir login:** exigiria a senha do
usuário (viola RN-21) e login por IP de datacenter é o padrão que o LinkedIn
mais persegue, com risco de restrição da conta.

### RN-23 — Registro pós-entrevista, com gravação opcional

Depois de cada entrevista **real**, o usuário registra como foi. Isso fecha o
ciclo do §5 com dado de campo, não com achismo.

**Dois modos:**

| Modo | Como funciona | Quando |
|---|---|---|
| **Anotação** (padrão) | O sistema pergunta e o usuário responde de memória: o que perguntaram, onde travou, o que não soube responder | Sempre disponível, risco zero |
| **Gravação** (opt-in) | Grava e transcreve; gera insights, feedback e plano a partir da transcrição | Só com consentimento — ver risco 9.5 |

**O usuário informa se passou.** Esse é o sinal de qualidade do sistema: as
perguntas que derrubaram viram lacunas, que viram plano (RN-18), que volta ao
diagnóstico (§5).

> Saber se a pessoa **conseguiu o emprego** é desejável, mas não é impeditivo de
> nada. O resultado **por entrevista** é mais granular e mais acionável.

### RN-24 — Privacidade: nem tudo que entra pode sair

O perfil concentra material sensível — pretensão salarial, motivo real de saída
de um emprego, onde a pessoa é fraca, o que ela não soube responder. Multiusuário.

**Todo campo do perfil é classificado como `privado` ou `publicável`.**

| `privado` — nunca entra em artefato | `publicável` — pode ser renderizado |
|---|---|
| Pretensão e histórico salarial | Experiências, formação, projetos |
| Motivo real de saída de emprego | Competências e nível declarado |
| Nível de domínio avaliado como fraco | Métricas e resultados confirmados |
| Falhas em entrevistas e transcrições | Links públicos |

Campo `privado` alimenta diagnóstico, plano e preparação de entrevista — **e
nunca é renderizado em CV, LinkedIn ou portfólio**, por acidente ou a pedido.

**Regras adicionais:**

- Os dados do usuário **não** treinam modelo nenhum
- Nada é compartilhado com terceiros, recrutador ou empresa sem ação explícita
  dele
- O usuário pode **apagar tudo**, e apagar significa apagar
- Complementa a RN-9 (o perfil é dele e é exportável)

### RN-25 — Atualização: o usuário avisa **ou** a varredura detecta

Resolve o gatilho pendente da RN-8. Dois caminhos, não excludentes:

1. **O usuário avisa** — terminou algo, conta ao sistema
2. **A varredura detecta** — na próxima passada pelas fontes, o sistema percebe
   o repositório novo, os commits, o curso concluído, e pergunta

Sem cutucão por inatividade. O sistema descobre em vez de cobrar.

## 7. Módulos

| # | Módulo | O que faz | Fatia |
|---|--------|-----------|-------|
| 1 | `inventario` | Descobre fontes disponíveis e o **alvo declarado** | MVP |
| 2 | `ingest` | GitHub (API pública) + exportações oficiais do LinkedIn (PDF/CSV) + grade, certificados | MVP |
| 3 | `perfil` | Schema canônico + versionamento + escala de domínio | MVP |
| 4 | `rubricas` | Base de conhecimento: o que um bom X tem | MVP |
| 5 | `diagnostico` | Roda rubricas → as três lacunas (§3) + auditoria (RN-15) | MVP |
| 6 | `entrevista` | Fecha lacuna de dados e **classifica domínio** (§4) | MVP |
| 7 | `cv` | Renderers nacional e internacional | MVP |
| 8 | `linkedin` | Ingere export oficial; anota o print enviado pelo usuário; texto pronto para colar | MVP |
| 9 | `repo-doctor` | **Edita** o GitHub: README, descrição, topics, pinned | MVP |
| 10 | `portfolio-spec` | Gera o briefing/design system e audita o site pelo link | MVP |
| 11 | `plano` | Plano de desenvolvimento + acompanhamento da evolução | **MVP** |
| 12 | `radar` | Busca e filtra vagas por match com o alvo | **MVP** |
| 13 | `candidatura` | Tailoring por JD, carta, rastreio do pipeline | Fase 2 |
| 14 | `simulador` | Entrevista simulada; treina narrativa (RN-12) | Fase 2 |
| 15 | `rede` | Indicações: cruza vagas-alvo com a rede do usuário | Fase 3 — aprovado |
| 16 | `antes-depois` | Congela o baseline e gera o comparativo medido (RN-20) | **MVP** |
| 17 | `pos-entrevista` | Registro do que aconteceu na entrevista real; gravação opcional (RN-23) | Fase 2 |

**Por que `plano` e `radar` estão no MVP (decidido na v8):** o usuário #1 tem
perfil em formação. Sem eles, o MVP entrega material bonito para um perfil que
ainda não compete pelas vagas que quer. E, do lado do negócio, são eles que
transformam uma transação de 2 meses numa relação de 12 (§9).

**Sobre o módulo 15 (`rede`):** indicação é o canal que mais converte em
contratação e o único que ninguém sistematiza. O sistema saberá onde o usuário
estudou, trabalhou e quais vagas dão match — cruzar com a rede do LinkedIn
permite dizer *"você tem 3 ex-colegas nessa empresa; este é o mais próximo"*.

## 8. Fatiamento

**MVP = a menor fatia que leva o usuário #1 a uma candidatura real com material
que se sustenta.** Não construir os 17 antes de entregar algo.

## 9. Riscos

### 9.1 O entrevistador é gargalo de qualidade (ALTO)

Perfil genérico → CV genérico. Tudo depende da extração — e agora também a
escala de domínio (§4) depende de a entrevista sondar fundo o suficiente.

**Mitigação:** técnicas do §10, sessões curtas, RN-4, RN-5, RN-14.

### 9.2 Classificar domínio é julgamento, não medição (BAIXO — rebaixado na v7)

"Domina" atribuído por conversa erra nos dois sentidos: subestima quem se
comunica mal, superestima quem fala bem.

**Rebaixado pela RN-14.** Como o nível é conselho e não veredito — não bloqueia
nem remove nada —, um erro de classificação custa uma recomendação ruim, não um
currículo mutilado. O dano deixou de ser estrutural.

### 9.3 LinkedIn (BAIXO — resolvido na v7)

O GitHub tem API oficial: editar README, descrição e topics é legítimo e
suportado. **O LinkedIn não** — não expõe edição de perfil a terceiros, e
automatizar escrita via navegador viola os termos de uso, com detecção ativa e
histórico de restrição de contas. O risco recairia justo sobre quem está
procurando emprego.

**Resolvido pela RN-19:** o sistema **lê e anota**, nunca escreve. Ler o próprio
perfil na própria sessão logada, uma página, sem volume, ainda roça a cláusula
anti-scraping — risco baixo, não zero. Escrita automatizada fica fora.

**Confirmado empiricamente em 2026-09-05:** requisição sem sessão ao perfil
`linkedin.com/in/elton-casaccia-399216311` retornou **HTTP 999 + authwall** — o
código anti-bot do LinkedIn. Sem sessão logada não se lê **nada**.

> **Resolvido pela RN-22 (v10):** o módulo `linkedin` **não** depende de acesso
> ao navegador. Ingere as exportações oficiais do próprio LinkedIn (PDF e
> arquivo de dados) e anota os prints que o usuário envia. Leitura automatizada
> vira conveniência opcional, nunca requisito.

### 9.4 Escopo (MÉDIO)

17 módulos, dois runtimes, quatro superfícies, base de rubricas.

**Mitigação:** §8.

### 9.5 Gravação de entrevista tem implicação legal (MÉDIO — novo na v11)

Gravar uma conversa envolve **outra pessoa**: o entrevistador. As regras variam
por jurisdição — e o sistema mira também vagas internacionais (RN-3). Em várias
delas, gravar sem consentimento de todos os participantes é ilegal, não apenas
deselegante. Plataformas de vídeo e políticas internas da empresa também podem
proibir.

**Mitigação (RN-23):** modo **anotação** é o padrão e não grava nada. O modo
**gravação** é opt-in, exibe aviso sobre consentimento e orienta o usuário a
pedir autorização ao entrevistador antes. A responsabilidade pela gravação é de
quem grava — o sistema informa, não decide por ele.

### ~~Fabricação de credenciais~~ — ELIMINADO (RN-11: o sistema não constrói projetos)

## 10. Técnicas do entrevistador

Sessões temáticas curtas, não uma entrevista-monstro:

1. **Trajetória** — linha do tempo, viradas, porquês
2. **Por sistema/projeto** — um de cada vez, em profundidade
3. **Competências** — sondagem para atribuir nível (§4), não checklist
4. **Resultados e números** — a que mais rende e a mais difícil
5. **Direção** — o alvo declarado (RN-3)

**Obrigatório:**

- **STAR** por história (situação, tarefa, ação, resultado)
- **Pressão por quantificação**: "quantos?", "de quanto pra quanto?", "em quanto
  tempo?", "comparado a quê?"
- **A pergunta que mais rende**: *"o que você tocou com as próprias mãos?"*
- **Sondagem de profundidade** para separar "usou uma vez" de "domina"
- **Marcação de procedência** (RN-4) e **de nível** (RN-14)

## 11. Decisões tomadas

| Decisão | Escolha |
|---|---|
| **Objetivo** | Conseguir emprego; artefatos são meio, não fim |
| **Natureza** | Produto multiusuário, Elton é usuário #1 |
| **Runtime** | Claude Code primeiro (skills), depois portar p/ ADK/LangGraph — direção; a fase técnica confirma ou revisa |
| **Motor** | Rubrica por superfície: avalia, pergunta, revalida (RN-16) |
| **Lacunas** | Três: dados, mercado, apresentação (§3) |
| **Competências** | Escala de domínio como **norte**, não veredito (§4, RN-14) |
| **GitHub** | O sistema **edita** direto |
| **LinkedIn** | Screenshot anotado + texto para colar; nunca escrita automatizada (RN-19) |
| **Portfólio** | O sistema especifica, o usuário constrói (RN-17) |
| **Projetos** | O sistema nunca constrói (RN-11) |
| **Alvo** | Declarado pelo usuário (RN-3) |
| **Ciclo de vida** | Sistema vivo, perfil permanente e exportável (RN-8, RN-9) |
| **Rede** | Módulo de indicações aprovado (Fase 3) |
| **Prova de valor** | Relatório antes/depois; baseline congelado antes de agir (RN-20) |
| **Credenciais** | Nunca pedidas (RN-21) |
| **Privacidade** | Campos `privado` vs `publicável`; sem treino, sem compartilhamento (RN-24) |
| **Pós-entrevista** | Registro sempre; gravação opt-in com aviso legal (RN-23) |
| **Gatilho** | Usuário avisa **ou** varredura detecta; sem cobrança (RN-25) |
| **Ingestão do LinkedIn** | Exportação oficial do usuário; extensão nunca é requisito (RN-22) |

## 12. Análise de mercado

> Levantamento feito a pedido do usuário: *"já existe algo assim? dá para
> monetizar? tem público ou é perda de tempo?"*. Baseado em conhecimento do
> setor — números e preços a confirmar antes de qualquer decisão de negócio.

### 12.1 Cada módulo isolado já é um produto

| Categoria | Quem já faz | Situação |
|---|---|---|
| Rastreio de vagas + CV | **Teal**, **Huntr** | Estabelecidos. O mais próximo da ideia de "sistema" |
| Otimização ATS | **Jobscan** | Estabelecido, pago, nicho definido |
| Construtor de CV com IA | **Enhancv**, **Kickresume**, **Rezi**, **Resume.io** | Mercado maduro e **saturado** |
| LinkedIn + carreira | **Careerflow.ai** | Sobrepõe ao módulo 8 |
| Simulação de entrevista | **Final Round AI**, Google Interview Warmup | Categoria cheia |
| Auto-candidatura | Simplify, LazyApply | Existe; boa parte é de baixa qualidade |

**Não existe montado:** os pedaços integrados em torno de um **perfil canônico
persistente**, com nível de domínio verificado e plano acompanhado. Teal chega
mais perto e ainda é agregador de ferramentas, não sistema com fonte única.

**No Brasil o cenário é bem mais vazio.**

### 12.2 As três verdades desconfortáveis

1. **Churn estrutural.** O produto existe para a pessoa parar de precisar dele.
   Consegue emprego → cancela. LTV de 2 a 4 meses.
2. **Quem mais precisa é quem menos paga.** Desempregado e júnior é o público
   com menor disposição a pagar — no Brasil, ainda mais.
3. **"Construtor de CV com IA" está comoditizado pelo ChatGPT.** Se o
   diferencial for gerar texto bonito, ele já evaporou.

### 12.3 O que sobra defensável

```
   comoditizado  ──►  gerar CV, gerar texto de LinkedIn (ChatGPT faz)

   defensável    ──►  perfil canônico persistente  (50 respostas investidas;
                                                    trocar dói)
                      plano + acompanhamento       (relação de 12 meses,
                                                    não transação de 2)
                      módulo de indicações         (ninguém faz; canal que
                                                    mais converte)
```

**`plano` e `radar` não são só features — consertam o modelo de negócio.**
Construtor de currículo tem LTV de 1 mês; acompanhante de carreira tem LTV de
1 ano, porque a pessoa fica enquanto **evolui**, não enquanto procura.

### 12.4 Caminho de receita mais provável: B2B

Universidades, bootcamps, MBAs e empresas de recolocação compram ferramenta de
carreira para alunos e desligados. Ticket alto, churn baixo, decisão racional.
É onde ferramentas de carreira efetivamente ganham dinheiro — bem melhor que
B2C.

### 12.5 Veredito

**Não é perda de tempo, porque o piso está garantido.** Mesmo sem receita, o
projeto entrega: (a) o currículo, LinkedIn e portfólio de que o usuário #1
precisa; (b) uma peça de portfólio séria — sistema multiagente portado entre
runtimes não é projeto de tutorial; (c) prática de ADK e LangGraph aplicada a
algo real.

**Perda de tempo seria** construir os 17 módulos mirando um SaaS antes de
validar. Usar primeiro, virar a primeira evidência, e então decidir com dado.

## 13. Ecossistema pesquisado (via skill `find-skills`)

| Pacote | Conteúdo | Instalações | Veredito |
|---|---|---|---|
| `paramchoudhary/resumeskills` | ~15 skills atômicas (ats-optimizer, bullet-writer, tailor, quantifier, linkedin-optimizer, portfolio-case-study, cover-letter, jd-analyzer, interview-prep, salary-negotiation) | 7–10K cada | **Referência.** Peças soltas, sem fonte única, sem rubrica, sem ciclo |
| `addyosmani/agent-skills@interview-me` | Extração de conhecimento tácito | 25.9K | **Estudar o padrão** — é o módulo 6 |
| `anthropics/knowledge-work-plugins@interview-prep` | Prep de entrevista (oficial) | 3.3K | Candidato ao módulo 14 |
| `career-ops-hq/career-ops` | Tentativa de sistema completo | 635 | Ver o que erraram |

**Conclusão:** ninguém resolveu a fonte única da verdade, o motor de rubricas, a
escala de domínio verificada, nem o ciclo que **age** sobre as fontes.

## 14. Em aberto

- [ ] Ferramenta de anotação de imagem (RN-19) — fase técnica
- [ ] De onde vêm as vagas do radar — fase técnica
- [ ] Stack sugerida no briefing do portfólio (RN-17) — fase técnica
- [ ] Formato e granularidade do schema do perfil — fase técnica
- [ ] Multiusuário: onde mora o perfil de cada pessoa — fase técnica

## 15. Insumos do Elton (usuário #1)

- [x] **GitHub** → auditoria em `dados/` (baseline congelado; não versionado)
- [x] **LinkedIn** → auditoria em `dados/` (baseline congelado, export oficial)
- [x] **MBA** → grade completa em `dados/` (11 disciplinas)
- [x] **Archi** → projeto pessoal, sem cliente; repos privados, autorizados a abrir
- [ ] ⛔ **Experiência profissional** — nada conhecido. **Maior buraco do perfil**
- [ ] ⛔ **TopHawks** — citado na 1ª mensagem, ainda sem informação
- [ ] Formação anterior (graduação?)
- [ ] Sistemas/produtos que quer vender
- [ ] **Alvo declarado** (RN-3): que vaga você quer?
- [ ] 2–3 vagas-alvo de exemplo

> **A ingestão chegou ao limite.** Tudo o que era legível por fonte pública ou
> export oficial já foi extraído. O que falta só existe na cabeça do usuário —
> é exatamente o ponto em que a RN-1 manda o módulo `entrevista` assumir.
