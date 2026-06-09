# Simulador de Taxas Ton — PWA

Simulador de taxas das maquininhas Ton para pequenos negócios e MEIs.
App instalável (PWA), 100% client-side, sem build system, sem dependências npm.

---

## Estrutura do projeto

```
pwa-ton/
├── index.html              ← App inteiro: HTML + CSS + JS num único arquivo
├── manifest.json           ← Identidade do PWA (nome, ícones, cores)
├── service-worker.js       ← Cache offline (cache-first strategy)
├── icon-192.png            ← Ícone padrão do app (any)
├── icon-512.png            ← Ícone grande (any)
├── icon-512-maskable.png   ← Ícone adaptativo para Android (maskable)
├── .nojekyll               ← Desabilita Jekyll no GitHub Pages
├── LEIA-ME.md              ← Instruções de deploy para o desenvolvedor
├── CLAUDE.md               ← Este arquivo
├── CLAUDE.local.md         ← Notas pessoais (gitignored, não commitar)
├── .mcp.json               ← Servidores MCP do projeto
└── .claude/
    ├── settings.json       ← Permissões e configurações do projeto (commitar)
    ├── settings.local.json ← Configurações locais pessoais (gitignored)
    └── commands/
        ├── update-rates.md    ← /update-rates: guia para atualizar taxas
        └── deploy-check.md    ← /deploy-check: valida arquivos antes do deploy
```

---

## Arquitetura

**Tudo vive em `index.html`.** Não há framework, transpilador, bundler ou package.json.
Editar CSS e JS significa editar diretamente o `<style>` e o `<script>` do arquivo.

### Estrutura do `index.html`
1. `<head>` — meta tags PWA + link para manifest + fonte Google Fonts (Poppins + Inter)
2. `<style>` — todo o CSS com variáveis CSS (`:root`) para cores da Ton
3. `<body>` — interface: `.wrap > h1 + .card(.form | .resultado)`
4. `<script>` — constante `TABELAS`, estado, funções de cálculo e eventos

### Paleta de cores (variáveis CSS)
| Variável         | Valor     | Uso                        |
|------------------|-----------|----------------------------|
| `--verde`        | `#15c466` | Destaque, pills ativos     |
| `--verde-claro`  | `#e7faef` | Fundo do toggle de repasse |
| `--verde-escuro` | `#0c3b29` | Textos, fundo do painel    |
| `--fundo`        | `#f2f7f4` | Background da página       |

---

## Dados: tabela de taxas (`TABELAS`)

As taxas vivem no objeto `TABELAS` no `<script>` de `index.html`.

```
TABELAS
├── standard          → T1, T2, T3, T3 Smart (mesma tabela)
│   ├── "1dia"        → recebimento em 1 dia útil
│   │   ├── promo     → plano promocional (30 dias / R$5 mil)
│   │   ├── ate3      → até R$3 mil/mês
│   │   ├── de3a6     → R$3 mil a R$6 mil/mês
│   │   ├── de6a10    → R$6 mil a R$10 mil/mês
│   │   ├── de10a30   → R$10 mil a R$30 mil/mês
│   │   └── acima30   → acima de R$30 mil/mês
│   │       └── cada plano: { mv: {...}, ea: {...} }
│   └── "mesmodia"    → recebimento no mesmo dia (estrutura idêntica)
└── tapton            → TapTon (sem planos por faturamento)
    ├── "1dia"        → { mv: {...}, ea: {...} }
    └── "mesmodia"    → { mv: {...}, ea: {...} }
```

Cada nó `mv` (Mastercard/Visa) ou `ea` (Elo/Amex) tem:
```js
{
  pix: 0,               // taxa Pix (sempre 0%)
  deb: 1.69,            // taxa débito (%)
  parc: [               // taxa crédito por número de parcelas (%)
    3.86,  // 1x (à vista)
    9.86,  // 2x
    11.24, // 3x
    // ... até 12x (índice 11)
  ]
}
```

**Para atualizar taxas:** edite apenas os valores numéricos no objeto `TABELAS`.
Não altere a estrutura de chaves — o código depende dela.

---

## Lógica de cálculo

**Sem repasse de taxa** (checkbox desmarcado):
```
taxaReais = valor × taxa/100
recebe    = valor − taxaReais
```

**Com repasse de taxa ao cliente** (checkbox marcado):
```
cobrado   = valor / (1 − taxa/100)   ← gross-up: merchant recebe valor cheio
taxaReais = cobrado − valor
recebe    = valor
```

---

## Como testar localmente

O service worker e a instalação como PWA exigem HTTPS ou `localhost`.

```bash
# Servidor local simples (Python)
python3 -m http.server 8080

# Ou Node.js
npx serve .
```

Abrir: `http://localhost:8080`

Verificar no DevTools do Chrome:
- Application → Manifest (sem erros)
- Application → Service Workers (status: activated)
- Lighthouse → PWA (pontuação esperada: 100)

---

## Deploy (GitHub Pages)

1. Commitar todos os arquivos (incluindo `.nojekyll`)
2. Settings → Pages → Deploy from branch `main` / `/ (root)`
3. URL: `https://USUARIO.github.io/REPO/`

**Arquivos obrigatórios no commit:**
`index.html`, `manifest.json`, `service-worker.js`,
`icon-192.png`, `icon-512.png`, `icon-512-maskable.png`, `.nojekyll`

**Nunca commitar:**
`CLAUDE.local.md`, `.claude/settings.local.json`

---

## Convenções de código

- JavaScript: ES6+, sem TypeScript, sem transpilador
- Formatação de valores: `n.toLocaleString("pt-BR", {minimumFractionDigits:2})`
- Eventos: `addEventListener` direto (sem delegação complexa)
- Estado global: objeto `estado` único no topo do script
- Sem `console.log` em produção

---

## Atualização de taxas

As taxas da Ton mudam periodicamente. Use o comando `/update-rates` para
um guia interativo de atualização. Sempre confirme as taxas diretamente no
app do Ton ou em `ton.com.br/planos-e-taxas` antes de publicar alterações.
