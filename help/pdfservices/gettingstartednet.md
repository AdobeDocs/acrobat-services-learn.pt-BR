---
title: Introdução à API de Serviços do Adobe PDF e ao .Net
description: Os desenvolvedores podem começar em apenas alguns minutos com os arquivos de amostra prontos para executar, fornecidos para acessar todos os serviços Web disponíveis
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6675
thumbnail: KT-6675.jpg
keywords: Destacado
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---

# Introdução à API de Serviços do Adobe PDF e ao .Net

![Criar Imagem Herói do PDF](assets/GettingStartedJava_hero.jpg)

Os desenvolvedores podem começar em apenas alguns minutos com os arquivos de amostra prontos para executar, fornecidos para acessar todos os serviços Web disponíveis. Este tutorial o orienta por todas as etapas para começar a executar os exemplos usando o SDK do PDF Services .Net:

## Etapa 1: Obtendo credenciais e fazendo download de arquivos de amostra

A primeira etapa é obter uma credencial (chave de API) para desbloquear o uso. [Inscreva-se para a avaliação gratuita aqui](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) e clique em &#39;Começar&#39; para criar suas novas credenciais.

![Etapa 1](assets/GettingStartedJava_step1.png)

É importante escolher uma “Conta pessoal” para se cadastrar na avaliação gratuita:

![Pessoal](assets/GettingStartedJava_personal.png)

Na próxima etapa, você escolherá o Serviço de API de Serviços do PDF e adicionará um nome e uma descrição para suas credenciais.

Há uma caixa de seleção para “Criar amostra de código personalizada”. Escolha essa opção para que suas novas credenciais sejam adicionadas automaticamente aos arquivos de amostra, o que salvará a etapa manual de adicioná-las ao projeto.

Em seguida, escolha Node.js como o idioma para receber as amostras específicas de Node.js e clique no botão “Criar credenciais”.

![Credenciais](assets/GettingStartedJava_credentials.png)

Você receberá um arquivo .zip para download, chamado PDFToolsSDK-.NetSamples.zip, que pode ser salvo no sistema de arquivos local.

## Etapa 2: configure o ambiente .Net e execute o código de exemplo

1. Baixe e instale o [.Net SDK](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. Extraia o **[!UICONTROL PDFToolsSDK-.NetSamples.zip]** baixado e descompacte o conteúdo
1. cd para o diretório raiz de amostras **[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. No diretório raiz de amostras, execute `dotnet build`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>compilação dotnet

   Agora você está pronto para executar os arquivos de amostra!

   Estas etapas finais mostram como executar sua primeira amostra com a operação Criar PDF a partir do Word:

1. No diretório raiz de exemplos, altere para CreatePDFFromDocx, cd CreatePDFFromDocx/

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. executar `dotnet run CreatePDFFromDocx.csproj`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples\CreatePDFFromDocx>execução de dotnet CreatePDFFromDocx.csproj

Seu PDF será criado no local designado na saída, que por padrão é a mesma pasta.

## Considerações finais

A API de serviços de PDF pode ajudar você a eliminar processos manuais, automatizando fluxos de trabalho comuns e transferindo a carga de processamento para a nuvem. Em um mundo onde cada navegador trata o PDF de forma diferente, aproveitando a API incorporada do Adobe PDF junto com a API de serviços de PDF, você pode criar processos simplificados, confiáveis e previsíveis que são executados e exibidos corretamente **toda vez**, independentemente da plataforma ou do dispositivo.

## Recursos e próximas etapas

* Para obter ajuda e suporte adicionais, visite o fórum da comunidade [[!DNL Adobe Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all)

* Documentação [API de Serviços PDF](https://www.adobe.com/go/pdftoolsapi_doc)

* [Perguntas frequentes](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) sobre a API de Serviços PDF

* [Fale conosco](https://www.adobe.com/go/pdftoolsapi_requestform) em caso de dúvidas sobre licenciamento e preços

* Artigos relacionados

  [A nova API de Serviços PDF oferece ainda mais recursos para fluxos de trabalho de documentos](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [Versão de julho de [!DNL Adobe Acrobat Services]: serviços PDF Embed e PDF](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
