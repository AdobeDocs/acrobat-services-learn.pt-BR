---
title: Usando a API de Serviços do Adobe PDF para Arquivos de PDF de OCR
description: Com o OCR (reconhecimento óptico de caracteres), você pode desbloquear PDF digitalizados para extrair texto e criar arquivos pesquisáveis
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6677
thumbnail: KT-6677.jpg
exl-id: 61a9a2d1-94c3-41c2-8f90-a56a938ef245
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 2%

---

# Uso da API de Serviços do Adobe PDF em arquivos de PDF de OCR

![Criar imagem principal do PDF](assets/OCR_hero.jpg)

Com o OCR (reconhecimento óptico de caracteres), você pode desbloquear PDF digitalizados para extrair texto e criar arquivos pesquisáveis. Usando nossas eficientes APIs baseadas em nuvem, integre o OCR em qualquer fluxo de trabalho de documento para obter a solução perfeita para arquivar, copiar texto e criar índices de documentos pesquisáveis. Crie arquivos pesquisáveis a partir de repositórios de PDF escaneados para desbloquear informações importantes e economizar tempo com rápida capacidade de pesquisa. Ou aplique o OCR aos seus PDF de digitalizações carregadas para permitir que sejam editados para uso em fluxos de trabalho de integração.

Os desenvolvedores podem começar em apenas alguns minutos com os arquivos de amostra prontos para execução fornecidos para OCR.

Neste tutorial, aborda as noções básicas de como executar sua primeira operação de OCR da API de serviços de PDF usando arquivos de amostra para as linguagens Node.js, Java e .Net.

## Etapa 1: Criar suas credenciais e configurar seu ambiente

Use os tutoriais de introdução abaixo para criar suas credenciais de API, baixar arquivos de amostra e configurar seu ambiente.

[Introdução à API de Serviços PDF e Java](gettingstartedjava.md)

[Introdução à API de Serviços PDF e .Net](gettingstartednet.md)

[Introdução à API de Serviços de PDF e ao Node.js](createpdffromhtml.md)

## Execute o exemplo de OCR fornecido nos arquivos de amostra

Nossa operação de OCR permite localidades em inglês por padrão, mas também oferece suporte para alemão, francês, dinamarquês e [outros idiomas](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#ocr-with-explicit-language). O padrão é a localidade en-us.

Quando você passa opções com a operação de OCR incluindo o local específico, o método também aceita o parâmetro &#39;type&#39;, que tem duas opções:

* SEARCHABLE_IMAGE: modifica a imagem original durante o processo de limpeza (por exemplo, desloca-a) antes de colocar uma camada de texto invisível sobre ela. Esse tipo remove artefatos indesejados e pode resultar em um documento mais legível em alguns cenários.

* SEARCHABLE_IMAGE_EXACT: Garante que o texto seja pesquisável e selecionável. Esta opção mantém a imagem original e coloca uma camada de texto invisível sobre ela. Recomendado para casos que exigem o máximo de fidelidade para a imagem original.

**Java**

1. Abra um prompt de comando.

1. Mude os diretórios para o diretório de código de exemplo.

   Por exemplo, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples>.

1. Execute o seguinte comando:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.ocrpdf.OcrPDF`

Seu PDF será criado no diretório src/main/resources.

**.Net**

1. Abra um prompt de comando.

1. Mude os diretórios para o diretório de código de exemplo.

   Por exemplo, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Altere os diretórios novamente para o diretório OcrPDF.

1. Execute o seguinte comando:

   `dotnet run OcrPDF.csproj`

Seu PDF será criado no mesmo diretório.

**Node.js**

1. Abra um prompt de comando.

1. Mude os diretórios para o diretório de código de exemplo.

   Por exemplo, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Execute o seguinte comando:

   `node src/ocr/ocr-pdf.js`

Seu PDF será criado no local designado na saída, que por padrão é o diretório de saída.

## Considerações finais

Com essas etapas simples usando os arquivos de amostra, você deve ter um exemplo de trabalho no qual é possível criar. Além do exemplo de OCR que usamos neste tutorial, há outro exemplo de OCR usando as opções de tipo e localidade aceitas discutidas anteriormente.

A partir daí, você pode simplesmente substituir os arquivos de entrada e saída localizados na amostra para usar seu próprio PDF a fim de finalizar sua prova de conceito para seu próprio caso de uso.

![Prova de conceito](assets/OCR_poc.png)

## Recursos e próximas etapas

* Para obter mais ajuda e suporte, visite o Adobe [[!DNL Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) fórum da comunidade

* API de serviços PDF [Documentação](https://www.adobe.com/go/pdftoolsapi_doc)

* [Perguntas frequentes](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) para perguntas sobre a API de serviços PDF

* [Fale conosco](https://www.adobe.com/go/pdftoolsapi_requestform) em caso de dúvidas sobre licenciamento e preços
