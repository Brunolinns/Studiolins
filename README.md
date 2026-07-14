# Studio Lins — site oficial

Página estática publicada via GitHub + Render. Fluxo de edição: alterar arquivos → commit → push → Render publica sozinho.

## 1. Subir no GitHub
1. Crie um repositório novo (ex.: `studio-lins-site`).
2. Envie todos os arquivos desta pasta para ele (index.html, assets/, render.yaml, sitemap.xml, robots.txt).

## 2. Publicar no Render
1. No Render: **New → Static Site** → conecte o repositório.
2. Build command: (vazio) · Publish directory: `.`
3. Deploy. O site sobe num endereço `.onrender.com` para conferência.

## 3. Apontar o domínio
1. No serviço do Render: **Settings → Custom Domains → Add** `studiolins.com.br` (e `www.studiolins.com.br`).
2. O Render mostra os registros DNS (A/ALIAS e CNAME). Configure-os no painel onde o domínio é gerenciado (registro.br ou a hospedagem antiga).
3. Propagação leva de minutos a algumas horas. O certificado HTTPS é automático.

## 4. Leads na planilha (Google Sheets)
1. Crie uma planilha no Google Sheets com o nome `Leads Studio Lins` e cabeçalhos na linha 1: `quando | nome | whatsapp | servico | origem`.
2. Na planilha: **Extensões → Apps Script**, apague o conteúdo e cole:

```javascript
function doPost(e) {
  var linha = JSON.parse(e.postData.contents);
  SpreadsheetApp.getActiveSpreadsheet().getSheets()[0]
    .appendRow([linha.quando, linha.nome, linha.whatsapp, linha.servico, linha.origem]);
  return ContentService.createTextOutput("ok");
}
```

3. **Implantar → Nova implantação → Tipo: App da Web** → Executar como: você · Acesso: **Qualquer pessoa** → Implantar → copie a URL gerada.
4. No `index.html`, cole a URL na constante `SHEETS_WEBHOOK` (linha ~470) e faça commit. Pronto: cada cadastro vira uma linha na planilha.

## 5. Pós-publicação (SEO)
1. No Google Search Console → **Sitemaps** → enviar `https://studiolins.com.br/sitemap.xml`.
2. Em **Inspeção de URL**, inspecionar `https://studiolins.com.br/` e clicar em **Solicitar indexação** — acelera o Google a reencontrar o site no ar.
