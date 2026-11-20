# ms-v7 â€” Social Autopilot

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

Um arquivo de voz de marca + 3 prompts â†’ posts nativos por rede, na sua voz, agendados pelo provedor que vocÃª escolher. **Nada vai ao ar sem o seu "sim".**

Adaptado de *The Social Autopilot* (Zubair Trabzada, AI Workshop). Os originais (prompt pack, template de voz, transcriÃ§Ã£o do vÃ­deo) estÃ£o em `docs/`. O plano de implementaÃ§Ã£o estÃ¡ em `docs/PLANO.md`.

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/ms-v7/guia/**

---

## 1. Como funciona (em 1 minuto)

```
brand-voice.md  â”€â”
config.yaml     â”€â”¼â”€â–º  /ms-post  â”€â”€â–º 3 perguntas â”€â”€â–º 3 opÃ§Ãµes â”€â”€â–º visual â”€â”€â–º "Agendo? (sim/rascunho/nÃ£o)"
provedores      â”€â”˜        â”‚
                          â”œâ”€â–º /ms-semana       retro + tabela de 5-7 posts + fila da semana
                          â””â”€â–º /ms-reaproveita  transcriÃ§Ã£o / artigo / URL â†’ um post por rede
```

- **Voz de marca** (`brands/<slug>/brand-voice.md`): quem vocÃª Ã©, pÃºblico, ofertas, frases, histÃ³rias verdadeiras, regras por rede. Ã‰ o que faz o post soar como vocÃª e nÃ£o como IA.
- **Config** (`config.yaml`): qual marca estÃ¡ ativa, quais redes recebem posts, quem publica (Metricool, Blotato ou copiar Ã  mÃ£o), quem gera imagem/vÃ­deo.
- **Skills** (`.claude/skills/`): os prompts do pack empacotados como comandos do Claude Code.
- **Pipelines** (`pipeline/`): receitas passo a passo de cada provedor.

---

## 2. Start (prÃ©-requisitos)

| O quÃª | Como | ObrigatÃ³rio? |
|---|---|---|
| Claude Code | `claude.com/claude-code` â€” plano Pro ou superior | sim |
| Este repo | `git clone git@github.com:inematds/ms-v7.git ~/projetos/ms-v7` | sim |
| Python 3 + PyYAML | `pip install pyyaml` (sÃ³ para validar o `config.yaml`) | recomendado |
| Provedor de publicaÃ§Ã£o | Metricool (MCP, grÃ¡tis) ou Blotato (MCP, pago) â€” ver Â§4 | nÃ£o (sem ele, modo `copiar`) |
| Gerador de imagem | flux2-klein local (padrÃ£o) ou Magnific (MCP) | nÃ£o |
| Host de mÃ­dia | `gh` autenticado na conta `inematds` (release em `inematds/midia`) | sÃ³ com provedor MCP |

Abra o Claude Code **dentro da pasta**:

```bash
cd ~/projetos/ms-v7
claude
```

Ele lÃª `CLAUDE.md` automaticamente e passa a conhecer as regras e os comandos.

---

## 3. Configurar a primeira marca

No Claude Code:

```
/ms-config
```

O comando pergunta **uma coisa por vez**, em texto livre. Ele faz trÃªs blocos:

**A. Voz de marca** â†’ cria `brands/<slug>/brand-voice.md` a partir de `brands/_template/`.
- Se a marca jÃ¡ estÃ¡ descrita em algum lugar (uma skill, um site, um doc), diga onde: ele prÃ©-preenche e sÃ³ pergunta o que falta.
- O que ele sempre pergunta porque ninguÃ©m pode inventar: 5 frases que vocÃª realmente fala, 3 histÃ³rias verdadeiras com nÃºmero, prova verificÃ¡vel, CTAs exatos.
- Respostas vagas sÃ£o recusadas ("muitos alunos" â†’ "quantos?").
- Termina com um **teste de voz**: ele escreve uma legenda sÃ³ com o arquivo e pergunta "vocÃª escreveria isso?".

**B. Redes e provedores** â†’ preenche `config.yaml`.
- Para cada rede: handle e se estÃ¡ `ativa`. SÃ³ redes ativas recebem posts.
- Provedor de publicaÃ§Ã£o: `metricool` Â· `blotato` Â· `copiar`.
- Imagem: `flux2-klein` (padrÃ£o) ou `magnific`. VÃ­deo: `nenhum` atÃ© precisar.

**C. Ajustes depois**: `"ativa o LinkedIn"`, `"troca pra blotato"`, `"usa a marca X"` â€” ele edita sÃ³ a chave pedida.

TambÃ©m dÃ¡ pra editar `config.yaml` Ã  mÃ£o. Validar:

```bash
python3 -c "import yaml; yaml.safe_load(open('config.yaml'))" && echo ok
```

### VÃ¡rias marcas

Cada marca Ã© uma pasta em `brands/`. SÃ³ uma Ã© a ativa (`marca_ativa` no `config.yaml`). Para trocar: `"usa a marca X"` ou edite a chave.

---

## 4. Conectar um provedor de publicaÃ§Ã£o

### Metricool (validado, plano Free)

```bash
claude mcp add --transport http metricool https://ai.metricool.com/mcp -s user
```

Dentro do Claude Code: `/mcp` â†’ **metricool** â†’ **Authenticate** â†’ autorizar no navegador. Depois, no `/ms-config`, informe o `blogId` (ele descobre com `getBrandSettings`).

Limites do Free: 1 marca, 1 conta por rede, **sem LinkedIn nem X**, **20 posts/mÃªs**, analytics de 30 dias, sem "apagar post" pelo MCP (cancela no app).

### Blotato (nÃ£o testado aqui)

Criar conta â†’ conectar redes â†’ copiar a URL do MCP â†’ `claude mcp add --transport http blotato <url> -s user`. Antes de usar em produÃ§Ã£o, faÃ§a um post em `rascunho` e anote em `pipeline/publicar.md` o payload que funcionou.

### Copiar (sem API)

Nada a conectar. As skills entregam o texto pronto em bloco de cÃ³digo e vocÃª cola na rede. Ã‰ o padrÃ£o enquanto nenhum provedor estiver configurado.

### Host de mÃ­dia

Provedores MCP sÃ³ aceitam **URL direta** de imagem/vÃ­deo. O padrÃ£o Ã© um GitHub Release:

```bash
gh release upload v1 arquivo.png --repo inematds/midia --clobber
# â†’ https://github.com/inematds/midia/releases/download/v1/arquivo.png
```

As skills fazem isso sozinhas; sÃ³ precisa do `gh` logado na conta `inematds`.

---

## 5. Uso diÃ¡rio

| Comando | O que acontece | Tempo |
|---|---|---|
| `/ms-post` | 3 perguntas (rede, tema, objetivo) â†’ pesquisa opcional â†’ 3 opÃ§Ãµes â†’ vocÃª escolhe â†’ visual â†’ "Agendo?" | 3-5 min |
| `/ms-semana` | Retro da semana passada (analytics) â†’ 3 perguntas â†’ tabela de 5-7 posts â†’ vocÃª edita/aprova â†’ escreve tudo â†’ visuais â†’ "Agendo a fila?" | 15 min |
| `/ms-reaproveita` | Cola transcriÃ§Ã£o/artigo/URL â†’ "quais redes?" â†’ um post autÃ´nomo por rede â†’ fila | 5 min |

**O gate de aprovaÃ§Ã£o** aceita trÃªs respostas:

- `sim` â†’ agenda com publicaÃ§Ã£o automÃ¡tica no horÃ¡rio proposto.
- `rascunho` â†’ cria como rascunho no provedor (nÃ£o publica).
- `nÃ£o` â†’ sÃ³ o texto, pra copiar.

Cada post fica em `posts/<data>-<slug>/post.md` com status, `id`/`uuid` do provedor e o visual aprovado. Semanas ficam em `posts/semanas/`.

---

## 6. Publicar no git

O trabalho **termina no push**. Deploy Ã© responsabilidade do webhook, nÃ£o sua.

```bash
cd ~/projetos/ms-v7
git config user.email   # tem que ser inematds@gmail.com
git add -A
git commit -m "post: 2026-09-15-ler-documentacao (instagram, agendado)"
git push
```

Regras:
- Repo: `inematds/ms-v7`. Autor e committer: `inematds <inematds@gmail.com>`. Se `git config user.email` divergir, corrija **localmente** (`git config user.email inematds@gmail.com`), sem tocar a config global.
- As skills jÃ¡ commitam ao final de cada post/semana. VocÃª sÃ³ dÃ¡ o `git push`.
- VÃ­deos e imagens fora de `posts/` sÃ£o ignorados pelo `.gitignore`. MÃ­dia pÃºblica vai em release no repo `inematds/midia`, nunca no histÃ³rico deste.
- VersÃ£o em `VERSION` e no topo de `CLAUDE.md` (`vX.XX.YY`: patch incrementa `YY`; feature incrementa `XX` e carrega `YY`; sÃ³ major zera).
- Falha real â†’ uma linha em `FALHAS.md` (`| data | o que quebrou | menor correÃ§Ã£o | prompt \| infra |`).

---

## 7. Publicar no portal (inema.club)

O portal aponta para uma **pÃ¡gina landing + guia** servida pelo GitHub Pages **deste mesmo repo** (nunca um repo separado).

**Passo 1 â€” criar o guia** (jÃ¡ existe em `guia/index.html`; regerar se mudar o projeto):

```
/projetos-landing-guia
```

Gera `guia/index.html` (self-contained, padrÃ£o INEMA dark Ã¢mbar) com `guia/assets/` ao lado.

**Passo 2 â€” ativar o GitHub Pages via GitHub Actions** (o workflow `.github/workflows/pages.yml` jÃ¡ estÃ¡ no repo; nÃ£o usar o build "legacy" por branch, que trava):

```bash
gh repo edit inematds/ms-v7 --visibility public --accept-visibility-change-consequences   # Pages exige repo pÃºblico no plano free
gh api -X POST repos/inematds/ms-v7/pages -f build_type=workflow \
  || gh api -X PUT repos/inematds/ms-v7/pages -f build_type=workflow
git add guia && git commit -m "docs: guia landing" && git push   # o push dispara o deploy
gh run list --repo inematds/ms-v7 --limit 3                       # acompanhar
```

> Commits que tocam `.github/workflows/` precisam ir por SSH (`git@github.com:inematds/ms-v7.git`): o token HTTPS da conta `inematds` nÃ£o tem o escopo `workflow`. O remote deste repo jÃ¡ Ã© SSH.

URL resultante: `https://inematds.github.io/ms-v7/guia/`. Conferir com `curl -sI <url> | head -1` (tem que responder 200).

> Antes de tornar pÃºblico: `brands/` contÃ©m sua voz de marca e histÃ³rias pessoais, e `posts/` contÃ©m o histÃ³rico. Se nÃ£o quiser isso pÃºblico, mova o guia para um branch `gh-pages` sÃ³ com `guia/`, ou mantenha o repo privado e hospede o guia em outro lugar.

**Passo 3 â€” colocar no portal:**

```
/atualiza-portal https://inematds.github.io/ms-v7/guia/
```

A skill cria o card nas 3 superfÃ­cies INEMA (portal `inema.club`, inemabuscas e catÃ¡logo PRO), commita e faz push nos 3 repos. O deploy no Vercel Ã© automÃ¡tico pelo webhook; nÃ£o precisa (nem deve) checar o dashboard.

---

## 8. Mapa do repo

```
ms-v7/
â”œâ”€â”€ CLAUDE.md                  regras fixas + comandos (o Claude Code lÃª sozinho)
â”œâ”€â”€ README.md                  este arquivo
â”œâ”€â”€ VERSION                    1.0.0
â”œâ”€â”€ FALHAS.md                  changelog de falhas, uma linha cada
â”œâ”€â”€ config.yaml                marca ativa Â· redes Â· provedores Â· regras
â”œâ”€â”€ brands/
â”‚   â”œâ”€â”€ _template/brand-voice.md   template de voz (PT-BR, todas as redes)
â”‚   â””â”€â”€ <slug>/brand-voice.md      uma pasta por marca
â”œâ”€â”€ pipeline/
â”‚   â”œâ”€â”€ publicar.md            Metricool (validado) Â· Blotato Â· copiar Â· hospedagem
â”‚   â””â”€â”€ visual.md              formatos por rede Â· flux2-klein Â· Magnific Â· vÃ­deo
â”œâ”€â”€ posts/
â”‚   â”œâ”€â”€ <AAAA-MM-DD>-<slug>/post.md   cada post, com status e id/uuid
â”‚   â””â”€â”€ semanas/<AAAA-WW>.md          plano + retro de cada semana
â”œâ”€â”€ .claude/skills/
â”‚   â”œâ”€â”€ ms-config/   ms-post/   ms-semana/   ms-reaproveita/
â”œâ”€â”€ guia/                      landing + guia (GitHub Pages) â†’ inematds.github.io/ms-v7/guia/
â””â”€â”€ docs/                      originais do pack + PLANO.md
```

---

## 9. Problemas comuns

| Sintoma | Causa provÃ¡vel | Fix |
|---|---|---|
| "nenhuma marca configurada" | `marca_ativa: null` | `/ms-config` |
| Skill entrega sÃ³ texto, nÃ£o agenda | provedor = `copiar` ou MCP desconectado | `/mcp` â†’ Authenticate, ou `/ms-config` "troca pra metricool" |
| `createScheduledPost` recusa a mÃ­dia | URL nÃ£o Ã© arquivo direto ou nÃ£o responde 200 | `curl -sI <url>`; refazer o `gh release upload` |
| Post duplicou no YouTube | rede `youtube` ativa numa marca cujo canal Ã© origem de outro pipeline | `ativa: false` para youtube no `config.yaml` |
| Analytics vazio na retro | janela de 30 dias do Free, ou conector errado | ver `pipeline/publicar.md` Â§ Analytics |
| Push recusado por autor | `user.email` errado | `git config user.email inematds@gmail.com` e commit vazio |

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>