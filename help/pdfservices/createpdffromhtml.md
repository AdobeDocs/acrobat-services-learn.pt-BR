---
title: Crie um PDF do HTML ou MS Office em alguns minutos com a PDF Services API e o Node.js
description: Dentro da API de serviços do PDF, há vários serviços disponíveis para criar e manipular PDF, ou exportar de PDF para MS Office e outros formatos
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6673.jpg
jira: KT-6673
exl-id: 1bd01bb8-ca5e-4a4a-8646-3d97113e2c51
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# Crie um PDF do HTML ou MS Office em alguns minutos com a PDF Services API e o Node.js

![Criar imagem de PDF Hero](assets/createpdffromhtml_hero.jpg)

A digitalização de fluxos de trabalho de documentos nunca foi tão fácil com a nova API de serviços da Adobe PDF, que oferece aos desenvolvedores liberdade para escolher entre vários poderosos serviços de manipulação de PDF para atender às necessidades de fluxos de trabalho complicados de negócios. Arquiteturas complicadas, estratégias de implementação e aumento de tecnologia podem ser simplificados com esses serviços da Web prontamente disponíveis na nuvem.

Dentro da API de serviços do PDF, há vários serviços disponíveis para criar e manipular PDF, ou exportar de PDF para MS Office e outros formatos.

* Crie um arquivo PDF de HTML estático ou dinâmico, MS Word, PowerPoint, Excel e muito mais
* Export PDF para MS Word, PowerPoint, Excel e muito mais
* OCR para reconhecer texto em arquivos PDF e ativar a pesquisa de documentos
* PDF Protect com senha ao abrir documentos
* Combine páginas PDF ou documentos PDF em um único PDF
* Compacte PDF para reduzir o tamanho do compartilhamento por email ou online
* Linearizar para otimizar um PDF para visualização rápida na Web
* Organize páginas PDF com serviços de inserção, substituição, reordenação, exclusão e rotação

Os desenvolvedores podem começar em poucos minutos com os arquivos de amostra prontos para serem executados fornecidos para acessar todos os serviços da Web disponíveis. Veja como começar.

## Obtenção de credenciais e download de arquivos de amostra

A primeira etapa é obter uma credencial (Chave de API) para desbloquear o uso. [Cadastre-se para obter a teste grátis aqui](https://www.adobe.com/go/dcsdks_credentials) e clique em &quot;Começar&quot; para criar suas novas credenciais.

![Chave de API](assets/apikey.png)

É importante escolher uma &quot;Conta pessoal&quot; para se cadastrar na avaliação gratuita:

![Conta Pessoal](assets/personalaccount.png)

Na próxima etapa, você escolherá o Serviço de API de serviços do PDF e adicionará um nome e uma descrição para suas credenciais.

Há uma caixa de seleção para &quot;Criar amostra de código personalizada&quot;. Escolha essa opção para que suas novas credenciais sejam adicionadas automaticamente aos arquivos de amostra, ignorando a etapa manual.

Em seguida, escolha Node.js como seu idioma para receber as amostras específicas de Node.js e clique no botão &#39;Criar credenciais&#39;.

![Criar credenciais](assets/createcredentials.png)

Você receberá um arquivo .zip para baixar, chamado PDFToolsSDK-Node.jsSamples.zip, que pode ser salvo em seu sistema de arquivos local.

## Adicionar suas credenciais aos exemplos de código

Se você escolheu a opção para &quot;Criar amostra de código personalizada&quot;, não precisa adicionar manualmente sua ID de cliente aos arquivos de amostra de código e pode pular a próxima etapa e ir diretamente para a seção Amostras de código em execução abaixo.

Se você não tiver escolhido a opção para &quot;Criar amostra de código personalizada&quot;, será necessário copiar a ID do cliente (chave de API) do Adobe.io Console:

![Exemplo de código](assets/codesample.png)

Descompacte o conteúdo de PDFToolsSDK-Node.jsSamples.zip.

Vá para o diretório raiz na pasta adobe-dc-pdf-tools-sdk-node-samples .

Abra pdftools-api-credentials.json com qualquer editor de texto ou IDE.

Cole a credencial no campo para a ID do cliente no código:

```javascript
{
 "client_credentials": {
  "client_id": "abcdefghijklmnopqrstuvwxyz",
```

Salve o arquivo e continue na próxima etapa para executar as amostras de código.

## Executando sua primeira amostra de código

Usando o prompt de comando, vá para o diretório raiz na pasta adobe-dc-pdf-tools-sdk-node-samples.

Digite npm install:

C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>npm install

Agora você está pronto para executar os arquivos de amostra!

Para sua primeira amostra, crie um PDF:

Ainda no prompt de comando, execute o comando create PDF sample com o seguinte comando:

C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>node src/createpdf/create-pdf-from-docx.js

Exemplo de saída:

![Exemplo de saída](assets/exampleoutput.png)

Seu PDF será criado no local designado na saída, que por padrão é o diretório pdfServicesSdkResult.

## Recursos e próximas etapas

* Para obter ajuda e suporte adicionais, visite o Adobe [[!DNL Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) fórum da comunidade

API de serviços PDF [Documentação](https://www.adobe.com/go/pdftoolsapi_doc)

* [Perguntas frequentes](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) para perguntas sobre a API de serviços do PDF

* [Fale conosco](https://www.adobe.com/go/pdftoolsapi_requestform) para perguntas sobre licenciamento e preços

* Artigos relacionados:
  [A nova API de serviços do PDF oferece ainda mais recursos para fluxos de trabalho de documentos](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [Versão de julho de [!DNL Adobe Acrobat Services]: Serviços de PDF e incorporação de PDF](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
