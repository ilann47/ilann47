> Links: [[wakatime]] · [[../STATE]]

# Perfil GitHub ilann47

## Objetivo
Manter o README público do perfil `ilann47`, incluindo estatísticas do WakaTime.

## Arquitetura e infraestrutura
- `README.md`: conteúdo do perfil e seção delimitada por marcadores `waka`.
- `.github/workflows/waka-readme.yml`: GitHub Actions com Python embutido para consultar o WakaTime e atualizar o README.
- Execução manual ou diária às 09:00 UTC, equivalente a 06:00 em America/Sao_Paulo.
- O próprio workflow faz commit e push quando as estatísticas mudam.
- Não há manifesto de aplicação ou comando de build.
- `tests/test_waka_workflow.py`: suíte unittest do script real embutido, sem dependências adicionais. O workflow roda os testes antes da consulta autenticada.
- `docs/wakatime-oauth.md`: configuração operacional e validação no GitHub.

## Padrões de trabalho
Seguir o AGENTS.md do diretório pai. Planejar a implementação antes de alterar código e atualizar o brain nas mudanças. Não ler nem registrar valores de credenciais.

## Módulos relacionados
- [[wakatime]]
- [[../STATE]]

## Histórico
| Data | Ação |
| --- | --- |
| 2026-10-31 | Checkout preparado e arquitetura inicial documentada a partir do código. |
| 2026-10-31 | Autenticação exclusivamente OAuth e testes locais adicionados; instruções de configuração documentadas. |
