# Simulador de Taxas Ton — PWA

App instalável (Progressive Web App) do simulador de taxas das maquininhas Ton.
Funciona offline e pode ser adicionado à tela inicial do Android e do iPhone.

> ✅ Pronto para GitHub Pages: caminhos relativos e arquivo `.nojekyll` já incluídos.

## Arquivos
- `index.html` — o simulador (página principal, abre automaticamente)
- `manifest.json` — identidade do app (nome, ícone, cores)
- `service-worker.js` — faz o app funcionar offline
- `icon-192.png`, `icon-512.png`, `icon-512-maskable.png` — ícones do app
- `.nojekyll` — desliga o Jekyll do GitHub Pages (recomendado)

## Publicar no GitHub Pages (passo a passo)
1. Crie um repositório novo no GitHub (pode ser público).
2. Envie TODOS os arquivos desta pasta para a RAIZ do repositório
   (não coloque dentro de subpasta). Inclua o arquivo oculto `.nojekyll`.
3. No repositório: Settings → Pages.
4. Em "Build and deployment": Source = "Deploy from a branch",
   Branch = `main`, pasta = `/ (root)` → Save.
5. Aguarde ~1 minuto. O endereço aparece no topo da própria página de Pages:
   `https://SEU-USUARIO.github.io/NOME-DO-REPO/`
6. Abra esse endereço no celular para instalar (veja abaixo).

Dica: se preferir publicar pela subpasta, mova tudo para uma pasta `docs/`
e escolha `/docs` no passo 4.

## Como instalar no celular
- **Android (Chrome):** abra o endereço HTTPS → aparece o aviso "Instalar app"
  (ou menu ⋮ → "Adicionar à tela inicial" / "Instalar aplicativo").
- **iPhone (Safari):** botão Compartilhar → "Adicionar à Tela de Início".

Depois de instalado, ele abre em tela cheia, com ícone próprio, e funciona sem internet.

## Observação
A instalação e o modo offline só funcionam por HTTPS — e o GitHub Pages já fornece
HTTPS automaticamente. Abrir o `index.html` direto do arquivo (file://) mostra o
simulador, mas não habilita a instalação como app.

Taxas baseadas na tabela oficial Ton para novos clientes. Consulte sempre as taxas
vigentes no app do Ton.

