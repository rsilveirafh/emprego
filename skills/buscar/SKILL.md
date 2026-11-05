---
name: buscar
description: Busca vagas novas para uma família de vaga (ou todas), sem repetir nada das rodadas anteriores nem das exclusões, e salva a rodada numerada no projeto.
argument-hint: "[sigla da família | todas] [filtro opcional: remoto, cidade, internacional...]"
disable-model-invocation: true
allowed-tools: Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Bash(git *)
---

## Configuração

Leia `~/.claude/emprego-config.json`. Campos: `projeto` (pasta da busca, abaixo `[proj]`), `commit` e `mensagem_commit`. Se o arquivo não existir, explique a instalação (README do plugin) e pare.

## Antes de tudo

Leia `[proj]/CLAUDE.md` (regras de ouro, critérios, famílias, golpes), `[proj]/perfil-verdade.md`, `[proj]/exclusoes.md` e `[proj]/tracker.csv`. Se o perfil-verdade ainda for o modelo (comentário `modelo:` no topo), peça para rodar `/emprego:perfil` primeiro e pare. Siga as regras de ouro do CLAUDE.md o tempo todo.

Argumentos: $ARGUMENTS (sigla da família e, opcionalmente, um filtro). Sem família, use `todas`.

## O que fazer

1. Veja as rodadas anteriores em `[proj]/vagas/*-busca.md`: a numeração continua de onde a última parou, e vaga já listada ou já no tracker não volta.
2. Pesquise vagas publicadas nos **últimos 14 dias** que se encaixem na família e nos critérios do CLAUDE.md (modelo, regime, piso). Use a busca da web, no modo mais completo disponível. Escolha as fontes pela família e pelo país: portais gerais (LinkedIn, Indeed, Glassdoor), portais locais e de nicho (no Brasil, por exemplo, Gupy, Programathor, Remotar, Trampos), vagas remotas internacionais (Wellfound, We Work Remotely, RemoteOK), sites de empresas, ONGs e órgãos públicos da área, e, para vagas acadêmicas, editais, bolsas e concursos.
3. Para cada vaga, confirme que o link abre e que a vaga ainda está aberta. Se não deu para confirmar, marque `link não confirmado`.
4. Salve em `[proj]/vagas/AAAA-MM-DD-busca.md` (se já existir no dia, acrescente uma seção `## Rodada 2`). Cabeçalho com data, família, filtro e fontes consultadas. Depois, uma tabela:

   `nº | vaga | empresa | família | candidatura (simplificada ou externa) | modelo | regime | publicada em | link | requisitos principais | alertas`

   Em alertas: exige mudança de cidade, sinais de golpe (checklist do CLAUDE.md), regime fora do comum.
5. Não faça triagem aqui: só a lista.

## Gravar

Se `commit` for true e `[proj]` estiver num repositório git: commit só dos arquivos de `[proj]`, com a mensagem de `mensagem_commit` (AAAA-MM-DD = hoje), e push se houver remoto. Não inclua outras mudanças do repositório.

## Relatar

Quantas vagas novas, por família e por tipo de candidatura, quais têm alerta, e o próximo passo: `/emprego:triar <números>` com as que parecem mais aderentes.
