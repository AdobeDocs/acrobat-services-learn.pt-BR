---
title: Introdução à API de serviços Adobe PDF e Java
description: Os desenvolvedores podem começar em poucos minutos com os arquivos de amostra prontos para serem executados fornecidos para acessar todos os serviços da Web disponíveis
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6676.jpg
kt: 6676
exl-id: 4a8f2119-c464-496b-bdc8-35dd387bef25
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---

# Introdução à API de serviços da Adobe PDF e Java

![Criar imagem de PDF Hero](assets/GettingStartedJava_hero.jpg)

Os desenvolvedores podem começar em poucos minutos com os arquivos de amostra prontos para serem executados fornecidos para acessar todos os serviços da Web disponíveis. Este tutorial o orienta através de todas as etapas para começar a executar as amostras usando o PDF Services Java SDK:

## Etapa 1: Obtenção de credenciais e download de arquivos de amostra

A primeira etapa é obter uma credencial (Chave de API) para desbloquear o uso. [Cadastre-se para obter a teste grátis aqui](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) e clique em &quot;Começar&quot; para criar suas novas credenciais.

![Etapa 1](assets/GettingStartedJava_step1.png)

É importante escolher uma &quot;Conta pessoal&quot; para se cadastrar na avaliação gratuita:

![Pessoal](assets/GettingStartedJava_personal.png)

Na próxima etapa, você escolherá o Serviço de API de serviços do PDF e adicionará um nome e uma descrição para suas credenciais.

Há uma caixa de seleção para &quot;Criar amostra de código personalizada&quot;. Escolha essa opção para que suas novas credenciais sejam adicionadas automaticamente aos arquivos de amostra, o que salvará a etapa manual de adicioná-las ao projeto.

Em seguida, escolha Java como sua linguagem para receber as amostras específicas de Java e clique no botão &#39;Criar credenciais&#39;.

![Credenciais](assets/GettingStartedJava_credentials.png)

Você receberá um arquivo .zip para baixar, chamado PDFToolsSDK-JavaSamples.zip, que pode ser salvo em seu sistema de arquivos local.

## Etapa 2: Configurar o ambiente Java

1. Instalar [Java 8 ou superior](https://www.oracle.com/java/technologies/javase-downloads.html) se você ainda não fez isso.
1. Executar `javac -version` para verificar sua instalação.
1. Verifique se a pasta bin do JDK está incluída na variável PATH (o método varia de acordo com o SO).
1. Instalar [Maven](https://maven.apache.org/install.html) usando sua ferramenta preferida, caso ainda não tenha feito isso.

As amostras personalizadas fornecem tudo, desde código de amostra pronto para execução, um arquivo json de credencial incorporado e conexões pré-configuradas a dependências.

1. Baixar [o projeto de amostra](https://github.com/adobe/pdftools-java-sdk-samples).
1. Crie o projeto de amostra com o Maven: instalação limpa do mvn.
1. Teste o código de amostra na linha de comando ou no IDE de sua preferência.

## Considerações finais

A API de serviços do PDF pode ajudar a eliminar processos manuais, automatizando fluxos de trabalho comuns e transferindo a carga de processamento para a nuvem. Em um mundo em que cada navegador trata o PDF de maneira diferente, aproveitando a API incorporada do Adobe PDF junto com a API de serviços do PDF, você pode criar processos otimizados, confiáveis e previsíveis que são executados e exibidos corretamente **sempre** independentemente da plataforma ou do dispositivo.

## Recursos e próximas etapas

* Para obter ajuda e suporte adicionais, visite o Adobe [[!DNL Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) fórum da comunidade

* API de serviços PDF [Documentação](https://www.adobe.com/go/pdftoolsapi_doc)

* [Perguntas frequentes](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) para perguntas sobre a API de serviços do PDF

* [Fale conosco](https://www.adobe.com/go/pdftoolsapi_requestform) para perguntas sobre licenciamento e preços

* Artigos relacionados

   [A nova API de serviços do PDF oferece ainda mais recursos para fluxos de trabalho de documentos](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

   [Versão de julho de [!DNL Adobe Acrobat Services]: Serviços de PDF e incorporação de PDF](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
