> Links: [[../brain/wakatime]] · [[../brain/core]] · [[../STATE]]

# Configuração OAuth do WakaTime no GitHub

## Contrato
O workflow consulta `GET https://api.wakatime.com/api/v1/users/current/summaries`
com `Authorization: Bearer <access_token>` e permissão `read_summaries`.
API keys dos plugins não são aceitas pelo workflow.

Para este README pessoal, o WakaTime oferece um token OAuth em
[wakatime.com/oauth](https://wakatime.com/oauth). Não é necessário publicar um
aplicativo OAuth ou configurar client ID, client secret e callback.
Fonte: [documentação oficial de autenticação](https://wakatime.com/developers/#authentication).

## Configurar a credencial
1. Entre na conta WakaTime que contém suas estatísticas e abra
   [OAuth pessoal](https://wakatime.com/oauth). Copie o token OAuth, com prefixo
   `waka_tok_`, e confira a permissão de leitura dos resumos quando apresentada.
2. Em [Settings → Secrets and variables → Actions](https://github.com/ilann47/ilann47/settings/secrets/actions),
   crie um **Repository secret** com o nome exato `WAKATIME_OAUTH_TOKEN` e o token
   como valor. Se já existir, atualize-o. Não use uma variável pública de Actions.
3. Não envie o token em chat nem grave seu valor em código, documentação ou logs.
   Mantenha as API keys dos plugins nos próprios plugins.

O secret antigo `WAKATIME_API_KEY` deixa de ser referenciado por este workflow.
Sua remoção não é necessária para ativar OAuth; esta mudança não o exclui.

## Validar no GitHub
Após publicar a branch e cadastrar o secret, execute o workflow na branch desejada:

```powershell
gh workflow run waka-readme.yml --repo ilann47/ilann47 --ref fix/wakatime-oauth
gh run list --repo ilann47/ilann47 --workflow waka-readme.yml --branch fix/wakatime-oauth --limit 3
```

Confira os passos de teste, geração e commit em [Actions](https://github.com/ilann47/ilann47/actions/workflows/waka-readme.yml).
Na branch de migração, o commit automático atualiza essa branch. O agendamento
diário só usa o novo código depois de integrá-lo à `main`.
Uma execução bem-sucedida sem mudanças de estatísticas pode terminar sem commit.

O secret não é recuperável pelo GitHub. A existência do nome não comprova validade,
permissões ou conta do token: a consulta real bem-sucedida é necessária.
HTTP 401/403 interrompe a execução, preserva o README e orienta conferir o token
e `read_summaries`. Se o token expirar ou for revogado, atualize o mesmo secret.
Este workflow não implementa renovação automática de tokens.

## Evidências de 31/10/2026
- Antes da migração, havia somente `WAKATIME_API_KEY`, criado e atualizado em
  `2026-10-24T01:10:09Z`. Nenhum valor de secret foi lido.
- Permissão padrão do token do Actions: `write`; o workflow declara `contents: write`.
- Execução anterior à migração: [41213024700](https://github.com/ilann47/ilann47/actions/runs/41213024700),
  iniciada em `2026-10-31T15:03:46Z`, conclusão `success`. Não comprova OAuth.
- Testes locais do script real embutido, com HTTP substituído por respostas
  sintéticas: `python -m unittest discover -s tests -v` — sete testes aprovados.
- Testes validam Bearer, rejeição de API key, ausência de token, 401/403 sem retry,
  retry transitório e preservação da seção em dados incompletos. Não comprovam
  credencial real ou resposta atual do provedor.
