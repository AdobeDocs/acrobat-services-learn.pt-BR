---
title: Introdução à API de serviços da Adobe PDF e .Net
description: Os desenvolvedores podem começar em poucos minutos com os arquivos de amostra prontos para serem executados fornecidos para acessar todos os serviços da Web disponíveis
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6675.jpg
jira: KT-6675
keywords: Destaque
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 0%

---

# Introdução à API de serviços da Adobe PDF e ao .Net

![Criar imagem de PDF Hero](assets/GettingStartedJava_hero.jpg)

Os desenvolvedores podem começar em poucos minutos com os arquivos de amostra prontos para serem executados fornecidos para acessar todos os serviços da Web disponíveis. Este tutorial o orienta através de todas as etapas para começar a executar as amostras usando o PDF Services .Net SDK:

## Etapa 1: Obtenção de credenciais e download de arquivos de amostra

A primeira etapa é obter uma credencial (Chave de API) para desbloquear o uso. [Cadastre-se para obter a teste grátis aqui](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) e clique em &quot;Começar&quot; para criar suas novas credenciais.

![Etapa 1](assets/GettingStartedJava_step1.png)

É importante escolher uma &quot;Conta pessoal&quot; para se cadastrar na avaliação gratuita:

![Pessoal](assets/GettingStartedJava_personal.png)

Na próxima etapa, você escolherá o Serviço de API de serviços do PDF e adicionará um nome e uma descrição para suas credenciais.

Há uma caixa de seleção para &quot;Criar amostra de código personalizada&quot;. Escolha essa opção para que suas novas credenciais sejam adicionadas automaticamente aos arquivos de amostra, o que salvará a etapa manual de adicioná-las ao projeto.

Em seguida, escolha Node.js como seu idioma para receber as amostras específicas de Node.js e clique no botão &#39;Criar credenciais&#39;.

![Credenciais](assets/GettingStartedJava_credentials.png)

Você receberá um arquivo .zip para baixar, chamado PDFToolsSDK-.NetSamples.zip, que pode ser salvo em seu sistema de arquivos local.

## Etapa 2: Configure o ambiente .Net e execute o código de exemplo

1. Baixe e instale o [SDK do .Net](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. Extraia o **[!UICONTROL PDFToolsSDK-.NetSamples.zip]** e descompacte o conteúdo
1. cd para o diretório raiz de amostras **[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. No diretório raiz de amostras, execute `dotnet build`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>compilação dotnet

   Agora você está pronto para executar os arquivos de amostra!

   Estas etapas finais mostram como executar sua primeira amostra com a operação Criar PDF a partir do Word:

1. No diretório raiz de amostras, altere para a pasta CreatePDFFromDocx, cd CreatePDFFromDocx/

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. executar `dotnet run CreatePDFFromDocx.csproj`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples\CreatePDFFromDocx>execução dotnet CreatePDFFromDocx.csproj

Seu PDF será criado no local designado na saída, que por padrão é a mesma pasta.

## Considerações finais

A API de serviços do PDF pode ajudar a eliminar processos manuais, automatizando fluxos de trabalho comuns e transferindo a carga de processamento para a nuvem. Em um mundo em que cada navegador trata o PDF de maneira diferente, aproveitando a API incorporada do Adobe PDF junto com a API de serviços do PDF, você pode criar processos otimizados, confiáveis e previsíveis que são executados e exibidos corretamente **sempre** independentemente da plataforma ou do dispositivo.

## Recursos e próximas etapas

* Para obter ajuda e suporte adicionais, visite o [[!DNL Adobe Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) fórum da comunidade

* API de serviços PDF [Documentação](https://www.adobe.com/go/pdftoolsapi_doc)

* [Perguntas frequentes](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) para perguntas sobre a API de serviços do PDF

* [Fale conosco](https://www.adobe.com/go/pdftoolsapi_requestform) para perguntas sobre licenciamento e preços

* Artigos relacionados

  [A nova API de serviços do PDF oferece ainda mais recursos para fluxos de trabalho de documentos](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [Versão de julho de [!DNL Adobe Acrobat Services]: Serviços de PDF e incorporação de PDF](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
