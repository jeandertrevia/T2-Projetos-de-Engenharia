# Regras de avaliação

A avaliação considera a solução funcionando, a documentação e a capacidade da equipe de justificar as decisões e explicar o que entregou. Os pesos abaixo reproduzem a régua apresentada no documento da disciplina.

| Critério | Peso | O que será observado |
| --- | ---: | --- |
| Aplicação dos conceitos | 25% | Uso dos conceitos do curso e vínculo explícito com as disciplinas anteriores. Essa aplicação é requisito de aprovação. Inovação é bem-vinda, mas não substitui essa conexão. |
| Arquitetura e decisões | 20% | Problema de negócio claro, arquitetura adequada ao problema e decisões técnicas justificadas. |
| Implementação | 20% | Solução funcional e pipeline ponta a ponta, automatizado, versionado e reproduzível. |
| Documentação | 15% | Instruções para executar, explicação das decisões e dos resultados, diagrama e dicionário de dados. O mapa de vínculo com as disciplinas é obrigatório na entrega final. |
| Qualidade de dados | 10% | Validações executáveis, relatório de qualidade, tratamento de falhas e quarentena para registros rejeitados. |
| Apresentação e domínio | 10% | Demonstração do produto e domínio da equipe sobre o código, o fluxo e as escolhas realizadas. |

## Camadas obrigatórias

A tecnologia é livre; a existência das sete camadas abaixo não é opcional. A equipe deve identificá-las no diagrama e na documentação, mesmo quando uma ferramenta atende mais de uma camada.

1. **Ingestão:** ao menos uma fonte externa, em batch ou streaming.
2. **Armazenamento:** persistência organizada em camadas, preservando os dados originais na camada bronze.
3. **Transformação:** limpeza, padronização, enriquecimento ou agregação.
4. **Orquestração:** execução automatizada e monitorável.
5. **Qualidade:** validação dos dados, tratamento de falhas e quarentena de rejeitados.
6. **Consumo:** disponibilização dos dados para análise ou uso por quem decide.
7. **Infraestrutura e versão:** ambiente reproduzível e código versionado.

## Evidências esperadas

- A ingestão pode ser reexecutada e alimenta a camada bronze com dados reais, preservando o original.
- A validação roda e produz relatório de qualidade e quarentena dos registros rejeitados.
- A camada gold pode ser consultada para responder a pelo menos três perguntas de negócio.
- O dicionário de dados está iniciado e é completado na documentação final.
- O pipeline completo pode ser iniciado com um comando, e há uma camada de consumo e uma demonstração.
- O mapa de vínculo com as disciplinas anteriores faz parte da entrega final.
- Os commits mostram a participação dos integrantes da equipe.

## O que enfraquece a entrega

- Um notebook isolado, sem pipeline, camadas ou orquestração.
- Um tutorial copiado sem adaptação ao problema de negócio declarado.
- Código ou ambiente que não pode ser executado por falta de instruções ou configuração reproduzível.
- Histórico de commits que não representa a participação da equipe.
- Documento que lista ferramentas, mas não explica decisões, resultados e limitações.
- Uso de IA como substituto do projeto ou entrega de código que a equipe não sabe explicar. IA pode apoiar a implementação e compor uma etapa da solução; a engenharia de dados ao redor continua sendo avaliada.

O escopo deve ser realista: uma solução menor, completa e funcionando vale mais do que uma solução ambiciosa pela metade.
