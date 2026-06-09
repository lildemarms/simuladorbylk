# /update-rates — Atualizar tabela de taxas

Ajuda a atualizar as taxas no simulador de forma segura e consistente.

## Instruções

1. Pergunte ao usuário qual taxa mudou:
   - Maquininha (standard: T1/T2/T3/T3 Smart, ou TapTon)
   - Prazo (1dia / mesmodia)
   - Plano (promo / ate3 / de3a6 / de6a10 / de10a30 / acima30)
   - Bandeira (mv = Mastercard/Visa, ea = Elo/Amex)
   - Tipo (pix / deb / parcela Nx)
   - Novo valor (%)

2. Localize o valor no objeto `TABELAS` dentro do `<script>` de `index.html`.

3. Mostre o valor atual e o novo valor antes de editar.

4. Edite **apenas** o valor numérico. Não altere a estrutura de chaves.

5. Após a edição:
   - Confirme que o JSON interno está correto com: `node --check index.html`
   - Mostre um resumo das alterações com: `git diff index.html`

6. Sugira uma mensagem de commit no padrão:
   `feat: atualiza taxa [tipo] [plano] [prazo] para X%`

## Regras importantes

- Nunca altere a estrutura do objeto `TABELAS`, apenas valores numéricos
- `parc` é um array de 12 posições: índice 0 = crédito 1x, índice 11 = crédito 12x
- Taxas sempre em percentual (ex: `1.69` para 1,69%)
- Pix é sempre `0` em todos os planos
- Confirme a fonte das novas taxas (app Ton ou ton.com.br) antes de aplicar
