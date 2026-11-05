---
name: retro
description: Retrospectiva quinzenal da busca de emprego - números por fonte, família, modelo de trabalho e currículo enviado, comparação com a anterior e pelo menos uma decisão.
argument-hint: "(sem argumentos)"
disable-model-invocation: true
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(git *)
---

## Configuração

Leia `~/.claude/emprego-config.json`. Campos: `projeto` (pasta da busca, abaixo `[proj]`), `commit` e `mensagem_commit`. Se o arquivo não existir, explique a instalação (README do plugin) e pare.

## Antes de tudo

Leia `[proj]/CLAUDE.md`, `[proj]/tracker.csv`, todas as rodadas em `[proj]/vagas/` e a retrospectiva anterior em `[proj]/retrospectivas/`, se houver.

## O que fazer

1. Conte, desde a última retrospectiva: vagas encontradas, triadas, candidaturas enviadas, respostas, entrevistas, recusas e sem retorno. Quebre por fonte, família, modelo de trabalho e currículo enviado.
2. Compare com a retrospectiva anterior: o que melhorou, o que piorou.
3. Escreva `[proj]/retrospectivas/AAAA-MM-DD.md` com os números, o que eles sugerem e **pelo menos uma decisão** (manter, ajustar ou parar algo) para o usuário confirmar.
4. Acrescente uma linha sobre esta retrospectiva na seção "Estado" do `[proj]/CLAUDE.md`, com a data da próxima (14 dias depois).

## Gravar

Se `commit` for true e `[proj]` estiver num repositório git: commit só dos arquivos de `[proj]`, com a mensagem de `mensagem_commit` (AAAA-MM-DD = hoje; resumo "retrospectiva"), e push se houver remoto. Não inclua outras mudanças do repositório.

## Relatar

Os números principais, a comparação e a decisão proposta.
