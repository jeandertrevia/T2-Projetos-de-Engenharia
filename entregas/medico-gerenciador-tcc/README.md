# Avaliação de Transição de Nível de Cuidado — medico-gerenciador-tcc

## Equipe

- **Integrantes:**
  - Jeander Trevia — matrícula 2519758
  - Joabe Levi — matrícula 2518430
  - Lianderson Falcão — matrícula 2518671
- **Repositório/código:** https://github.com/joabe-levi/project_tcc (código completo também replicado em `src/` nesta pasta, snapshot da entrega)

## Problema de negócio

Em hospitais e operadoras de saúde, a decisão de mudar o nível de cuidado de
um paciente internado — sair da UTI, ir para Semi-UTI, Enfermaria ou Home
Care, ou precisar escalonar de volta para a UTI — é feita hoje de forma
subjetiva pelo profissional responsável ("médico gerenciador"), sem registro
estruturado e sem nenhum indicador de consistência entre o que os critérios
clínicos objetivos sugerem e o que é de fato decidido.

Isso gera dois riscos opostos: leito ocupado além do necessário (custo
desnecessário para a operadora) e desescalonamento/alta precoce insegura
(risco clínico). A solução resolve isso registrando cada avaliação de forma
estruturada e calculando, de forma independente, se a decisão do profissional
está alinhada com os critérios clínicos.

**Perguntas de negócio que a solução responde** (ver `docs/dicionario_de_dados.md` para a lista completa):
1. Os profissionais concordam com o que os critérios clínicos recomendariam?
2. Quando uma transição é recomendada, ela de fato acontece na prática?
3. Existe diferença de comportamento (conservador vs. agressivo) entre hospitais ou profissionais?
4. Quanto a ferramenta economiza, em termos de custo de leito, ao identificar desescalonamentos seguros?
5. O uso da ferramenta está crescendo (indicador de produto)?

*Reprodução independente de um conceito de produto real (Médico Gerenciador),
sem nenhum dado real de empresa — estrutura, schema e dados totalmente
sintéticos, desenhados do zero para este trabalho.*

## Solução e arquitetura

Um profissional preenche um formulário (app Streamlit, rodando como
Databricks App) a cada avaliação clínica. O envio grava um JSON bruto num
Volume do Unity Catalog, o que dispara automaticamente (file arrival
trigger) um Job no Databricks que: (1) ingere o JSON como Delta (camada
Bronze) via Auto Loader; (2) roda um pipeline dbt em camadas — staging →
**qualidade** (validação + quarentena) → intermediate (cálculo do indicador)
→ prata → ouro. As tabelas Ouro alimentam um dashboard (Databricks AI/BI)
com 3 páginas: indicadores clínicos, economia/produto, e qualidade de dados.

Diagrama completo e mapeamento das 7 camadas obrigatórias: **[`docs/arquitetura.md`](docs/arquitetura.md)**.

Todo o Job e o Dashboard são definidos como código (Databricks Asset
Bundle) e o deploy é automatizado via CI/CD (GitHub Actions), sem
configuração manual no ambiente.

## Como executar

### Pré-requisitos

- Conta Databricks (usamos o Free Edition) com Unity Catalog habilitado.
- [Databricks CLI](https://docs.databricks.com/dev-tools/cli/index.html) autenticado.
- Python 3.10+ e `dbt-databricks` para rodar/testar localmente.
- Um SQL Warehouse ativo no workspace.

### Configuração

1. Crie o catalog/schemas e o Volume de ingestão no Unity Catalog (`catalog.ingestao.formularios_json`).
2. Copie `src/.env.example` para `.env` e preencha com o host/warehouse do seu workspace (nunca commitar `.env` preenchido).
3. Ajuste `src/pipeline/databricks.yml` e `src/pipeline/resources/*.yml` (host do workspace, `warehouse_id`) para o seu ambiente.

### Execução

```bash
cd src/pipeline
databricks bundle deploy -t dev
databricks bundle run pipeline_tcc_medallion
```

Isso builda e sobe o Job (ingestão + dbt) e o Dashboard. Depois do primeiro
deploy, o Job passa a rodar sozinho sempre que um novo formulário é
preenchido (file arrival trigger) — não precisa rodar manualmente no dia a
dia.

### Testes e qualidade

```bash
cd src/pipeline/dbt
dbt build   # roda os 44 testes (unicidade, not-null, dominio, integridade referencial)
```

Ver **[`tests/README.md`](tests/README.md)** para onde consultar o relatório
de qualidade e como a quarentena de registros rejeitados funciona (com
evidência de teste real).

## Dados e resultados

- **Fontes:** formulário clínico preenchido pelo profissional (app
  Streamlit), sem nenhum dado real de paciente — todos os dados são
  sintéticos, gerados para este projeto.
- **Camadas:** `ingestao` (Volume, JSON bruto) → `bronze` (Delta, 1:1 com o
  bruto) → `qualidade` (validação + quarentena) → `prata` (consolidado) →
  `ouro` (marts finais, consumidos pelo dashboard).
- **Perguntas respondidas pela camada gold:** ver seção "Problema de
  negócio" acima (5 perguntas).
- **Dicionário de dados:** [`docs/dicionario_de_dados.md`](docs/dicionario_de_dados.md).
- **Dashboard:** 3 páginas (Visão Geral, Economia & Produto, Qualidade de
  Dados) — link disponível via `databricks bundle summary` após o deploy.

## Vínculo com as disciplinas

> Pendente de preenchimento final — mapa completo será adicionado assim que
> o cronograma das disciplinas anteriores for consolidado pela equipe.

## Demonstração, limitações e decisões

**Demonstração:** preencher um formulário no app → acompanhar o Job disparar
automaticamente no Databricks Jobs UI → conferir as tabelas `bronze` →
`qualidade` → `prata` → `ouro` populadas → ver o dashboard atualizado.

**Limitações conhecidas:**
- Dataset é sintético (217 avaliações, 60 internações) — não há dado real de
  paciente ou de hospital.
- O custo diário por nível de cuidado (`seed_custo_diario_leito`) é uma
  estimativa de referência para fins de cálculo do indicador de economia,
  não uma tabela de preços real de nenhuma operadora.
- Ambiente single-tenant (um catálogo só) — não há isolamento entre
  clientes/hospitais diferentes, o que seria necessário para um produto
  comercial de verdade.

**Decisões que a equipe pode justificar:**
- Databricks + Unity Catalog como plataforma única, para não precisar manter
  infraestrutura própria durante o TCC.
- Camada de qualidade como *gate* real (registro inválido não avança), não
  como validação decorativa — testado com registros deliberadamente
  malformados.
- Databricks Asset Bundles + GitHub Actions para que o deploy nunca dependa
  de configuração manual pela UI.
