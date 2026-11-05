---
name: triar
description: Tria vagas da última busca (pelos números) ou uma vaga avulsa (link ou texto), com aderência baseada só em evidências do perfil-verdade, lacunas e alertas de golpe.
argument-hint: "[números da busca, ex.: 3 5 8] ou [link ou texto da vaga]"
disable-model-invocation: true
allowed-tools: Read, Write, Edit, Glob, Grep, WebFetch, Bash(git *)
---

## Configuração

Leia `~/.claude/emprego-config.json`. Campos: `projeto` (pasta da busca, abaixo `[proj]`), `commit` e `mensagem_commit`. Se o arquivo não existir, explique a instalação (README do plugin) e pare.

## Antes de tudo

Leia `[proj]/CLAUDE.md` (regras de ouro, critérios, famílias, golpes), `[proj]/perfil-verdade.md`, `[proj]/exclusoes.md` e `[proj]/tracker.csv`. Se o perfil-verdade ainda for o modelo (comentário `modelo:` no topo), peça para rodar `/emprego:perfil` primeiro e pare. Siga as regras de ouro do CLAUDE.md o tempo todo.

Argumentos: $ARGUMENTS. Números se referem à rodada mais recente em `[proj]/vagas/*-busca.md`. Um link ou texto colado é uma vaga avulsa.

## O que fazer

1. Para cada vaga, leia o anúncio completo (abra o link). Se não abrir, peça para o usuário colar o texto.
2. Monte uma linha por vaga:

   `nº | vaga/empresa | família | aderência (alta, média ou baixa) | evidência (trecho do perfil-verdade) | lacunas obrigatórias | alertas | recomendação`

   - Aderência **alta** só quando todo requisito obrigatório tem evidência no perfil-verdade. Requisito sem evidência é "sem evidência", nunca suposição.
   - Alertas: golpe (checklist do CLAUDE.md), mudança de cidade, regime fora do comum, empresa em exclusões.
   - Recomendação: candidatar, candidatar com ressalva (diga qual) ou pular.
3. Grave a triagem no arquivo da busca, numa seção `## Triagem (AAAA-MM-DD)`. Vaga avulsa vai para `[proj]/vagas/AAAA-MM-DD-avulsas.md`.
4. Se aparecer um padrão de vaga indesejada, sugira (sem gravar) `/emprego:perfil não quero: ...`.

## Gravar

Se `commit` for true e `[proj]` estiver num repositório git: commit só dos arquivos de `[proj]`, com a mensagem de `mensagem_commit` (AAAA-MM-DD = hoje), e push se houver remoto. Não inclua outras mudanças do repositório.

## Relatar

A tabela e, para as recomendadas, o próximo passo: `/emprego:candidatar <nº ou empresa>`.
