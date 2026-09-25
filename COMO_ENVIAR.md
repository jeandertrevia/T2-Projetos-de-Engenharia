# Como enviar o trabalho (Pull Request)

O trabalho final deve ser enviado neste repositório em `entregas/<nome-da-equipe>/`. Abra um Pull Request (PR) para que a entrega fique versionada e possa ser revisada.

## 1. Prepare a pasta da equipe

1. Faça um fork do repositório para sua conta ou use o fluxo de contribuição definido pelo professor.
2. Clone o fork e crie uma branch para a entrega:

   ```bash
   git clone <URL-DO-SEU-FORK>
   cd <NOME-DO-REPOSITORIO>
   git switch -c entrega/<nome-da-equipe>
   ```

3. Copie `entregas/_modelo/` para `entregas/<nome-da-equipe>/` e complete os documentos. Use um nome curto, legível e consistente para a equipe.
4. Inclua o código, os testes, os arquivos de configuração e as instruções necessárias para reproduzir a execução. Preserve a organização do modelo; acrescente subpastas quando o projeto precisar.

## 2. Revise antes de publicar

- Confirme que o README da equipe explica o problema, a solução, os pré-requisitos e como executar o pipeline.
- Teste as instruções em um ambiente limpo ou containerizado. O pipeline deve subir com um comando, conforme o escopo apresentado na disciplina.
- Inclua evidências e documentação, como diagrama de arquitetura, dicionário de dados, resultados e mapa de vínculo com as disciplinas anteriores.
- Verifique que os commits representam as contribuições reais dos integrantes. O material da disciplina pede commit de cada integrante.
- Remova credenciais, tokens, chaves, arquivos `.env` locais, dados pessoais e arquivos grandes ou restritos. Forneça `.env.example` sem valores secretos e instruções para obter os dados de forma autorizada.
- Não entregue apenas um notebook: inclua as camadas, a execução automatizada, a validação de dados e a forma de consumo esperadas para o projeto.

## 3. Envie a branch e abra o PR

```bash
git add entregas/<nome-da-equipe>
git commit -m "Adiciona projeto da equipe <nome-da-equipe>"
git push -u origin entrega/<nome-da-equipe>
```

No GitHub, abra um Pull Request da sua branch para a branch principal (`main`) deste repositório. Se estiver trabalhando em um fork, selecione o repositório original como destino.

### Título sugerido

```text
Entrega final: <nome da equipe> — <nome do projeto>
```

### Descrição do PR

Informe os nomes dos integrantes e preencha esta lista:

- [ ] A pasta está em `entregas/<nome-da-equipe>/`.
- [ ] O README documenta problema, arquitetura, pré-requisitos e execução.
- [ ] O pipeline foi executado e o resultado pode ser reproduzido.
- [ ] As sete camadas obrigatórias estão identificadas ou justificadas na documentação.
- [ ] Há validações, relatório de qualidade e tratamento/quarentena de rejeitados.
- [ ] O dicionário de dados e o mapa de vínculo com disciplinas anteriores estão incluídos.
- [ ] Não há segredos, dados não autorizados ou artefatos desnecessários no diff.
- [ ] Todos os integrantes estão identificados e suas contribuições estão representadas no histórico de commits.

Descreva também o que foi implementado, como reproduzir a demonstração, limitações conhecidas e qualquer decisão que os revisores devam observar. Responda aos comentários de revisão no próprio PR e atualize a mesma branch até a aprovação.

## Prazo e formato

O PDF da disciplina indica **30/09** para a Entrega 2 (parte técnica rodando e versionada) e **30/10** para a Entrega 3 (documento final por e-mail). Este repositório orienta a submissão do projeto por PR; confirme com o professor se o PR substitui também o envio por e-mail do documento final.
