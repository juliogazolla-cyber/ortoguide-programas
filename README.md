# Construtor de Programas · Masterclass OrtoGuide

Arquivo único: `index.html`. Não precisa de servidor nem de instalação.

## No ar

Link público: https://programa.ortoguideaccess.com.br/

O domínio aponta para o GitHub Pages por um registro CNAME no Registro.br (`programa` → `juliogazolla-cyber.github.io`). O link antigo, juliogazolla-cyber.github.io/ortoguide-programas, redireciona para o novo.

Para atualizar, edite o `index.html` e envie de novo para o repositório `juliogazolla-cyber/ortoguide-programas` (o Claude Code faz isso).

## Leads

Cada médico gera dois e-mails em ortoguidementoria@gmail.com, enviados pelo FormSubmit:
- "Novo cadastro na Masterclass": quando ele passa da primeira tela.
- "Lead quente": quando gera o blueprint, com modelo, valor da consulta, régua mínima e preço. É quem mais vale uma ligação.

Na primeira vez, o FormSubmit manda um e-mail de ativação. Sem clicar em "Activate form", nenhum lead chega.

Para trocar o e-mail, altere `emailLeads` no topo do `index.html`.

### Opcional: também numa planilha do Google

1. Crie uma planilha no Google Sheets.
2. Menu **Extensões > Apps Script**, apague o que tiver e cole:

```js
function doPost(e) {
  const d = JSON.parse(e.postData.contents);
  const ss = SpreadsheetApp.getActive();
  const sh = ss.getSheetByName('Leads') || ss.insertSheet('Leads');
  const cols = ['ts','evento','origem','nome','whatsapp','email','subespecialidade','cidade','momento',
    'vendePrograma','marketing','crencas','consulta','modelo','programa','semanas','parceiros','procedimentos','regua','preco'];
  if (sh.getLastRow() === 0) sh.appendRow(cols);
  sh.appendRow(cols.map(c => d[c] ?? ''));
  return ContentService.createTextOutput('ok');
}
```

3. **Implantar > Nova implantação > App da Web**. Executar como: você. Acesso: **Qualquer pessoa**.
4. Cole a URL `/exec` em `webhookUrl`, no topo do `index.html`.

## Botão final

`ctaUrl` e `ctaTexto` no mesmo `CONFIG`. Hoje abre o WhatsApp de vendas (51 99780-5002) com uma mensagem pronta.


## O que a ferramenta faz

8 telas curtas, pensadas para quem nunca montou um programa:
cadastro, o que é um programa (com as quebras de objeção), condição (ordenada pela subespecialidade), procedimentos, seu paciente, formato, preço e seu programa.

Na etapa de procedimentos o médico marca o que faz (infiltração, viscossuplementação, PRP, onda de choque, bloqueios, hidrodistensão, barbotagem, radiofrequência, ultrassom, órtese) e escolhe, para cada um, se entra incluído para todos ou à parte, quando indicado. Os incluídos entram no preço. Os valores sugeridos ficam no objeto `PROCS` e são só referência.

Tudo vem preenchido pelo modelo escolhido. Os detalhes (plano de ações, procedimentos à parte, métrica, brinde, nome) ficam numa tela opcional de ajustes, aberta a partir do resultado.

10 modelos pré-preenchidos: epicondilite, manguito, capsulite, artrose, lombalgia, fascite plantar, rizartrose, jornada cirúrgica, retorno ao esporte e em branco. Os textos dos modelos ficam no objeto `MODELOS` dentro do `index.html`. Vale um ortopedista da equipe revisar antes da aula.

Entregáveis para o aluno: blueprint em PDF, calendário do paciente para imprimir, roteiro de 5 frases, roteiro da secretária, planilha de KPIs (.csv) e o prompt completo do Arquiteto com o blueprint dele para continuar no ChatGPT, Claude ou Gemini.

O progresso fica salvo no navegador do aluno. Ele pode fechar e voltar depois.
