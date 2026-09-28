# Testes e validações

Os testes vivem junto do código dbt, em `src/pipeline/dbt/` — schema tests
(`_*.yml`) declarados ao lado de cada model, executados junto do build.

## Como rodar

```bash
cd src/pipeline/dbt
pip install dbt-databricks
dbt deps
dbt build   # roda run + test, na ordem certa
```

`dbt build` executa, na ordem: seeds → staging → **camada de qualidade**
(`models/qualidade/`) → intermediate → prata → ouro, e roda todos os testes
declarados a cada passo (unicidade, not-null, domínio de valores,
integridade referencial). Total atual: **44 testes** (0 falhas na última
execução).

## Onde consultar o relatório de qualidade e a quarentena

A camada de qualidade (`src/pipeline/dbt/models/qualidade/`) roda como parte
do pipeline normal, não é um passo separado:

- `qualidade.formularios_validados` — cada avaliação, com 9 flags de
  validação e o flag geral `registro_valido`.
- `qualidade.quarentena_formularios` — só os registros **rejeitados**, com o
  motivo (`motivos_rejeicao`). Não seguem para as camadas seguintes.
- `qualidade.relatorio_qualidade_resumo` — total processado, válido,
  rejeitado e % de qualidade da última execução.
- `qualidade.relatorio_qualidade_por_regra` — quantos registros falharam em
  cada uma das 9 regras.

Essas 4 tabelas também aparecem na página **"Qualidade de Dados"** do
dashboard (ver link no README da equipe).

## Evidência de que a quarentena funciona de verdade

Testamos com 7 registros deliberadamente malformados (um por regra de
validação) injetados no volume de ingestão junto aos 217 registros válidos:

| Métrica | Valor |
|---|---|
| Total processado | 224 |
| Válidos | 217 |
| Rejeitados (quarentena) | 7 |
| % de qualidade | 96,88% |

Cada uma das 9 regras capturou exatamente o registro que a violava, e a
camada Ouro (`mart_transicoes` etc.) permaneceu com 217 linhas — os
registros rejeitados não contaminaram os indicadores finais.
