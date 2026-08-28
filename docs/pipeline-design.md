Aqui está o conteúdo. Copia tudo e cola no Notepad/VS Code quando abrir o arquivo:

markdown
# Design do Pipeline — Bob para Modernização de COBOL

## Objetivo do Projeto

Usar o IBM Bob 2.0 para automatizar o trabalho do especialista COBOL que está
sumindo do mercado: apontar para código legado e gerar documentação de
onboarding (arquitetura, glossário e regras de negócio) que hoje não existe.

**Diferencial:** em vez de traduzir/migrar COBOL para outra linguagem (como o
WCA4Z da IBM), atacamos o gargalo real — destravar o conhecimento preso no
código para que qualquer dev moderno entenda o sistema em minutos, não em
meses.

**Métrica de impacto:** tempo para entender um trecho de COBOL, de horas para
minutos.

---

## Bloco 0 — Validação do MVP (concluído)

- **O que foi feito:** rodamos o Bob em modo Ask sobre um arquivo COBOL do
  Genapp, pedindo "explique este código".
- **Critério de gate:** medir o custo em Bobcoin. Se o custo for baixo e a
  explicação for útil, o projeto segue.
- **Resultado:** validado. Gasto de tokens baixo, explicação funcional.

---

## Bloco 1 — Núcleo do Pipeline (em andamento)

### Objetivo
Construir o pipeline básico: um programa COBOL → pacote de onboarding
completo, de forma estruturada e repetível (não uma pergunta avulsa).

### Fluxo: Plan → Agent

O pipeline gera três saídas a partir de um arquivo/trecho COBOL:

1. **Arquitetura** — como os módulos do código se conectam entre si.
2. **Glossário** — tradução de termos e nomes de variáveis/campos COBOL para
   linguagem de negócio.
3. **Regras de negócio** — o que o código decide e por quê (lógica de
   negócio escondida em condicionais, cálculos, validações).

### Repositório de teste

- **VAULT-CBS** (`NSEvent/cobal-banking-system`) — core banking system com
  32.000+ linhas de COBOL, 25 módulos e 612 testes. Implementa gestão de
  contas, empréstimos, contabilidade de razão dupla, ACH, cálculo de juros e
  compliance regulatório.

### Trabalho em paralelo (P2)

Enquanto o pipeline básico é construído, prototipamos **um subagente
isolado** para validar que a abordagem de subagentes funciona antes de
escalar para o sistema inteiro (Bloco 2 — subagentes paralelos dissecando o
Genapp completo).

### Meta do bloco

Camada 2 completa — pipeline básico funcionando de ponta a ponta em um
trecho de COBOL, com as três saídas (arquitetura, glossário, regras de
negócio) geradas.

---

## Próximos blocos (referência rápida)

- **Bloco 2 — Escala:** subagentes paralelos dissecando o sistema completo;
  captura de número de impacto (tempo antes vs. depois).
- **Bloco 3 — Coroação:** empacotar tudo num workflow repetível; diagrama
  visual (mermaid); Q&A interativo se sobrar tempo.

---

*Última atualização: 28/08/2026*
