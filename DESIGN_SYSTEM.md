# Órbita — Design System (extraído do protótipo)

Fonte: `identidade-orbita/orbita-identidade.html` (marca) + `orbitawebsite.html`
(protótipo funcional: landing, funcionalidades, preços, back-office/sócios).
Todos os valores abaixo são retirados literalmente do CSS inline do protótipo,
não inventados. Onde o protótipo não cobre (ex: modais, toasts), a regra
indicada é extrapolada por analogia direta ao padrão existente — assinalado
com **(extrapolado)**.

## Cor

| Token | Valor | Uso |
|---|---|---|
| `papel` | `#EDEAE0` | fundo base de toda a app |
| `papel-fundo` | `#E2DED1` | faixas secundárias (bandas alternadas, cards em grelha tipo "o que entra em todos os planos") |
| `tinta` | `#101014` | texto principal, botão primário, ícones |
| `grafite` | `#6E6D64` | texto secundário, labels, placeholders |
| `fio` | `rgba(16,16,20,.16)` | toda borda 1px — cards, tabelas, inputs, divisores |
| `fio-forte` | `rgba(16,16,20,.28)` | borda tracejada (estados "isento"/"rascunho") |
| `latão` | `#D9B44A` (borda) / `#8A6F16` (texto) | estado de aviso (a expirar, aberto) |
| `accent` | `#C4442B` por omissão (Órbita) | **cor por tenant** — é a única cor que muda por empresa. Nunca papel/tinta/tipografia. |

Cores de tenant já definidas no protótipo: Órbita `#C4442B` (trânsito) ·
Clube de vinhos `#4C6B45` (vinha) · Associação náutica `#2B4C7E` (enseada) ·
Banda filarmónica `#7A3E6B` (palco). Implementar como CSS custom property
`--accent` no `:root` de cada tenant (lido de `Company` — ver nota de
implementação no fim).

`accent` é usado em: hover de botões (border+background/color passam de tinta
para accent), badges "em atraso"/"esgotado" (border+color = accent), marcador
"TENANT ATIVO" no hero.

## Tipografia

Fonte: **Archivo** (variável, eixo `wdth`), fallback `-apple-system, system-ui, sans-serif`.

| Papel | Tamanho | Peso | Letter-spacing | wdth | Transform |
|---|---|---|---|---|---|
| Display/H1 (hero, título de página) | `clamp(38px,3.6vw,58px)` a `96px` | 700 | `-0.045em` | 84–86 | lowercase |
| Título de secção (H2) | `27px` | 700 | `-0.03em` | 88 | normal |
| Nome/valor destacado (17px card) | `17px` | 700 | `-0.02em` | — | normal |
| Corpo | `17px` | 400 | normal | — | normal |
| Interface/tabela/botão | `13px` | 500 (700 quando é o valor principal de uma linha) | normal | — | normal |
| Etiqueta/eyebrow (labels, cabeçalhos de tabela, pills) | `11px` | 500 | `0.15em` | — | normal (mas visualmente lida como maiúscula pelo letter-spacing; o texto fonte já vem em minúsculas ou maiúsculas conforme o caso) |
| Número grande (stat/KPI) | `54px` | 700 | `-0.045em` | 84 | — |

## Espaçamento e grelha

- Sidebar do back-office: `grid-template-columns: minmax(200px,268px) minmax(0,1fr)`.
- Padding de secção: `88px` vertical em landing, `28px`/`32px` em back-office.
- Grelhas de cards (feature grid, pricing, stat-strip): `gap: 1px` com
  `background: var(--fio)` e `border: 1px solid var(--fio)` — o efeito de
  grelha vem do fundo aparecer nos gaps, não de `gap` com cor própria.

## Raio (border-radius)

- **999px** (pill total) — todo o interativo: botões, inputs, select, badges,
  segmented control (filtros), avatar-like.
- **20px** — cards/painéis: tenant card, stat-strip, pricing card, grelha de
  planos.
- **0** — quadrados de ícone (selos), 42×42, borda 1px sólida, sem raio.

## Borda

Regra única: `1px solid var(--fio)` em tudo (cards, tabelas, inputs, divisores).
Exceções propositadas:
- Botão outline e primário: `1px solid var(--tinta)` (ou `var(--accent)` no hover).
- Badge "isento"/"rascunho": `1px dashed var(--fio-forte)`.
- Segmented control (filtros): moldura exterior `1px solid var(--tinta)`,
  divisórias internas `1px solid var(--fio)`.

## Componentes (specs exatas do protótipo)

### Botão primário
```
border-radius: 999px; padding: 13px 26px; border: 1px solid #101014;
background: #101014; color: #EDEAE0; font-size: 13px; font-weight: 500;
```
Hover: border/background → `var(--accent)`, texto mantém `#EDEAE0`.

### Botão secundário (outline)
```
border-radius: 999px; padding: 13px 26px; border: 1px solid #101014;
background: transparent; color: #101014; font-size: 13px; font-weight: 500;
```
Hover: border/color → `var(--accent)`.

### Botão desativado/terciário (paginação "Anteriores") **(extrapolado do padrão)**
```
border-radius: 999px; padding: 13px 26px; border: 1px solid var(--fio);
background: transparent; color: var(--grafite); font-size: 13px; font-weight: 500;
```

### Badge de estado (pill)
Base comum: `display: inline-block; border-radius: 999px; padding: 4px 12px;
font-size: 11px; font-weight: 500; letter-spacing: 0.15em; white-space: nowrap;`

