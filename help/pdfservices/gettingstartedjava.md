---
title: Introdução à API de Serviços do Adobe PDF e Java
description: Os desenvolvedores podem começar em apenas alguns minutos com os arquivos de amostra prontos para executar, fornecidos para acessar todos os serviços Web disponíveis
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6676
thumbnail: KT-6676.jpg
exl-id: 4a8f2119-c464-496b-bdc8-35dd387bef25
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# Introdução à API de Serviços do Adobe PDF e Java

![Criar Imagem Herói do PDF](assets/GettingStartedJava_hero.jpg)

Os desenvolvedores podem começar em apenas alguns minutos com os arquivos de amostra prontos para executar, fornecidos para acessar todos os serviços Web disponíveis. Este tutorial o orienta por todas as etapas para começar a executar os exemplos usando o SDK Java dos Serviços de PDF:

## Etapa 1: Obtendo credenciais e fazendo download de arquivos de amostra

A primeira etapa é obter uma credencial (chave de API) para desbloquear o uso. [Inscreva-se para a avaliação gratuita aqui](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) e clique em &#39;Começar&#39; para criar suas novas credenciais.

![Etapa 1](assets/GettingStartedJava_step1.png)

É importante escolher uma “Conta pessoal” para se cadastrar na avaliação gratuita:

![Pessoal](assets/GettingStartedJava_personal.png)

Na próxima etapa, você escolherá o Serviço de API de Serviços do PDF e adicionará um nome e uma descrição para suas credenciais.

Há uma caixa de seleção para “Criar amostra de código personalizada”. Escolha essa opção para que suas novas credenciais sejam adicionadas automaticamente aos arquivos de amostra, o que salvará a etapa manual de adicioná-las ao projeto.

Em seguida, escolha Java como sua linguagem para receber as amostras específicas de Java e clique no botão “Criar credenciais”.

![Credenciais](assets/GettingStartedJava_credentials.png)

Você receberá um arquivo .zip para baixar, chamado PDFToolsSDK-JavaSamples.zip, que pode ser salvo no sistema de arquivos local.

## Etapa 2: Configurar o ambiente Java

1. Instale o [Java 8 ou posterior](https://www.oracle.com/java/technologies/javase-downloads.html), se ainda não o tiver feito.
1. Execute o `javac -version` para verificar sua instalação.
1. Verifique se a pasta bin do JDK está incluída na variável PATH (o método varia de acordo com o sistema operacional).
1. Instale o [Maven](https://maven.apache.org/install.html) usando sua ferramenta preferida, se ainda não o fez.

As amostras personalizadas fornecem tudo, desde código de amostra pronto para execução, um arquivo json de credencial incorporado e conexões pré-configuradas a dependências.

1. Baixe [o projeto de amostra](https://github.com/adobe/pdftools-java-sdk-samples).
1. Construa o projeto de amostra com o Maven: mvn clean install.
1. Teste o código de amostra na linha de comando ou no IDE de sua preferência.

## Considerações finais

A API de serviços de PDF pode ajudar você a eliminar processos manuais, automatizando fluxos de trabalho comuns e transferindo a carga de processamento para a nuvem. Em um mundo onde cada navegador trata o PDF de forma diferente, aproveitando a API incorporada do Adobe PDF junto com a API de serviços de PDF, você pode criar processos simplificados, confiáveis e previsíveis que são executados e exibidos corretamente **toda vez**, independentemente da plataforma ou do dispositivo.

## Recursos e próximas etapas

* Para obter ajuda e suporte adicionais, visite o fórum da comunidade de [[!DNL Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) do Adobe

* Documentação [API de Serviços PDF](https://www.adobe.com/go/pdftoolsapi_doc)

* [Perguntas frequentes](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) sobre a API de Serviços PDF

* [Fale conosco](https://www.adobe.com/go/pdftoolsapi_requestform) em caso de dúvidas sobre licenciamento e preços

* Artigos relacionados

  [A nova API de Serviços PDF oferece ainda mais recursos para fluxos de trabalho de documentos](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [Versão de julho de [!DNL Adobe Acrobat Services]: serviços PDF Embed e PDF](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
