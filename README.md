# Construtor de Programas · Masterclass OrtoGuide

Arquivo único: `index.html`. Não precisa de servidor nem de instalação.

## Antes da masterclass (17/10)

### 1. Ligar a captura de leads (Google Sheets, 5 minutos)

1. Crie uma planilha no Google Sheets.
2. Menu **Extensões > Apps Script**, apague o que tiver e cole:

```js
function doPost(e) {
  const d = JSON.parse(e.postData.contents);
  const ss = SpreadsheetApp.getActive();
  const sh = ss.getSheetByName('Leads') || ss.insertSheet('Leads');
  const cols = ['ts','evento','origem','nome','whatsapp','email','subespecialidade','cidade','momento',
    'vendePrograma','marketing','crencas','consulta','modelo','programa','semanas','parceiros','regua','preco'];
  if (sh.getLastRow() === 0) sh.appendRow(cols);
  sh.appendRow(cols.map(c => d[c] ?? ''));
  return ContentService.createTextOutput('ok');
}
```

3. **Implantar > Nova implantação > Tipo: App da Web**. Executar como: você. Quem pode acessar: **Qualquer pessoa**.
4. Copie a URL que termina em `/exec` e cole no topo do `index.html`:

```js
const CONFIG = {
  webhookUrl: 'https://script.google.com/macros/s/XXXX/exec',
  ...
```

A planilha recebe duas linhas por médico:
- `cadastro`: quando ele passa da primeira tela.
- `blueprint`: quando gera o resultado, com modelo, valor da consulta, régua mínima e preço. É o lead mais quente.

Também funciona com Make, Zapier ou RD Station: qualquer URL que aceite POST com JSON no corpo.

### 2. Botão final

`ctaUrl` e `ctaTexto` no mesmo `CONFIG`. Hoje aponta para o Instagram da OrtoGuide. Troque pelo link do WhatsApp de vendas ou da página de aplicação da Mentoria.

### 3. Publicar

Qualquer hospedagem de site estático serve. O mais rápido: arrastar a pasta em https://app.netlify.com/drop e ganhar um link na hora. Também roda em Vercel, GitHub Pages ou numa subpasta do site da OrtoGuide.

## O que a ferramenta faz

10 etapas, seguindo o método do Arquiteto de Programas:
cadastro, crenças, programa-modelo, dor e transformação, tempo e contatos, parceiros e acopláveis, pilares e ações, métrica e experiência, preço, blueprint.

8 modelos pré-preenchidos: epicondilite, manguito, capsulite, artrose, lombalgia, jornada cirúrgica, retorno ao esporte e em branco. Os textos dos modelos ficam no objeto `MODELOS` dentro do `index.html`. Vale um ortopedista da equipe revisar antes da aula.

Entregáveis para o aluno: blueprint em PDF, calendário do paciente para imprimir, roteiro de 5 frases, roteiro da secretária, planilha de KPIs (.csv) e o prompt completo do Arquiteto com o blueprint dele para continuar no ChatGPT, Claude ou Gemini.

O progresso fica salvo no navegador do aluno. Ele pode fechar e voltar depois.
