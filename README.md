# Acordo — Orçamentos e propostas

[Acessar o sistema](https://alexmanhes.github.io/acordo-orcamentos/)

Aplicação frontend demonstrativa em português para cadastrar clientes e serviços, compor orçamentos, aplicar descontos, baixar propostas em PDF e simular decisões de clientes. HTML, CSS e JavaScript, sem build ou servidor de aplicação.

## Fluxo

- Clientes e serviços com edição, arquivamento e restauração. Os cadastros arquivados continuam preservados nas propostas existentes.
- Orçamento com até 80 itens, quantidades fracionadas, preços negociados, desconto percentual ou fixo, validade e condições comerciais.
- Rascunho → marcado como enviado → aprovação ou recusa simulada. Propostas vencidas não aceitam decisões. Editar somente rascunhos; duplicar cria uma revisão independente.
- Dados do cliente, emissor, itens e preços são copiados para a proposta. Mudanças no catálogo não alteram propostas já criadas.
- PDF A4 com fonte Lato incorporada, itens paginados, totais, desconto, condições e decisão registrada. Download direto no navegador, sem impressão ou serviço externo.
- Indicadores calculados a partir da base local. Taxa de aprovação = aprovadas / (aprovadas + recusadas); propostas expiradas ficam fora das decisões.

## Cálculos

Valores monetários inteiros em centavos; quantidades em centésimos e percentuais em pontos-base. Produtos e divisão usam BigInt, com arredondamento de metade para cima. Cada item é arredondado antes da soma; o desconto é aplicado uma vez ao subtotal. Desconto fixo acima do subtotal, percentual acima de 100%, quantidades zeradas, números negativos e limites excessivos são recusados. Não há cálculo fiscal.

## Dados e limites

A base inicial é fictícia e usa a data de Brasília como referência. Cadastros, propostas, decisões e histórico ficam no localStorage deste navegador, chave `acordo-data-v1`. Sem contas de usuários, envio de e-mail, link remoto de aprovação, sincronização ou assinatura digital. A visão do cliente e suas decisões são simulações locais. Os indicadores não representam uma empresa real.

Gravações ocorrem antes de atualizar a interface; se o armazenamento falhar, a alteração não é confirmada. Dados locais inválidos não são sobrescritos e a aplicação abre em modo de consulta. Limpar o armazenamento do navegador remove os registros salvos.

## Executar

```sh
node serve.cjs
```

Abra `http://127.0.0.1:8811/`. Hospedagem estática: raiz do projeto.

```sh
node --check js/app.js
node --check js/model.js
node --check js/pdf.js
node tests/model.cjs
node tests/pdf.cjs
```

Os testes cobrem datas, dinheiro, descontos, snapshots, estados, expiração, cadastros e propostas extensas. O teste de PDF escreve documentos demonstrativos em `outputs/pdf/`, para renderização e revisão visual.

## Organização

```text
index.html
css/styles.css
js/model.js        # Regras de negócio
js/app.js          # Interface e persistência
js/pdf.js          # Proposta PDF
vendor/           # jsPDF e licença MIT
assets/fonts/     # Lato e licença OFL
tests/
```

PDF gerado com [jsPDF 4.2.1](https://github.com/parallax/jsPDF/releases/tag/v4.2.1), distribuído localmente sob licença MIT. Fontes [Lato](https://github.com/google/fonts/tree/main/ofl/lato), sob licença OFL, incorporadas no PDF. DM Sans e Manrope da interface são carregadas pelo Google Fonts, com alternativas do sistema. Não são usados recursos HTML externos na geração do documento.


## GitHub Pages

Publicação pela branch main, pasta / (root). O arquivo .nojekyll mantém os arquivos estáticos. Bibliotecas, fontes e licenças do PDF estão no próprio repositório.
