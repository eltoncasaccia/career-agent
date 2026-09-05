# Critérios de Aceite

> Passo 04 do método: **critérios antes da implementação.**
> Escopo: os 13 módulos do MVP. Fase 2 e 3 ganham critérios quando entrarem.
>
> Cada critério é **verificável**: dá para dizer sim ou não, sem opinião.
> Problema em [`../PROBLEMA.md`](../PROBLEMA.md) · Solução em [`SOLUCAO.md`](SOLUCAO.md)

---

## Critérios globais

Valem para o sistema inteiro. Violação de qualquer um é bug, não preferência.

| # | Critério | Regra |
|---|---|---|
| G1 | O sistema nunca solicita senha de plataforma alguma | RN-21 |
| G2 | Nenhuma alteração em conta do usuário ocorre sem confirmação explícita daquela ação | RN-6 |
| G3 | Nenhum campo marcado `privado` aparece em artefato gerado, em nenhuma circunstância | RN-24 |
| G4 | Nenhuma métrica, competência ou responsabilidade é gerada sem origem rastreável no perfil | RN-4 |
| G5 | O sistema nunca escreve código de projeto de portfólio | RN-11 |
| G6 | Nenhuma funcionalidade exige instalação de extensão de navegador | RN-22 |
| G7 | O baseline de uma superfície é capturado antes da primeira alteração nela | RN-20 |
| G8 | O perfil é exportável em formato legível por humano a qualquer momento | RN-9 |

---

## 1. `inventario`

- [ ] Pergunta quais fontes o usuário tem antes de qualquer outra coisa
- [ ] Aceita "não tenho" em qualquer fonte sem bloquear o fluxo
- [ ] Captura o **alvo declarado** (RN-3); sem alvo, o diagnóstico de mercado não roda e o sistema diz isso
- [ ] Ao final, existe uma lista explícita do que será ingerido e do que virará lacuna

## 2. `ingest`

- [ ] Lê GitHub por API pública, sem credencial
- [ ] Aceita export oficial do LinkedIn (PDF e/ou CSV) enviado pelo usuário
- [ ] Aceita grade curricular e certificados
- [ ] Fonte ilegível ou ausente **não** interrompe o fluxo: vira lacuna de dados
- [ ] Toda informação extraída registra de qual fonte veio

## 3. `perfil`

- [ ] Formato legível por humano e versionado
- [ ] Toda evidência carrega procedência: `confirmada` / `estimada` / `a verificar` (RN-4)
- [ ] Toda competência carrega nível: `sem` / `pouco` / `domina` + evidência (RN-14)
- [ ] Todo campo carrega classificação `privado` / `publicável` (RN-24)
- [ ] Exportável e apagável por comando do usuário
- [ ] Histórico de alterações preservado

## 4. `rubricas`

- [ ] Existe rubrica explícita e versionada para GitHub, LinkedIn, CV e portfólio (RN-16)
- [ ] Cada item de rubrica é verificável, não subjetivo
- [ ] A mesma rubrica serve aos três usos: avaliar, gerar pergunta, revalidar
- [ ] Rubrica é editável sem tocar no código dos módulos

## 5. `diagnostico`

- [ ] Classifica cada achado em uma das três lacunas: dados, mercado, apresentação
- [ ] **Nunca** classifica como "mercado" algo que a escala de domínio mostra como dominado — isso é apresentação
- [ ] Sem alvo declarado, reporta lacuna de dados e apresentação, e informa que a de mercado depende do alvo
- [ ] Aponta o que está no material atual e não se sustenta no perfil (RN-15)
- [ ] Produz saída priorizada por impacto e esforço, não lista plana

## 6. `entrevista`

