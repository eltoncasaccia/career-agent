# Sistema de Carreira

Um produto que leva uma pessoa de *"não sei nem por onde começar"* até
contratada — arrumando currículo, LinkedIn, GitHub e portfólio no caminho.

**Fase atual:** especificação (upstream). Nenhum código ainda, por decisão.

## Estrutura

| Caminho | O que é |
|---|---|
| [`PROBLEMA.md`](PROBLEMA.md) | **Problem space.** O quê, em uma página. Sem solução |
| [`spec/SOLUCAO.md`](spec/SOLUCAO.md) | **Solution space.** 25 regras de negócio, 17 módulos, riscos, mercado |
| [`spec/CRITERIOS-DE-ACEITE.md`](spec/CRITERIOS-DE-ACEITE.md) | Critérios verificáveis, escritos antes da implementação |
| `dados/` | Dados do usuário #1 — **não versionado**, permanece local (RN-24) |

## Método

Especificação antes de código, seguindo:

1. Definir o **problema**, não a solução — uma página, foco no "o quê"
2. Separar **problem space** de **solution space**
3. **Versionar specs no git**, junto do código
4. Escrever **critérios de aceite primeiro**
5. Projetar a **arquitetura antes** de escrever qualquer linha (fase upstream)

Os passos 1 a 4 estão feitos. O passo 5 é o próximo.

## Próximo passo

Fase técnica: arquitetura, linguagem, se é multiagente, quais frameworks — em
documento próprio, fora dos acima. A única direção já registrada é a de runtime
(Claude Code primeiro, depois ADK/LangGraph — [`SOLUCAO.md` §11](spec/SOLUCAO.md#11-decisões-tomadas));
a fase técnica confirma ou revisa.
