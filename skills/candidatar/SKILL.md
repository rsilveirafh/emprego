---
name: candidatar
description: Prepara uma candidatura (anúncio salvo, revisão de cobertura contra o currículo e o perfil-verdade e, se pedido, uma versão do currículo só para a vaga, gerada na pasta dela) e, depois que o usuário enviar, registra no tracker com a versão exata do currículo.
argument-hint: "[nº da busca, empresa ou link] | [enviei <empresa>]"
disable-model-invocation: true
allowed-tools: Read, Write, Edit, Glob, Grep, WebFetch, Bash(git *), Bash(sh *)
---

## Configuração

Leia `~/.claude/emprego-config.json`. Campos:
- `projeto`: pasta da busca (abaixo, `[proj]`);
- `curriculo_repo`, `curriculo_fonte`, `curriculo_compilar`: repositório do currículo, arquivo com o texto dele e comando que gera os PDFs (opcionais);
- `curriculo_compilar_vaga`: comando que gera os PDFs de uma **versão da vaga**, rodado dentro de `curriculo_repo`, em que `{arquivo}` é o caminho do arquivo da versão e `{pasta}` a pasta da vaga (opcional);
- `commit` e `mensagem_commit`: se e como commitar ao final.

Se o arquivo não existir, explique a instalação (README do plugin) e pare.

## Antes de tudo

Leia `[proj]/CLAUDE.md` (regras de ouro, critérios, famílias, tabela "Currículos", golpes), `[proj]/perfil-verdade.md`, `[proj]/exclusoes.md` e `[proj]/tracker.csv`. Se o perfil-verdade ainda for o modelo (comentário `modelo:` no topo), peça para rodar `/emprego:perfil` primeiro e pare. Siga as regras de ouro do CLAUDE.md o tempo todo. **Quem envia a candidatura é o usuário, nunca você.**

Argumentos: $ARGUMENTS.

## Se for uma vaga nova

1. Crie `[proj]/vagas/AAAA-MM-DD-empresa/vaga.md` com link, data, fonte, família, modelo, regime e o **texto completo** do anúncio (anúncios somem).
2. Escolha o currículo pela tabela "Currículos" do CLAUDE.md, conforme o canal (portal/ATS ou direto para uma pessoa), e o idioma da vaga.
3. Escreva `[proj]/vagas/AAAA-MM-DD-empresa/cobertura.md` cruzando a vaga com o currículo escolhido (texto em `curriculo_fonte`, se houver; senão, peça o arquivo) e com o perfil-verdade:
   1. fontes usadas (currículo, idioma e, se houver repositório, o commit atual);
   2. cobertura forte: requisito → trecho do currículo → trecho do perfil;
   3. cobertura parcial;
   4. lacunas, cada uma pelo nome;
   5. inconsistências: data, cargo ou número que não bata com o perfil;
   6. decisão, que é do usuário: enviar como está, ajustar o currículo (só com fato verdadeiro do perfil que ficou de fora) ou desistir.

   Nunca sugira acrescentar o que é lacuna.
4. Se houver formulário com perguntas abertas, rascunhe as respostas a partir do perfil-verdade. Pretensão salarial: proponha uma estratégia e deixe a decisão com o usuário.
5. Passe pelo checklist de golpes do CLAUDE.md.
6. Se o usuário decidir ajustar o currículo, há dois caminhos. Nos dois, corrija primeiro o perfil-verdade se o fato não estiver lá.
   - **Ajuste na base** (vale para todas as vagas daqui em diante): edite `curriculo_fonte`, rode `curriculo_compilar` dentro de `curriculo_repo` e faça commit lá (e push, se houver remoto).
   - **Versão da vaga** (quando o usuário pedir um currículo para esta vaga, ou quando a vaga pedir um foco que não serve às outras): crie `[proj]/vagas/AAAA-MM-DD-empresa/curriculo.<ext>`, com a mesma extensão de `curriculo_fonte`.
     - O arquivo traz só o que muda em relação à base (título, resumo, ordem e corte dos itens, projetos, competências), se o compilador aceitar sobreposição; senão, uma cópia inteira. No topo, um comentário com a vaga, a data e a regra: "só fatos do perfil-verdade; muda ordem, foco e corte, não acrescenta fato".
     - Mude a ênfase, não os fatos: cargo oficial, datas e números iguais ao perfil. Papel que não é o cargo oficial vai entre parênteses e é apontado para o usuário aprovar.
     - Rode `curriculo_compilar_vaga` dentro de `curriculo_repo`, com `{arquivo}` e `{pasta}` substituídos. Os PDFs ficam na pasta da vaga, ao lado do arquivo. Sem `curriculo_compilar_vaga`, deixe o arquivo pronto e diga ao usuário que falta gerar o PDF.
     - Leia o PDF gerado (número de páginas, item cortado, quebra feia) e registre em `cobertura.md` uma tabela "o que mudou em relação à base" e a lista do que o usuário precisa aprovar.
7. Pare aqui.

## Se o argumento for "enviei <empresa>"

1. Se houver `curriculo_repo` com git: crie a tag anotada `envio-AAAA-MM-DD-empresa` no commit atual (mensagem: empresa, currículo e idioma) e faça push da tag, se houver remoto. Sem repositório: anote o nome do arquivo enviado.
   - Se a pasta da vaga tiver uma **versão da vaga** (`curriculo.<ext>` e os PDFs), confirme com o usuário qual PDF foi enviado. Esse arquivo, commitado junto com `[proj]`, é o registro do que a empresa recebeu; daí em diante a versão da vaga não muda.
2. Acrescente ou atualize a linha da vaga em `tracker.csv`, com `curriculo` = `currículo-idioma · tag` (ou o arquivo), status `enviada` e o próximo passo. Com versão da vaga: `vaga-<modelo>-<idioma> · tag` (ex.: `vaga-sobrio-en · envio-2026-11-09-ueni`).

## Gravar

Se `commit` for true e `[proj]` estiver num repositório git: commit só dos arquivos de `[proj]`, com a mensagem de `mensagem_commit` (AAAA-MM-DD = hoje), e push se houver remoto. Não inclua outras mudanças do repositório.

## Relatar

O resumo da cobertura (fortes, parciais, lacunas, inconsistências) e o que o usuário precisa decidir; ou, depois do envio, a versão registrada e a linha do tracker.
