> Links: [[core]] · [[../STATE]]

# Estatísticas WakaTime

## Objetivo
Atualizar a atividade de programação dos últimos sete dias completos no README.

## Contexto
O usuário autorizou a migração após receber um aviso do WakaTime. O contrato oficial foi consultado em 31/10/2026: API keys são para escrita dos plugins; scripts pessoais devem usar OAuth. A listagem remota mostrou apenas WAKATIME_API_KEY. Seu valor não foi lido.

## Fluxo (camadas da arquitetura)
GitHub Actions → testes unittest → Python embutido → validação do token OAuth → API WakaTime com Bearer → validação dos sete dias e agregação → substituição da seção waka → commit e push do README.

## Endpoints (se houver)
`GET https://api.wakatime.com/api/v1/users/current/summaries`, com `start`, `end` e `timezone`. A janela termina ontem em `America/Sao_Paulo`.

## Estrutura de Dados (DTOs, Entidades)
Consome `data[].range.date`, `data[].languages[].name`, `data[].languages[].total_seconds` e `cumulative_total.seconds`. O README mostra o total e até dez linguagens.

## Integrações externas (se houver)
- WakaTime: recebe `WAKATIME_OAUTH_TOKEN` do secret GitHub de mesmo nome.
- Bearer obrigatório, token pessoal disponível em https://wakatime.com/oauth e permissão `read_summaries`. O caminho Basic foi removido. Sem client ID/client secret/callback neste uso pessoal.
- GitHub: checkout e atualização do próprio repositório, com permissão `contents: write`.

## Tratamento de Erros
Secret ausente, token sem prefixo OAuth ou prefixo vazio interrompem antes da rede. Falhas 401/403 não são repetidas e orientam conferir validade e read_summaries. Há até quatro tentativas para erros transitórios. Respostas incompletas, totais inválidos e marcadores ausentes também impedem a atualização. Essas falhas preservam o README.

## Testes (curl ou equivalente)
`python -m unittest discover -s tests -v`: sete testes locais aprovados após RED no código antigo. Exercitam o script do próprio YAML, com urllib substituído por respostas sintéticas: nome do secret, token ausente, chave legada, Bearer/período, 401/403, retry transitório e dados incompletos. Relógio/fuso fixos na fixture; não validam a base IANA nem a credencial real. YAML e diff também verificados. Nenhuma consulta autenticada real foi realizada nesta etapa.

## Decisões Técnicas
Secret dedicado para tornar a configuração inequívoca; sem fallback para API key. Manter agregação/período e layout existentes. Sem dependências de produção adicionais ou renovação automática: se o token pessoal expirar/for revogado, atualizar o secret. A validação real exige uma execução autenticada do novo workflow no GitHub Actions. Ver [[../docs/wakatime-oauth]].

## Módulos relacionados
- [[core]]
- [[../STATE]]
- [[../docs/wakatime-oauth]]
- [[../.planning/2026-10-31-wakatime-oauth]]

## Histórico
| Data | Ação |
| --- | --- |
| 2026-10-31 | Diagnóstico estático inicial e registro das pendências de OAuth. |
| 2026-10-31 | Migração para secret OAuth dedicado implementada, sete testes aprovados e configuração remota inspecionada sem valores de secrets. |
