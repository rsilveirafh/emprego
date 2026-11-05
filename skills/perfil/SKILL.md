---
name: perfil
description: Monta e mantém o perfil-verdade da busca de emprego. Na primeira vez, monta a partir do currículo, LinkedIn ou entrevista; depois revisa os [confirmar] e registra fatos novos ou corrigidos, lacunas, critérios e exclusões, levando ao currículo o que mudou.
argument-hint: "(vazio = montar ou revisar) | [fato novo ou correção] | [lacuna: ...] | [não quero: ...] | [critério: ...]"
disable-model-invocation: true
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(git *)
---

## Configuração

Leia `~/.claude/emprego-config.json`. Campos:
- `projeto`: pasta da busca (abaixo, `[proj]`);
- `curriculo_repo`, `curriculo_fonte`, `curriculo_compilar`: repositório do currículo, arquivo com o texto dele e comando que gera os PDFs (opcionais);
- `commit` e `mensagem_commit`: se e como commitar ao final.

Se o arquivo não existir, explique a instalação (copiar `projeto-template/` do plugin para a pasta da busca e criar a config, como no README) e pare.

## Antes de tudo

Leia `[proj]/CLAUDE.md` (regras de ouro e critérios), `[proj]/perfil-verdade.md` (inclusive a legenda das marcas e as "Regras de comunicação") e `[proj]/exclusoes.md`.

O perfil-verdade é a fonte única dos fatos da carreira: tudo o que os outros comandos `/emprego:*` dizem sobre o usuário sai dele. Por isso:
- **Só entra o que o usuário disser ou confirmar.** Não complete, não arredonde para cima, não troque "ajudei" por "liderei". Se a frase for vaga, pergunte.
- **Todo número tem origem:** `[medido: de onde veio]`, `[estimado: como estimou]` ou `[sem número]`. Sem origem, pergunte antes de gravar.
- **Segredos nunca entram:** senha, CPF/RG, dados bancários. Nada de dado de cliente nem informação confidencial de empregador.

Argumentos: $ARGUMENTS.

## Modo 0: primeira vez (o perfil ainda é o modelo)

O perfil é modelo quando tem o comentário `modelo:` no topo ou as seções estão vazias.

1. Pergunte de onde tirar os fatos: currículo (PDF ou o `curriculo_fonte` da config), exportação do LinkedIn, Lattes, portfólio, ou só conversa.
2. Leia as fontes e monte um rascunho em todas as seções. **Tudo o que veio de fonte entra com `[confirmar]`**, e números sem origem clara entram como `[confirmar: origem]`.
3. Em "Lacunas conhecidas", liste o que as vagas das famílias do CLAUDE.md costumam pedir e não aparece em nenhuma fonte.
4. Remova o comentário `modelo:` e preencha a lista de fontes no topo.
5. Pergunte também os critérios (famílias, modelo, regime, piso) e os currículos que existem, e preencha as tabelas do `[proj]/CLAUDE.md`.
6. Siga para o Modo 1.

## Modo 1: sem argumentos (revisão)

1. Liste todos os itens com `[confirmar]` no perfil-verdade e em `exclusoes.md`, agrupados por seção, com a contagem.
2. Pergunte **no máximo 3 de cada vez**, começando pelos que mais pesam numa vaga (experiência e números antes de competências). Para cada um, mostre o texto atual e diga o que falta (data, número, origem, sim ou não).
3. A cada resposta: corrija o texto, troque `[confirmar]` pela marca certa ou apague o item se o usuário disser que não é verdade. "Não sei" ou "depois" mantém o `[confirmar]`.
4. Pare quando o usuário quiser; o que ficou continua marcado para a próxima revisão.

## Modo 2: fato novo ou correção

Ex.: "na empresa X eu também fazia relatórios em Power BI", "o mestrado terminou em 2012, não 2011".

1. Diga em que seção vai entrar (ou que trecho muda) e pergunte o que faltar: quando, onde, o que exatamente foi feito, para quem, com que ferramenta, e o número com a origem.
2. Mostre o antes e o depois e grave só com o ok do usuário. Fato que não se encaixa em seção nenhuma vai para "Adendos".
3. Veja se o fato afeta outros lugares e proponha, sem aplicar sozinho:
   - **currículo** (`curriculo_fonte`, se houver): o texto novo em cada idioma usado. Com o ok, edite, rode `curriculo_compilar` dentro de `curriculo_repo` e faça commit lá (e push, se houver remoto);
   - **"Lacunas conhecidas"**: se o fato fecha uma lacuna, tire-a de lá;
   - LinkedIn, portfólio ou site: só avise.

## Modo 3: "lacuna: ..."

Registre na seção "Lacunas conhecidas", com a resposta honesta para entrevista ("não fiz X; o mais perto foi Y; estou estudando Z") se o usuário souber o Y e o Z. Lacuna nunca vira competência.

## Modo 4: "não quero: ..." ou "critério: ..."

- Empresa ou tipo de vaga que não quer: acrescente em `exclusoes.md` com motivo e data. O que entra lá não volta.
- Mudança de critério (família, modelo, regime, piso): atualize as tabelas "Critérios" ou "Famílias de vaga" do `[proj]/CLAUDE.md`, com a data da decisão na seção "Estado".

## Gravar

Se `commit` for true e `[proj]` estiver num repositório git: commit só dos arquivos de `[proj]`, com a mensagem de `mensagem_commit` (AAAA-MM-DD = hoje; resumo começando por "perfil-verdade"), e push se houver remoto. Não inclua outras mudanças do repositório.

## Relatar

O que mudou, quantos `[confirmar]` ainda restam, e o que foi (ou deveria ser) levado ao currículo.
