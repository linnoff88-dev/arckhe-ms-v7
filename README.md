ï»¿# ms-v7 ê¢â¬â Social Autopilot

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

Um arquivo de voz de marca + 3 prompts ê¢â â posts nativos por rede, na sua voz, agendados pelo provedor que vocêÂª escolher. **Nada vai ao ar sem o seu "sim".**

Adaptado de *The Social Autopilot* (Zubair Trabzada, AI Workshop). Os originais (prompt pack, template de voz, transcriêÂ§êÂ£o do vêÂ­deo) estêÂ£o em `docs/`. O plano de implementaêÂ§êÂ£o estêÂ¡ em `docs/PLANO.md`.

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/ms-v7/guia/**

---

## 1. Como funciona (em 1 minuto)

```
brand-voice.md  ê¢ââ¬ê¢âÂ
config.yaml     ê¢ââ¬ê¢âÂ¼ê¢ââ¬ê¢âÂº  /ms-post  ê¢ââ¬ê¢ââ¬ê¢âÂº 3 perguntas ê¢ââ¬ê¢ââ¬ê¢âÂº 3 opêÂ§êÂµes ê¢ââ¬ê¢ââ¬ê¢âÂº visual ê¢ââ¬ê¢ââ¬ê¢âÂº "Agendo? (sim/rascunho/nêÂ£o)"
provedores      ê¢ââ¬ê¢âË        ê¢ââ
                          ê¢âÅê¢ââ¬ê¢âÂº /ms-semana       retro + tabela de 5-7 posts + fila da semana
                          ê¢ââê¢ââ¬ê¢âÂº /ms-reaproveita  transcriêÂ§êÂ£o / artigo / URL ê¢â â um post por rede
```

- **Voz de marca** (`brands/<slug>/brand-voice.md`): quem vocêÂª êÂ©, pêÂºblico, ofertas, frases, histêÂ³rias verdadeiras, regras por rede. êâ° o que faz o post soar como vocêÂª e nêÂ£o como IA.
- **Config** (`config.yaml`): qual marca estêÂ¡ ativa, quais redes recebem posts, quem publica (Metricool, Blotato ou copiar êÂ  mêÂ£o), quem gera imagem/vêÂ­deo.
- **Skills** (`.claude/skills/`): os prompts do pack empacotados como comandos do Claude Code.
- **Pipelines** (`pipeline/`): receitas passo a passo de cada provedor.

---

## 2. Start (prêÂ©-requisitos)

| O quêÂª | Como | ObrigatêÂ³rio? |
|---|---|---|
| Claude Code | `claude.com/claude-code` ê¢â¬â plano Pro ou superior | sim |
| Este repo | `git clone git@github.com:inematds/ms-v7.git ~/projetos/ms-v7` | sim |
| Python 3 + PyYAML | `pip install pyyaml` (sêÂ³ para validar o `config.yaml`) | recomendado |
| Provedor de publicaêÂ§êÂ£o | Metricool (MCP, grêÂ¡tis) ou Blotato (MCP, pago) ê¢â¬â ver êÂ§4 | nêÂ£o (sem ele, modo `copiar`) |
| Gerador de imagem | flux2-klein local (padrêÂ£o) ou Magnific (MCP) | nêÂ£o |
| Host de mêÂ­dia | `gh` autenticado na conta `inematds` (release em `inematds/midia`) | sêÂ³ com provedor MCP |

Abra o Claude Code **dentro da pasta**:

```bash
cd ~/projetos/ms-v7
claude
```

Ele lêÂª `CLAUDE.md` automaticamente e passa a conhecer as regras e os comandos.

---

## 3. Configurar a primeira marca

No Claude Code:

```
/ms-config
```

O comando pergunta **uma coisa por vez**, em texto livre. Ele faz trêÂªs blocos:

**A. Voz de marca** ê¢â â cria `brands/<slug>/brand-voice.md` a partir de `brands/_template/`.
- Se a marca jêÂ¡ estêÂ¡ descrita em algum lugar (uma skill, um site, um doc), diga onde: ele prêÂ©-preenche e sêÂ³ pergunta o que falta.
- O que ele sempre pergunta porque ninguêÂ©m pode inventar: 5 frases que vocêÂª realmente fala, 3 histêÂ³rias verdadeiras com nêÂºmero, prova verificêÂ¡vel, CTAs exatos.
- Respostas vagas sêÂ£o recusadas ("muitos alunos" ê¢â â "quantos?").
- Termina com um **teste de voz**: ele escreve uma legenda sêÂ³ com o arquivo e pergunta "vocêÂª escreveria isso?".

**B. Redes e provedores** ê¢â â preenche `config.yaml`.
- Para cada rede: handle e se estêÂ¡ `ativa`. SêÂ³ redes ativas recebem posts.
- Provedor de publicaêÂ§êÂ£o: `metricool` êÂ· `blotato` êÂ· `copiar`.
- Imagem: `flux2-klein` (padrêÂ£o) ou `magnific`. VêÂ­deo: `nenhum` atêÂ© precisar.

**C. Ajustes depois**: `"ativa o LinkedIn"`, `"troca pra blotato"`, `"usa a marca X"` ê¢â¬â ele edita sêÂ³ a chave pedida.

TambêÂ©m dêÂ¡ pra editar `config.yaml` êÂ  mêÂ£o. Validar:

```bash
python3 -c "import yaml; yaml.safe_load(open('config.yaml'))" && echo ok
```

### VêÂ¡rias marcas

Cada marca êÂ© uma pasta em `brands/`. SêÂ³ uma êÂ© a ativa (`marca_ativa` no `config.yaml`). Para trocar: `"usa a marca X"` ou edite a chave.

---

## 4. Conectar um provedor de publicaêÂ§êÂ£o

### Metricool (validado, plano Free)

```bash
claude mcp add --transport http metricool https://ai.metricool.com/mcp -s user
```

Dentro do Claude Code: `/mcp` ê¢â â **metricool** ê¢â â **Authenticate** ê¢â â autorizar no navegador. Depois, no `/ms-config`, informe o `blogId` (ele descobre com `getBrandSettings`).

Limites do Free: 1 marca, 1 conta por rede, **sem LinkedIn nem X**, **20 posts/mêÂªs**, analytics de 30 dias, sem "apagar post" pelo MCP (cancela no app).

### Blotato (nêÂ£o testado aqui)

Criar conta ê¢â â conectar redes ê¢â â copiar a URL do MCP ê¢â â `claude mcp add --transport http blotato <url> -s user`. Antes de usar em produêÂ§êÂ£o, faêÂ§a um post em `rascunho` e anote em `pipeline/publicar.md` o payload que funcionou.

### Copiar (sem API)

Nada a conectar. As skills entregam o texto pronto em bloco de cêÂ³digo e vocêÂª cola na rede. êâ° o padrêÂ£o enquanto nenhum provedor estiver configurado.

### Host de mêÂ­dia

Provedores MCP sêÂ³ aceitam **URL direta** de imagem/vêÂ­deo. O padrêÂ£o êÂ© um GitHub Release:

```bash
gh release upload v1 arquivo.png --repo inematds/midia --clobber
# ê¢â â https://github.com/inematds/midia/releases/download/v1/arquivo.png
```

As skills fazem isso sozinhas; sêÂ³ precisa do `gh` logado na conta `inematds`.

---

## 5. Uso diêÂ¡rio

| Comando | O que acontece | Tempo |
|---|---|---|
| `/ms-post` | 3 perguntas (rede, tema, objetivo) ê¢â â pesquisa opcional ê¢â â 3 opêÂ§êÂµes ê¢â â vocêÂª escolhe ê¢â â visual ê¢â â "Agendo?" | 3-5 min |
| `/ms-semana` | Retro da semana passada (analytics) ê¢â â 3 perguntas ê¢â â tabela de 5-7 posts ê¢â â vocêÂª edita/aprova ê¢â â escreve tudo ê¢â â visuais ê¢â â "Agendo a fila?" | 15 min |
| `/ms-reaproveita` | Cola transcriêÂ§êÂ£o/artigo/URL ê¢â â "quais redes?" ê¢â â um post autêÂ´nomo por rede ê¢â â fila | 5 min |

**O gate de aprovaêÂ§êÂ£o** aceita trêÂªs respostas:

- `sim` ê¢â â agenda com publicaêÂ§êÂ£o automêÂ¡tica no horêÂ¡rio proposto.
- `rascunho` ê¢â â cria como rascunho no provedor (nêÂ£o publica).
- `nêÂ£o` ê¢â â sêÂ³ o texto, pra copiar.

Cada post fica em `posts/<data>-<slug>/post.md` com status, `id`/`uuid` do provedor e o visual aprovado. Semanas ficam em `posts/semanas/`.

---

## 6. Publicar no git

O trabalho **termina no push**. Deploy êÂ© responsabilidade do webhook, nêÂ£o sua.

```bash
cd ~/projetos/ms-v7
git config user.email   # tem que ser inematds@gmail.com
git add -A
git commit -m "post: 2026-09-15-ler-documentacao (instagram, agendado)"
git push
```

Regras:
- Repo: `inematds/ms-v7`. Autor e committer: `inematds <inematds@gmail.com>`. Se `git config user.email` divergir, corrija **localmente** (`git config user.email inematds@gmail.com`), sem tocar a config global.
- As skills jêÂ¡ commitam ao final de cada post/semana. VocêÂª sêÂ³ dêÂ¡ o `git push`.
- VêÂ­deos e imagens fora de `posts/` sêÂ£o ignorados pelo `.gitignore`. MêÂ­dia pêÂºblica vai em release no repo `inematds/midia`, nunca no histêÂ³rico deste.
- VersêÂ£o em `VERSION` e no topo de `CLAUDE.md` (`vX.XX.YY`: patch incrementa `YY`; feature incrementa `XX` e carrega `YY`; sêÂ³ major zera).
- Falha real ê¢â â uma linha em `FALHAS.md` (`| data | o que quebrou | menor correêÂ§êÂ£o | prompt \| infra |`).

---

## 7. Publicar no portal (inema.club)

O portal aponta para uma **pêÂ¡gina landing + guia** servida pelo GitHub Pages **deste mesmo repo** (nunca um repo separado).

**Passo 1 ê¢â¬â criar o guia** (jêÂ¡ existe em `guia/index.html`; regerar se mudar o projeto):

```
/projetos-landing-guia
```

Gera `guia/index.html` (self-contained, padrêÂ£o INEMA dark êÂ¢mbar) com `guia/assets/` ao lado.

**Passo 2 ê¢â¬â ativar o GitHub Pages via GitHub Actions** (o workflow `.github/workflows/pages.yml` jêÂ¡ estêÂ¡ no repo; nêÂ£o usar o build "legacy" por branch, que trava):

```bash
gh repo edit inematds/ms-v7 --visibility public --accept-visibility-change-consequences   # Pages exige repo pêÂºblico no plano free
gh api -X POST repos/inematds/ms-v7/pages -f build_type=workflow \
  || gh api -X PUT repos/inematds/ms-v7/pages -f build_type=workflow
git add guia && git commit -m "docs: guia landing" && git push   # o push dispara o deploy
gh run list --repo inematds/ms-v7 --limit 3                       # acompanhar
```

> Commits que tocam `.github/workflows/` precisam ir por SSH (`git@github.com:inematds/ms-v7.git`): o token HTTPS da conta `inematds` nêÂ£o tem o escopo `workflow`. O remote deste repo jêÂ¡ êÂ© SSH.

URL resultante: `https://inematds.github.io/ms-v7/guia/`. Conferir com `curl -sI <url> | head -1` (tem que responder 200).

> Antes de tornar pêÂºblico: `brands/` contêÂ©m sua voz de marca e histêÂ³rias pessoais, e `posts/` contêÂ©m o histêÂ³rico. Se nêÂ£o quiser isso pêÂºblico, mova o guia para um branch `gh-pages` sêÂ³ com `guia/`, ou mantenha o repo privado e hospede o guia em outro lugar.

**Passo 3 ê¢â¬â colocar no portal:**

```
/atualiza-portal https://inematds.github.io/ms-v7/guia/
```

A skill cria o card nas 3 superfêÂ­cies INEMA (portal `inema.club`, inemabuscas e catêÂ¡logo PRO), commita e faz push nos 3 repos. O deploy no Vercel êÂ© automêÂ¡tico pelo webhook; nêÂ£o precisa (nem deve) checar o dashboard.

---

## 8. Mapa do repo

```
ms-v7/
ê¢âÅê¢ââ¬ê¢ââ¬ CLAUDE.md                  regras fixas + comandos (o Claude Code lêÂª sozinho)
ê¢âÅê¢ââ¬ê¢ââ¬ README.md                  este arquivo
ê¢âÅê¢ââ¬ê¢ââ¬ VERSION                    1.0.0
ê¢âÅê¢ââ¬ê¢ââ¬ FALHAS.md                  changelog de falhas, uma linha cada
ê¢âÅê¢ââ¬ê¢ââ¬ config.yaml                marca ativa êÂ· redes êÂ· provedores êÂ· regras
ê¢âÅê¢ââ¬ê¢ââ¬ brands/
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ _template/brand-voice.md   template de voz (PT-BR, todas as redes)
ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ <slug>/brand-voice.md      uma pasta por marca
ê¢âÅê¢ââ¬ê¢ââ¬ pipeline/
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ publicar.md            Metricool (validado) êÂ· Blotato êÂ· copiar êÂ· hospedagem
ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ visual.md              formatos por rede êÂ· flux2-klein êÂ· Magnific êÂ· vêÂ­deo
ê¢âÅê¢ââ¬ê¢ââ¬ posts/
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ <AAAA-MM-DD>-<slug>/post.md   cada post, com status e id/uuid
ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ semanas/<AAAA-WW>.md          plano + retro de cada semana
ê¢âÅê¢ââ¬ê¢ââ¬ .claude/skills/
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ ms-config/   ms-post/   ms-semana/   ms-reaproveita/
ê¢âÅê¢ââ¬ê¢ââ¬ guia/                      landing + guia (GitHub Pages) ê¢â â inematds.github.io/ms-v7/guia/
ê¢ââê¢ââ¬ê¢ââ¬ docs/                      originais do pack + PLANO.md
```

---

## 9. Problemas comuns

| Sintoma | Causa provêÂ¡vel | Fix |
|---|---|---|
| "nenhuma marca configurada" | `marca_ativa: null` | `/ms-config` |
| Skill entrega sêÂ³ texto, nêÂ£o agenda | provedor = `copiar` ou MCP desconectado | `/mcp` ê¢â â Authenticate, ou `/ms-config` "troca pra metricool" |
| `createScheduledPost` recusa a mêÂ­dia | URL nêÂ£o êÂ© arquivo direto ou nêÂ£o responde 200 | `curl -sI <url>`; refazer o `gh release upload` |
| Post duplicou no YouTube | rede `youtube` ativa numa marca cujo canal êÂ© origem de outro pipeline | `ativa: false` para youtube no `config.yaml` |
| Analytics vazio na retro | janela de 30 dias do Free, ou conector errado | ver `pipeline/publicar.md` êÂ§ Analytics |
| Push recusado por autor | `user.email` errado | `git config user.email inematds@gmail.com` e commit vazio |

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>