- [ ] Pergunta **somente** o que não conseguiu extrair das fontes (RN-1)
- [ ] Perguntas vêm **antes** da geração de qualquer artefato
- [ ] Usa a grade curricular para gerar pergunta, nunca para inferir competência (RN-5)
- [ ] Atribui nível de domínio com evidência anexada (RN-14)
- [ ] O nível atribuído é revisável pelo usuário e nunca bloqueia nada
- [ ] Sessões temáticas curtas; o usuário pode parar e retomar sem perder o que já respondeu
- [ ] Persegue quantificação quando a resposta é vaga

## 7. `cv`

- [ ] Gera versão nacional (PT) e internacional (EN) do mesmo perfil
- [ ] Nenhum conteúdo que não exista no perfil aparece no documento
- [ ] Nenhum campo `privado` aparece
- [ ] Versão internacional omite dados pessoais que são padrão no Brasil e problema fora
- [ ] O usuário decide o que fica, mesmo contra recomendação do sistema (RN-14)

## 8. `linkedin`

- [ ] Ingere export oficial; **não** requer extensão nem credencial
- [ ] Anota print enviado pelo usuário indicando **onde** está cada problema (RN-19)
- [ ] Entrega texto pronto para colar, por seção
- [ ] Nunca escreve no LinkedIn
- [ ] Detecta perfil duplicado e URL malformada quando os dados permitirem

## 9. `repo-doctor`

- [ ] Avalia cada repositório pela rubrica de GitHub
- [ ] Distingue fork de repositório original na análise
- [ ] Sinaliza repositório vazio, sem descrição, sem topics, sem README
- [ ] Gera o texto de README, descrição e topics
- [ ] **Edita apenas mediante confirmação por ação** (G2)
- [ ] Nunca escreve código de projeto (G5)
- [ ] Se não existe repositório, orienta a criar e recebe o link — não cria projeto

## 10. `portfolio-spec`

- [ ] Gera **especificação**, não site: estrutura, conteúdo por seção, nome, paleta, tipografia (RN-17)
- [ ] A especificação é suficiente para o usuário construir sem novas perguntas
- [ ] Recebe o link do site pronto e audita pela rubrica
- [ ] Enquadra projetos como "o quê / pra quê", deixando o "como" para o repositório (RN-2)
- [ ] Uma única URL, com todos os projetos (RN-7)

## 11. `plano`

- [ ] Só roda com alvo declarado (RN-3)
- [ ] Cada item do plano rastreia até uma lacuna de mercado concreta
- [ ] Ordenado por impacto no alvo, não por facilidade
- [ ] Registra progresso ao longo do tempo
- [ ] Competência que sobe de nível dispara re-renderização dos artefatos (RN-18)

## 12. `radar`

- [ ] Toda vaga retornada vem com o motivo do match e o que falta
- [ ] Roda **em paralelo** ao plano, não depois dele (RN-18)
- [ ] Produz lacuna de mercado a partir das vagas reais, não de ideal abstrato (RN-13)
- [ ] Vaga sem match suficiente é reportada com o que falta, não omitida em silêncio

## 13. `antes-depois`

- [ ] Captura baseline antes da primeira alteração de cada superfície (G7)
- [ ] Baseline é imutável depois de gravado
- [ ] Toda ação executada é registrada com data e verificação
- [ ] Gera comparativo com números, não adjetivos
- [ ] Distingue ação executada de ação autorizada mas pendente

---

## Critérios de aceite do MVP como um todo

O MVP está pronto quando, para o usuário #1, **todos** forem verdadeiros:

- [ ] Existe um perfil canônico populado, versionado e exportável
- [ ] Existem CV nacional e internacional gerados dele, sem nada inventado
- [ ] Existe texto pronto para cada seção do LinkedIn, com o print anotado
- [ ] O GitHub foi avaliado e o que corrigir está escrito com o texto pronto
- [ ] Existe especificação de portfólio suficiente para construir o site
- [ ] Existe plano de desenvolvimento rastreado a lacunas de mercado reais
- [ ] Existe lista de vagas com match justificado
- [ ] Existe relatório antes/depois com números
- [ ] Nenhum dos 8 critérios globais foi violado em nenhum momento
