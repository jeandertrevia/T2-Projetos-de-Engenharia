# Entregas das equipes

Cada equipe deve adicionar uma única pasta neste diretório, com o nome `entregas/<nome-da-equipe>/`, e enviar a alteração em um Pull Request. Não edite a pasta `_modelo`; copie-a para criar a pasta da equipe.

```text
entregas/
├── README.md
├── _modelo/                 # Estrutura inicial; não é uma entrega
└── nome-da-equipe/          # Uma pasta por equipe
    ├── README.md            # Problema, solução e como executar
    ├── docs/
    │   └── arquitetura.md
    ├── src/                 # Código do projeto
    ├── tests/               # Testes e validações
    ├── docker-compose.yml   # Quando aplicável
    └── .env.example         # Nomes das variáveis, sem segredos
```

O projeto pode adaptar essa estrutura, desde que continue fácil de entender e executar. Não versione credenciais, `.env` preenchido, dados pessoais ou conjuntos de dados cuja distribuição não esteja autorizada. Prefira scripts de download autorizado, dados sintéticos ou amostras pequenas e anonimizadas quando forem suficientes para reproduzir a demonstração.

Consulte [Como enviar](../COMO_ENVIAR.md) para o fluxo de branch e Pull Request e [Regras de avaliação](../REGRAS_DE_AVALIACAO.md) para os critérios da disciplina.
