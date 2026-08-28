---

## Prova de Conceito — Bloco 1 (Concluído)

### Teste executado

- **Arquivo analisado:** `src/lgapdb01.cbl` (Genapp — IBM demonstration insurance application)
- **Ferramenta:** IBM Bob, modo Ask
- **Custo:** 0.124 Bobcoin
- **Contexto consultado pelo Bob:** arquivo principal + copybooks (`lgcmarea.cpy`, `lgpolicy.cpy`)

### Resultado

O pipeline gerou as três saídas planejadas com sucesso, em um único
programa de produção real (CICS/DB2):

1. **Arquitetura** — função principal identificada corretamente ("ADD
   Policy"), mapa de dependências (copybooks, includes DB2, programas
   CICS linkados) e diagrama de fluxo de execução.
2. **Glossário** — tradução completa de ~30 campos COBOL (COMMAREA,
   campos por tipo de apólice, variáveis internas de infraestrutura
   CICS) para linguagem de negócio.
3. **Regras de negócio** — 6 regras de decisão identificadas, incluindo
   validações, roteamento por tipo de apólice, geração de número via
   auto-incremento DB2, e tratamento de erros com tabela completa de
   códigos de retorno.

### Achado de maior valor (prova do conceito central do projeto)

O Bob identificou uma **regra de negócio não-óbvia e não documentada**:
uma assimetria no comportamento de rollback entre a tabela-mãe
(`POLICY`) e as tabelas-filha (Endowment/House/Motor/Commercial).

- Falha no INSERT da tabela-mãe → rollback automático (comportamento
  padrão do CICS).
- Falha no INSERT da tabela-filha → como a tabela-mãe **já foi
  commitada**, o programa força deliberadamente um `ABEND` (código
  `LGSQ`) só para acionar o rollback da linha órfã.

O Bob traduziu essa lógica para o equivalente conceitual em Java
(lançar uma `RuntimeException` para forçar rollback do container),
tornando a regra imediatamente compreensível para um dev moderno sem
conhecimento prévio de CICS/DB2.

**Por que isso importa:** essa é exatamente a categoria de
conhecimento tribal que motivou o projeto — uma decisão de design que
só um especialista CICS/DB2 explicaria, agora documentada e traduzida
automaticamente.

### Bônus — detecção de risco não solicitada

O Bob sinalizou, por conta própria, um possível bug latente: o
programa de pós-processamento `LGAPVS01` é chamado mesmo quando
`CA-REQUEST-ID` é inválido (código de erro 99, caindo no `WHEN OTHER`
do segundo `EVALUATE`). Isso demonstra que a ferramenta vai além da
documentação passiva — ajuda a identificar riscos reais no código.

### Validação do gate do Bloco 1

✅ Pipeline básico funcionando de ponta a ponta em um programa COBOL
real de produção.
✅ As três saídas (arquitetura, glossário, regras de negócio) geradas
com qualidade suficiente para onboarding real.
✅ Custo em Bobcoin baixo o suficiente para ser executado em escala
(0.124 por programa de ~porte médio).

**Próximo passo:** prototipar subagente isolado (P2) antes de escalar
para análise paralela do sistema completo (Bloco 2).