| Estado (exemplos) | Border | Cor do texto |
|---|---|---|
| neutro (em dia, pago, publicado) | `1px solid var(--fio)` | `var(--grafite)` |
| aviso (a expirar, aberto) | `1px solid #D9B44A` | `#8A6F16` |
| crítico (em atraso, esgotado) | `1px solid var(--accent)` | `var(--accent)` |
| inativo/rascunho (isento, rascunho) | `1px dashed var(--fio-forte)` | `var(--grafite)` |

Mapear para os `status`/`enum` reais do backend em `common/QA`: ver secção
"Mapeamento de estados" abaixo.

### Segmented control / filtros (tabs de estado)
Wrapper: `display:flex; border:1px solid var(--tinta); border-radius:999px; overflow:hidden;`
Item ativo: `padding:12px 18px; font-size:13px; font-weight:500; background:var(--tinta); color:var(--papel);`
Item inativo: mesmo padding/fonte, `background:transparent; color:var(--grafite); border-right:1px solid var(--fio)` (o último item sem border-right).

### Input de pesquisa / select
```
border: 1px solid var(--fio); border-radius: 999px; padding: 12px 20px;
font-size: 13px; font-weight: 500; background: transparent; color: #101014;
```

### Tabela
- `border-collapse: collapse; font-size:13px; font-weight:500; min-width:880px`
  (scroll horizontal em ecrãs estreitos — `overflow-x:auto` no wrapper).
- `th`: `text-align:left` (ou `right` em colunas numéricas/estado),
  `padding:14px 16px` (primeira coluna `40px` à esquerda, última `40px` à
  direita — gutter visual), `font-size:11px; font-weight:500;
  letter-spacing:0.15em; color:var(--grafite); border-bottom:1px solid var(--fio)`.
- `td`: `padding:15px 16px` (mesmo gutter nas pontas), `border-bottom:1px solid var(--fio)`.
  Coluna principal (nome): `font-size:13px;font-weight:700` + linha
  secundária por baixo (email) a `13px` cor `grafite`. Colunas numéricas e de
  estado alinhadas à direita.

### Cartão de tenant ativo (sidebar)
```
border: 1px solid var(--fio); border-radius: 20px; padding: 16px 18px;
```
Label eyebrow "TENANT ATIVO" (11px/500/.15em/grafite) → logo 30×30 + nome
(17px/700/-0.02em) → linha de contagem (13px/500/grafite, margin-top 10px).

### Nav lateral
`display:grid; gap:2px`. Item: `border-radius:999px; padding:11px 18px;
font-size:13px`. Ativo: `font-weight:700; color:var(--tinta);
border:1px solid var(--tinta)`. Inativo: `font-weight:500;
color:var(--grafite); border:1px solid transparent`.

### Rodapé de sessão (sidebar)
`margin-top:auto; border-top:1px solid var(--fio); padding-top:20px`. Label
eyebrow "SESSÃO" + nome/role a `13px/500` margin-top 10px.

### Cabeçalho de página (back-office)
`padding:32px ... 24px; border-bottom:1px solid var(--fio)`. Título (H1,
lowercase, clamp 38–58px) à esquerda, contagem/subtítulo por baixo
(13px/500/grafite, margin-top 14px), ações (botões) à direita.

### Stat-strip / KPI
Grelha com `gap:1px; background:var(--fio); border:1px solid var(--fio);
border-radius:20px; overflow:hidden`. Cada célula `background:var(--papel);
padding:22px 24px`. Número `54px/700/-0.045em/wdth 84`, label por baixo
`13px/500/grafite margin-top:8px`.

## Ícones ("selos")

Quadrado 42×42, borda 1px sólida `tinta`, sem raio, ícone SVG centrado
(stroke `tinta`, stroke-width 1.15, sem fill). Um selo por área: sócios,
quotas, eventos, conteúdo, órbita/tenant, registo. Os paths exatos estão em
`orbitawebsite.html` (`const ICONS`).

## Logo / marca

Sistema orbital gerado por função (elipses concêntricas rodadas
progressivamente + núcleo cheio + 1–2 nós). Lógica completa em
`orbita-identidade.html` (função `sistema()`). O nó principal usa `--accent`
(cor do tenant); um segundo nó opcional usa `latão` fixo.

## Mapeamento de estados (protótipo → domínio real)

O protótipo usa nomes ilustrativos; no `backend/prisma/schema.prisma` real os
enums são outros. Mapear por *categoria visual*, não por nome literal:

- **neutro** → `PaymentStatus.paid`, `EventStatus.published`/`open` sem
  urgência, `Content.status = published`.
- **aviso (âmbar)** → quota a vencer nos próximos dias, evento perto da
  capacidade, conteúdo a expirar.
- **crítico (accent)** → `PaymentStatus.overdue`, evento `esgotado`/lista de
  espera cheia.
- **tracejado/inativo** → `PaymentStatus.exempt`, `Content.status = draft`,
  subscrição `canceled`.

## Nota de implementação (tenant color)

`--accent` tem de vir de dados reais do tenant (`Company`), não estar
hard-coded. Hoje o schema não tem um campo de cor de marca — se não existir,
propor migração aditiva (`Company.brandColor`, default `#C4442B`) em vez de
inventar um valor fixo no frontend. Sinalizar isto como decisão a confirmar
antes de space, não assumir.
