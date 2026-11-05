---
name: entrevista
description: Prepara a entrevista de uma vaga do tracker a partir do currículo que a empresa recebeu - histórias STAR com fatos do perfil-verdade, respostas honestas para as lacunas, perguntas para a empresa e estratégia de salário.
argument-hint: "[empresa] [etapa e data, se souber]"
disable-model-invocation: true
allowed-tools: Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Bash(git *)
---

## Configuração

Leia `~/.claude/emprego-config.json`. Campos: `projeto` (pasta da busca, abaixo `[proj]`), `curriculo_repo` e `curriculo_fonte` (opcionais), `commit` e `mensagem_commit`. Se o arquivo não existir, explique a instalação (README do plugin) e pare.

## Antes de tudo

Leia `[proj]/CLAUDE.md` (regras de ouro, golpes), `[proj]/perfil-verdade.md`, `[proj]/exclusoes.md` e `[proj]/tracker.csv`. Siga as regras de ouro do CLAUDE.md o tempo todo.

Argumentos: $ARGUMENTS.

## O que fazer

1. Encontre a pasta da vaga em `[proj]/vagas/` e leia `vaga.md` e `cobertura.md`. Veja no tracker a versão do currículo enviado e leia essa versão: com tag, `git -C <curriculo_repo> show <tag>:<curriculo_fonte>`; sem tag, o arquivo anotado. A entrevista parte do que a empresa leu.
2. Pesquise a empresa (site, notícias recentes, produto, avaliações de funcionários) e confira se o contato veio por canal oficial (checklist de golpes).
3. Escreva `[proj]/vagas/AAAA-MM-DD-empresa/entrevista.md`, no idioma da entrevista:
   - 5 ou 6 histórias reais no formato STAR (situação, tarefa, ação, resultado), só com fatos e números do perfil-verdade, cada uma servindo a mais de uma pergunta provável;
   - perguntas prováveis para a vaga, com a história que responde cada uma;
   - para cada lacuna da cobertura, uma resposta honesta ("não fiz X; o mais perto foi Y; estou estudando Z");
   - 4 ou 5 perguntas para fazer à empresa;
   - estratégia de salário (faixa de mercado, o que pedir, o que aceitar), com a decisão do usuário.
4. Atualize no tracker o status e o próximo passo.

Lembre ao usuário de reescrever as histórias com as palavras dele: decoradas, elas soam decoradas.

## Gravar

Se `commit` for true e `[proj]` estiver num repositório git: commit só dos arquivos de `[proj]`, com a mensagem de `mensagem_commit` (AAAA-MM-DD = hoje), e push se houver remoto. Não inclua outras mudanças do repositório.
