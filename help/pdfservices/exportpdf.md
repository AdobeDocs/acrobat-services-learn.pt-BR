---
title: Uso da API de serviços do PDF para exportar PDF para Word, PowerPoint e muito mais
description: Saiba como executar a operação de exportação da API de serviços do PDF usando arquivos de exemplo para as linguagens Node.js, Java e .Net
type: Tutorial
role: Developer
level: Intermediate
thumbnail: KT-6674.jpg
kt: 6674
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 2%

---

# Uso da API de serviços do PDF para exportar PDF para Word, PowerPoint e muito mais

![Criar imagem de PDF Hero](assets/ExportPDF_hero.jpg)

A API de serviços da Adobe PDF converte arquivos PDF em MS Office, texto e imagens usando APIs. Há muitos casos de uso comuns para desbloquear PDF existentes para edição e análise de conteúdo e, com os serviços de API do PDF, os desenvolvedores podem integrar facilmente esse recurso aos sistemas e aplicativos existentes. Converta arquivos PDF em MS Word para edição de conteúdo, aprovações e envio posterior de assinaturas para criar fluxos de trabalho de contrato personalizados. Ou exporte conteúdo de PDF para o formato MS Excel para cálculos financeiros e de fatura ou análise de dados.

A operação Exportar suporta as seguintes conversões de arquivo PDF:

* PDF para Microsoft Word (DOC, DOCX)
* PDF para Microsoft PowerPoint (PPTX)
* PDF para Microsoft Excel (XLSX)
* PDF para texto (RTF)
* PDF para imagem (JPEG, PNG)

Neste tutorial, aprenda as noções básicas de como executar sua primeira operação de exportação da API de serviços do PDF usando arquivos de exemplo para as linguagens Node.js, Java e .Net.

## Etapa 1: Crie suas credenciais e configure seu ambiente:

Use os tutoriais de introdução abaixo para criar suas credenciais de API, baixar arquivos de amostra e configurar seu ambiente.

[Introdução à API de serviços PDF e Java](gettingstartedjava.md)

[Introdução à API de serviços do PDF e .Net](gettingstartednet.md)

[Introdução à API de serviços PDF e Node.js](createpdffromhtml.md)

## Etapa 2: Executar operação de exportação de pdf usando os arquivos de amostra

**Java**

1. Abra um prompt de comando.

1. Altere os diretórios no diretório de código de amostra.

   Por exemplo, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples

1. Execute o seguinte comando:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

Seu PDF é criado no diretório src/main/resources.

**.Net**

1. Abra um prompt de comando.

1. Altere os diretórios no diretório de código de amostra.

   Por exemplo, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Altere os diretórios novamente no diretório ExportPDFtoDocx.

1. Execute o seguinte comando:

   `dotnet run ExportPDFToDocx.csproj`

Seu PDF é criado no mesmo diretório.

**Node.js**

1. Abra um prompt de comando.

1. Altere os diretórios no diretório de código de amostra.

   Por exemplo, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Execute o seguinte comando:

   `node src/ocr/ocr-pdf.js`

Seu PDF é criado no local designado na saída, que por padrão é o diretório pdfServicesSdkResult.

## Considerações finais

Agora você deve ter um exemplo ativo que pode ser importado para seus aplicativos existentes para iniciar uma prova de conceito. Em cada um dos diretórios de exemplo, você pode ver outro exemplo para exportar arquivos PDF para o formato de imagem. As mesmas etapas acima permitem executar essa amostra também. Para mudar para outro formato, você pode atualizar o código para o novo formato desejado:

SupportedTargetFormats.PPTX

E o resultado do destino:

output/exportPdfOutput.PPTX

Para outro formato.

## Recursos e próximas etapas

* Para obter ajuda e suporte adicionais, visite o [[!DNL Adobe Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) fórum da comunidade

* API de serviços PDF [Documentação](https://www.adobe.com/go/pdftoolsapi_doc)

* [Perguntas frequentes](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) para perguntas sobre a API de serviços do PDF

* [Fale conosco](https://www.adobe.com/go/pdftoolsapi_requestform) para perguntas sobre licenciamento e preços
