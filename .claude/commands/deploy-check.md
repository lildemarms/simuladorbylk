# /deploy-check — Validar antes do deploy no GitHub Pages

Executa uma checklist completa para garantir que o PWA está pronto para o GitHub Pages.

## Checklist a executar

### 1. Arquivos obrigatórios
Verifique se todos os arquivos existem:
```
index.html
manifest.json
service-worker.js
icon-192.png
icon-512.png
icon-512-maskable.png
.nojekyll
```

### 2. Valide o JSON
```bash
python3 -m json.tool manifest.json
```

### 3. Valide o JavaScript
```bash
node --check service-worker.js
```

### 4. Verifique caminhos absolutos (quebram em repositório de projeto)
Não deve haver nenhum resultado:
```bash
grep -n 'href="/' index.html
grep -n 'src="/' index.html
grep -n 'register("/' index.html
```

### 5. Verifique consistência dos ícones no manifest
Os arquivos `src` listados no manifest devem existir no diretório.

### 6. Verifique o cache do service worker
Os arquivos listados em `ASSETS` no `service-worker.js` devem existir.

### 7. Verifique arquivos gitignored não commitados
```bash
git status --short
```
`CLAUDE.local.md` e `.claude/settings.local.json` não devem aparecer como
arquivos rastreados.

### 8. Resumo final
Exiba:
- ✅ ou ❌ para cada item
- Lista de ações necessárias antes do deploy (se houver)
- Comando de commit sugerido se tudo estiver ok:
  `git add . && git commit -m "deploy: atualiza PWA"`
