# Busca de emprego

Início: AAAA-MM-DD. Fim: quando eu aceitar uma proposta.

Manual da busca. Os comandos `/emprego:*` leem este arquivo, o [[perfil-verdade]], as [[exclusoes]]
e o [[tracker.csv]] antes de qualquer coisa. As instruções de cada etapa ficam nos próprios comandos;
aqui ficam as regras gerais e as minhas escolhas.

## Regras de ouro

1. **Uma fonte de verdade:** [[perfil-verdade]]. Fato que não está lá não vai para currículo,
   formulário nem entrevista. Mudou um fato? Corrige lá primeiro (`/emprego:perfil`), depois no currículo.
2. **Nunca inventar número.** Todo número tem origem marcada no perfil.
3. **A IA propõe, eu decido.** Leio tudo o que sai com o meu nome e eu mesmo envio as candidaturas.
4. **Lacuna declarada vale mais que maquiagem.** Requisito que não tenho fica escrito como lacuna.
5. **Exclusão é regra:** o que está em [[exclusoes]] não volta.
6. **Versão enviada não muda.** Cada envio fica registrado com a versão exata do currículo.
7. **Segredos fora do chat:** senha, CPF/RG e dados bancários nunca entram em prompt nem em formulário de candidatura.

## Critérios

<!-- Preencha. /emprego:perfil com "critério: ..." também atualiza esta tabela. -->

| Critério | Aceito |
|---|---|
| Famílias | ver tabela abaixo |
| Modelo | ex.: remoto, híbrido em [cidade], presencial, mudança de cidade |
| Regime | ex.: CLT, PJ, freela |
| Piso salarial | ex.: R$ X, ou "nenhum: decido caso a caso" |

## Famílias de vaga

<!-- Os tipos de vaga que você procura. A busca e a triagem usam a sigla. -->

| Família | Sigla | O que pesa na vaga |
|---|---|---|
| ex.: Dados / BI | `dados` | SQL, Power BI, estatística |
| ex.: Desenvolvimento | `dev` | linguagens, frameworks, produto |

## Currículos

<!-- Quais versões do currículo você tem e quando mandar cada uma. O /emprego:candidatar escolhe por esta tabela. -->

| Currículo | Arquivo | Quando mandar |
|---|---|---|
| ex.: uma coluna | `curriculo-pt.pdf` / `resume-en.pdf` | portal de vaga, formulário, ATS |
| ex.: visual | `curriculo-visual-pt.pdf` | direto para uma pessoa |

**Versões.** Se o currículo estiver num repositório git (`curriculo_repo` na config), cada envio
ganha uma tag `envio-AAAA-MM-DD-empresa`. Sem repositório, o tracker anota o arquivo e a data.

## Golpes: checklist antes de aceitar uma entrevista

- Pediram para **baixar e rodar código** ou um repositório "para achar um bug"? Recusar. Teste legítimo acontece no navegador, em plataforma conhecida.
- Pediram **CPF, RG ou foto de documento** na candidatura? Deixar em branco. Documento só na contratação, por canal oficial.
- Pediram **pagamento, taxa, curso ou "kit"**? É golpe.
- O **domínio do e-mail** é diferente do site da empresa? O contato veio só por WhatsApp ou Telegram? Há urgência exagerada? Confirmar pelo site oficial antes de responder.
- Proposta boa demais para o perfil, ou sem entrevista? Desconfiar.

## O ciclo

```
/emprego:perfil ─▶ /emprego:buscar ─▶ /emprego:triar ─▶ /emprego:candidatar ─▶ (eu envio)
                                                          └─▶ /emprego:candidatar enviei <empresa>
      ▲                                                                 │
      └──────── /emprego:retro (a cada 2 semanas) ◀── /emprego:entrevista ◀┘
```

Se o tempo apertar: pelo menos `/emprego:candidatar` (cobertura) e o registro no tracker.

## Estrutura

```
busca/
├── CLAUDE.md              este arquivo
├── perfil-verdade.md      fonte única dos fatos da carreira
├── exclusoes.md           empresas e tipos de vaga que não quero
├── tracker.csv            uma linha por candidatura ou contato
├── vagas/
│   ├── AAAA-MM-DD-busca.md         rodada de busca + triagem
│   └── AAAA-MM-DD-empresa/         vaga.md, cobertura.md, entrevista.md
└── retrospectivas/AAAA-MM-DD.md
```

## Estado

- AAAA-MM-DD: projeto criado.
