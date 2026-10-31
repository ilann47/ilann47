> Links: [[../STATE]] · [[../brain/wakatime]] · [[../docs/wakatime-oauth]]

# Migração WakaTime OAuth

Autorização: usuário aprovou seguir com a migração e verificar configuração no GitHub.

## Ordem e verificação
1. Conferir contrato oficial e configuração remota, sem valores de credenciais.
   Concluído: OAuth pessoal, Bearer e read_summaries; falta o secret dedicado.
2. Criar `tests/test_waka_workflow.py` antes de alterar produção.
   Concluído: RED observado no script original por uso do nome antigo de secret.
3. Ajustar `.github/workflows/waka-readme.yml`: secret dedicado, Bearer obrigatório,
   rejeição de chave legada e mensagens operacionais; executar testes antes da consulta.
   Concluído: sete testes GREEN, YAML válido, `git diff --check` sem erros.
4. Atualizar `.gitignore`, `docs/wakatime-oauth.md`, [[../brain/core]],
   [[../brain/wakatime]] e [[../STATE]]. Revisar diff e registrar commit lógico.
5. Publicar a branch para revisão, configurar secret e executar na branch.
   Credencial depende de configuração segura pelo usuário; nenhuma leitura de valor.
6. Integrar à main e confirmar execução do novo workflow para ativar o agendamento.
   Integração depende de autorização; não inferir migração ativa a partir de teste local.

## Riscos e limites
- Sem secret válido a consulta falha antes da rede e o README é preservado.
- Agendamento continua na versão da main até integração.
- Sem alteração do período, agregação, layout do README ou dependências de produção.
- HTTP substituído nos testes; relógio e fuso fixos na fixture permitem execução
  em Windows sem tzdata. Nenhuma validação da base IANA foi alegada.
