# emprego

Um plugin do [Claude Code](https://claude.com/claude-code) para buscar emprego com uma **fonte única de verdade**: um arquivo com os fatos da sua carreira, de onde sai tudo o que vai para currículo, formulário e entrevista. O agente busca, tria e prepara; você decide e você envia.

[English below](#english).

---

## A ideia

Com IA, é fácil gerar currículo e carta para cada vaga. Também é fácil, sem perceber, inflar um número, transformar "ajudei" em "liderei" ou listar uma ferramenta que você usou uma vez. Na entrevista, a conta chega.

Este plugin parte de outro lugar: **o `perfil-verdade.md`**. Ele guarda só o que você confirmou, com a origem de cada número (`[medido]`, `[estimado]` ou `[sem número]`), suas lacunas declaradas e o que nunca pode ir para currículo. Todos os comandos leem esse arquivo antes de trabalhar, e nenhum deles escreve sobre você o que não está lá.

```
            /emprego:perfil   (monta e mantém o perfil-verdade)
                   │
/emprego:buscar ─▶ /emprego:triar ─▶ /emprego:candidatar ─▶ você envia
                                            │
                       /emprego:candidatar enviei <empresa>  (registra a versão enviada)
                                            │
                   /emprego:entrevista ─▶ /emprego:retro  (a cada 2 semanas)
```

## Os comandos

| Comando | O que faz |
|---|---|
| `/emprego:perfil` | Na primeira vez, monta o perfil a partir do seu currículo, LinkedIn ou de uma conversa, com tudo marcado `[confirmar]`. Depois, revisa os `[confirmar]` (3 por vez), registra fato novo ou correção (`/emprego:perfil fiz X em Y`), lacuna (`lacuna: ...`), exclusão (`não quero: ...`) e critério (`critério: ...`), e propõe a mudança no currículo |
| `/emprego:buscar dados remoto` | Busca vagas dos últimos 14 dias para uma família, com link conferido, sem repetir rodadas anteriores nem o que está nas exclusões. Salva uma lista numerada |
| `/emprego:triar 3 5 8` | Aderência de cada vaga **com evidência tirada do perfil**: alta só quando todo requisito obrigatório tem prova. Lacunas pelo nome, alertas de golpe |
| `/emprego:candidatar 5` | Salva o anúncio inteiro, escolhe o currículo pelo canal, cruza requisito por requisito com o currículo e o perfil (cobertura forte, parcial, lacunas, inconsistências) e para. **Quem envia é você** |
| `/emprego:candidatar enviei Empresa` | Registra o envio no tracker com a versão exata do currículo (tag no git, se o currículo for um repositório) |
| `/emprego:entrevista Empresa` | Parte do currículo **que a empresa recebeu**: histórias STAR só com fatos do perfil, resposta honesta para cada lacuna, perguntas para a empresa e estratégia de salário |
| `/emprego:retro` | Números por fonte, família, modelo e currículo; comparação com a retrospectiva anterior; pelo menos uma decisão |

Os comandos não disparam sozinhos: só rodam quando você digita.

## O projeto da busca

A pasta `projeto-template/` é o esqueleto:

```
busca/
├── CLAUDE.md            regras de ouro, critérios, famílias de vaga, currículos, checklist de golpes
├── perfil-verdade.md    fonte única dos fatos da carreira
├── exclusoes.md         empresas e tipos de vaga que não voltam
├── tracker.csv          uma linha por candidatura ou contato
├── vagas/               rodadas de busca e uma pasta por candidatura
└── retrospectivas/
```

As regras gerais e as suas escolhas ficam no `CLAUDE.md` do projeto; as instruções de cada etapa ficam nos comandos. Mudou uma regra, muda num lugar só.

## Instalação

1. Instale pelo catálogo [rsilveirafh/claude-plugins](https://github.com/rsilveirafh/claude-plugins):

   ```bash
   claude plugin marketplace add rsilveirafh/claude-plugins
   claude plugin install emprego@rsilveirafh-plugins
   ```

   Para editar os prompts à vontade, copie cada `skills/<nome>/SKILL.md` para `~/.claude/commands/emprego/<nome>.md` em vez de instalar o plugin.

2. Copie `projeto-template/` para onde a sua busca vai morar. Pode ser uma pasta dentro do seu vault de notas. Se for um repositório git **privado**, os comandos commitam a cada passo.

3. Copie `config.example.json` para `~/.claude/emprego-config.json` e preencha:

   ```json
   {
     "projeto": "/caminho/absoluto/para/a/pasta/da/busca",
     "curriculo_repo": "",
     "curriculo_fonte": "",
     "curriculo_compilar": "",
     "commit": true,
     "mensagem_commit": "busca de emprego (AAAA-MM-DD): <resumo>"
   }
   ```

   Os campos `curriculo_*` são opcionais. Se o seu currículo é gerado a partir de um arquivo de texto num repositório git (YAML, Markdown, LaTeX, Typst...), preencha-os: o plugin passa a propor as mudanças no currículo e a marcar com tag cada versão enviada.

4. Rode `/emprego:perfil`.

## Segurança

- Senha, CPF/RG e dados bancários nunca entram em prompt nem em formulário de candidatura.
- O `CLAUDE.md` do projeto traz um checklist de golpes comuns em vagas de tecnologia: pedido para rodar um repositório "para achar um bug", documento na candidatura, taxa, domínio de e-mail diferente do da empresa.
- O perfil e o tracker têm dados pessoais: mantenha o projeto em repositório privado.

## Origem

O método é uma adaptação, para o Claude Code e para quem usa o currículo como código, da trilha manual do [meu-emprego-novo](https://github.com/R238Company/meu-emprego-novo) (R238, MIT), que por sua vez deriva de trabalho de Rushi Chaudhari (MIT). Os avisos de copyright estão em [LICENSE](LICENSE).

Uso este plugin na minha própria busca desde novembro de 2026.

## Licença

MIT.

---

## English

A [Claude Code](https://claude.com/claude-code) plugin for job hunting around a **single source of truth**: one file with the facts of your career, from which everything that goes into résumés, forms and interviews is drawn. The agent searches, screens and prepares; you decide and you apply.

**The commands and prompts are in Portuguese**, namespace included.

- `perfil-verdade.md` ("truth profile") holds only what you confirmed, with the origin of every number (`[medido]` measured, `[estimado]` estimated, `[sem número]` no number), declared gaps and what must never go on a résumé.
- `/emprego:perfil` builds it the first time (from your résumé, LinkedIn or a conversation, everything flagged `[confirmar]`) and maintains it afterwards.
- `/emprego:buscar` searches the last 14 days of postings; `/emprego:triar` screens them with evidence from the profile only; `/emprego:candidatar` runs a requirement-by-requirement coverage review and stops before sending; `/emprego:candidatar enviei <company>` logs the exact résumé version sent (git tag, if your résumé lives in a repo); `/emprego:entrevista` prepares STAR stories from the version the company actually received; `/emprego:retro` runs a fortnightly retrospective that must end in a decision.

Install: `claude plugin marketplace add rsilveirafh/claude-plugins`, then `claude plugin install emprego@rsilveirafh-plugins`. Copy `projeto-template/` somewhere private, copy `config.example.json` to `~/.claude/emprego-config.json`, and run `/emprego:perfil`.

Adapted from the manual track of [meu-emprego-novo](https://github.com/R238Company/meu-emprego-novo) (R238, MIT), itself derived from work by Rushi Chaudhari (MIT). MIT licensed.
