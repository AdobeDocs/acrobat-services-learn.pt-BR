---
title: Usar a API de Serviços de PDF para exportar PDF para Word, PowerPoint e muito mais
description: Saiba como executar a operação de exportação da API de serviços de PDF usando arquivos de amostra para as linguagens Node.js, Java e .Net
feature: PDF Services API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-6674
thumbnail: KT-6674.jpg
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 2%

---

# Usar a API de Serviços de PDF para exportar PDF para Word, PowerPoint e muito mais

![Criar imagem principal do PDF](assets/ExportPDF_hero.jpg)

A API de Serviços do Adobe PDF converte arquivos PDF em MS Office, texto e imagens usando APIs. Há muitos casos de uso comuns para desbloquear PDF existentes para edição e análise de conteúdo e, com os serviços de PDF, os desenvolvedores de API podem integrar facilmente esse recurso nos sistemas e aplicativos existentes. Converta arquivos PDF em MS Word para edição de conteúdo, aprovações e envio posterior de assinaturas para criar fluxos de trabalho de contratos personalizados. Ou exporte o conteúdo de PDF para o formato MS Excel para cálculos de fatura e financeiros ou análise de dados.

A operação de exportação suporta as seguintes conversões de arquivo PDF:

* PDF para Microsoft Word (DOC, DOCX)
* PDF para Microsoft PowerPoint (PPTX)
* PDF para Microsoft Excel (XLSX)
* PDF para texto (RTF)
* PDF para imagem (JPEG, PNG)

Neste tutorial, aprenda os conceitos básicos de como executar sua primeira operação de exportação da API de serviços de PDF usando arquivos de amostra para as linguagens Node.js, Java e .Net.

## Etapa 1: Criar suas credenciais e configurar seu ambiente:

Use os tutoriais de introdução abaixo para criar suas credenciais de API, baixar arquivos de amostra e configurar o seu ambiente.

[Introdução à API de Serviços PDF e Java](gettingstartedjava.md)

[Introdução à API de Serviços PDF e .Net](gettingstartednet.md)

[Introdução à API de Serviços de PDF e ao Node.js](createpdffromhtml.md)

## Etapa 2: execute a operação de exportação de pdf usando os arquivos de amostra

**Java**

1. Abra um prompt de comando.

1. Mude os diretórios para o diretório de código de exemplo.

   Por exemplo, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples

1. Execute o seguinte comando:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

Seu PDF é criado no diretório src/main/resources.

**.Net**

1. Abra um prompt de comando.

1. Mude os diretórios para o diretório de código de exemplo.

   Por exemplo, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Altere os diretórios novamente para o diretório ExportPDFtoDocx.

1. Execute o seguinte comando:

   `dotnet run ExportPDFToDocx.csproj`

Seu PDF é criado no mesmo diretório.

**Node.js**

1. Abra um prompt de comando.

1. Mude os diretórios para o diretório de código de exemplo.

   Por exemplo, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Execute o seguinte comando:

   `node src/ocr/ocr-pdf.js`

Seu PDF é criado no local designado na saída, que por padrão é o diretório pdfServicesSdkResult.

## Considerações finais

Agora você deve ter um exemplo de trabalho que pode ser importado para os aplicativos existentes para iniciar uma prova de conceito. Em cada um dos diretórios de amostra, é possível ver outro exemplo para exportar arquivos de PDF para o formato de imagem. As mesmas etapas acima também permitem executar essa amostra. Para mudar para outro formato, você pode atualizar o código para o novo formato que deseja:

SupportedTargetFormats.PPTX

E o resultado do destino:

output/exportPdfOutput.PPTX

Para outro formato.

## Recursos e próximas etapas

* Para obter ajuda e suporte adicionais, visite o [[!DNL Adobe Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) fórum da comunidade

* API de serviços PDF [Documentação](https://www.adobe.com/go/pdftoolsapi_doc)

* [Perguntas frequentes](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) para perguntas sobre a API de serviços PDF

* [Fale conosco](https://www.adobe.com/go/pdftoolsapi_requestform) em caso de dúvidas sobre licenciamento e preços
