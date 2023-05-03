---
title: Gerenciando fluxos de trabalho de documentos financeiros em Java
description: "[!DNL Adobe Acrobat Services] fornece todas as ferramentas, serviços e recursos necessários para processar e extrair dados de documentos financeiros do PDF"
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-7482.jpg
kt: 7482
exl-id: 3bdc2610-d497-4a54-afc0-8b8baa234960
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# Gerenciamento de fluxos de trabalho de documentos financeiros em Java

![Use Case Hero Banner](assets/UseCaseFinancialHero.jpg)

O setor financeiro usa arquivos PDF extensivamente para trocar dados porque ajuda a manter o formato, o design e a estrutura do documento. Esse formato robusto permite que analistas financeiros e consultores ajudem seus clientes a tomar decisões bem informadas.

No entanto, o formato PDF pode ser desafiador para processar e automatizar, especialmente ao combinar várias fontes de dados — um caso de uso comum no setor financeiro. Criar uma solução personalizada para processar documentos PDF é uma opção, mas não há necessidade de investir muito tempo e dinheiro em software e infraestrutura. [!DNL Adobe Acrobat Services] fornece todas as ferramentas, serviços e recursos necessários para processar e extrair dados de documentos do PDF.

## O que você pode aprender

Neste tutorial prático, aprenda a usar [!DNL Adobe Acrobat Services] APIs para [!DNL Java Spring Boot] aplicativos. Você cria um aplicativo MVC (controle de exibição de modelo) que extrai conteúdo de documentos PDF, converte-o em outros formatos de dados, como Excel, combina vários PDF e a senha protege os recursos. Este tutorial explica como processar documentos PDF e mostrá-los em seus sites usando o Adobe [API de incorporação do PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

## APIs e recursos relevantes

* [API de serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API de incorporação do PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Amostras do projeto](https://github.com/adobe/pdftools-java-sdk-samples)

## Configuração

[!DNL Adobe Acrobat Services] usa um sistema de autenticação para controlar o acesso a recursos. Para acessar os serviços, você deve solicitar uma chave de API do Adobe para sua organização ou aplicativo. Se você tiver uma chave de API, vá para a próxima seção. Para criar uma nova chave de API, visite [Introdução](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) no [!DNL Acrobat Services] local. Você pode criar uma chave usando a teste grátis, que fornece 1.000 transações de documentos que podem ser usadas por até seis meses.

Para acompanhar este tutorial, você precisa de dois conjuntos de chaves de API:

* Serviços Adobe PDF — usados para processar o documento PDF

* API incorporada do Adobe PDF

Depois de criar as credenciais, copie as credenciais da API de serviços do PDF e a chave privada para o [!DNL Spring Boot] dentro da seção resources. Saiba mais sobre o [Bibliotecas e dependências Maven e Gradle](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) no [!DNL Adobe Acrobat Services] site. Certifique-se de configurar todos os pacotes e bibliotecas necessários antes de continuar.

![Captura de tela do local do diretório para credenciais da API de serviços do PDF](assets/FAWJ_1.png)

Para configurar os serviços de registro, visite [documentação do Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) e role até a seção Log.

>[!NOTE]
>
> No ambiente de produção, não salve as chaves privadas no controle de versão. Sempre use um cofre secreto ou um serviço de injeção de chave para impedir o uso não autorizado de credenciais.

Agora que seu [!DNL Spring Boot] estiver configurado, você poderá continuar processando os PDF e gerando relatórios para os clientes.

## Enviando os dados do relatório

Para usar a API de serviços da Adobe PDF, primeiro configure um `ExecutionContext` que consome as credenciais fornecidas. Como você tem as credenciais dentro do aplicativo, é possível lê-las no arquivo e criar o contexto da seguinte maneira:

```
Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
    .fromFile(AUTH_FILE_PATH)
    .build();

ExecutionContext executionContext = ExecutionContext.create(credentials);
```

Em seguida, obtenha o contexto para processar os documentos do PDF. Estas são as ações que você pode executar:

* Converter os documentos do PDF (para Excel, Word ou tipo de gráfico)

* Crie os documentos do PDF (do HTML, Excel, Word e mais)

* Combinar vários documentos PDF

* Protect e desproteger os documentos PDF (você deve ter a senha)

* Otimizar os documentos do PDF para distribuição nas redes

Todas essas amostras estão disponíveis no [Amostras do GitHub](https://github.com/adobe/pdfservices-java-sdk-samples/tree/master/src/main/java/com/adobe/pdfservices/operation/samples) repositório.

Em seguida, em [!DNL Spring Boot], você pode obter um arquivo usando o caminho String ou o Fluxo no qual o arquivo está sendo carregado. Cada operação executada deve ser inicializada e um caminho de arquivo de entrada deve ser definido. Para este tutorial, use os relatórios PDF publicamente disponíveis da [Blackrock](https://www.blackrock.com/us/individual/products/investment-funds). Você pode usar qualquer outra fonte, incluindo seus próprios relatórios.

Comece capturando o [FileRef](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/io/FileRef.html) do arquivo. Para simplificar, foque nos arquivos por caminho de string. Abaixo, você cria uma operação para converter um arquivo em seu caminho de PDF para Excel:

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
ExportPDFOperation exportOperation = ExportPDFOperation.createNew(ExportPDFTargetFormat.XLSX);

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_PDF);
exportOperation.setInput(inputPdf);
```

Após essa etapa, o programa estará pronto para executar a primeira operação no PDF. Em seguida, você executa a operação e obtém o resultado na planilha do Excel:

```
try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_EXCEL);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

Esse cenário manipula apenas um arquivo PDF. Você também pode começar com vários arquivos PDF e combiná-los em um único arquivo. O uso de vários arquivos é comum no relatório de dados financeiros porque é necessário processar fundos de várias origens para fornecer um relatório abrangente.

## Gerando o relatório

[!DNL Adobe Acrobat Services] O não oferece suporte ao processamento de documentos do Excel prontos para uso, mas ainda é possível usar frameworks de comunidade e bibliotecas para processar o conteúdo.

Por exemplo, você pode usar o [Apache POI](https://poi.apache.org/) para processar o Excel (ou outros documentos do Microsoft) em seu [!DNL Java Spring Boot] ou executar outras tarefas manuais ou automatizadas no arquivo Excel.

Neste exemplo, começando com os documentos do PDF, você extrai o valor líquido do ativo para os três fundos e os mostra em uma tabela. Você também pode extrair outras informações, como gráficos e tabelas, com base em seus requisitos e nos dados disponíveis. Você pode até mesmo trazer dados de outras fontes.

Depois que o relatório for gerado, neste exemplo, em um formato Excel, você poderá usar as operações dos Serviços do Adobe PDF para converter o relatório de volta em um documento PDF e protegê-lo.

Para converter o relatório do formato Excel em um documento PDF, use a seguinte operação:

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
CreatePDFOperation exportOperation = CreatePDFOperation.createNew();

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_EXCEL);
exportOperation.setInput(inputPdf);

try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_PDF);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

>[!TIP]
>
> Para evitar ter que recriar o objeto toda vez que uma solicitação chegar, use a injeção de dependência de Spring para injetar o `ExecutionContext` objeto.

Esse código gera um documento PDF a partir do relatório no formato Excel.

Antes de fornecer essa PDF aos seus clientes, você pode protegê-la com uma senha. Criar outra operação que cuide dessa proteção para você, [ProtectPDFOperation](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/ProtectPDFOperation.html), depois use [ProtectPDFOptions](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/options/protectpdf/package-summary.html) para adicionar a senha ao documento.

```
ProtectPDFOptions options = ProtectPDFOptions.passwordProtectOptionsBuilder()
                    .setUserPassword("p@55w0rd")
                    .setEncryptionAlgorithm(EncryptionAlgorithm.AES_256)
                    .build();
ProtectPDFOperation operation = ProtectPDFOperation.createNew(options);
```

Em seguida, especifique a entrada e execute a operação. O arquivo resultante deve ter uma senha para impedir o acesso não autorizado.

## Exibição do relatório

Agora que o relatório do PDF é gerado, você pode exibi-lo no site usando a API incorporada do Adobe PDF. Essa API JavaScript permite que os desenvolvedores da Web carreguem e renderizem os documentos PDF de forma nativa dentro do navegador da Web.

>[!NOTE]
>
> Neste ponto, você precisa do segundo token de credencial, a ID do cliente.

No seu [!DNL Spring Boot] , adicione o seguinte snippet de HTML onde deseja renderizar o relatório PDF:

```
<div id="pdf-viewer"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function()
    {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-client-id-here>", divId: "pdf-viewer" });
        adobeDCView.previewFile(
        {
            content: {
                location: {
                    url: "<your-document.pdf>"
                }
            },
            metaData: {
                fileName: "<document-name.pdf>"
            }
        });
    });
</script>
```

Esse script carrega o documento PDF e permite que os visualizadores façam anotações e comentários nos documentos. Aqui está a exibição dessa API incorporada, conforme mostrado no Firefox:

![Captura de tela de um documento PDF no Firefox](assets/FAWJ_2.png)

A API PDF Embed fornece todas as ferramentas necessárias para visualizar o PDF, bem como para anotar o relatório.

## Próximas etapas

Este tutorial prático explorou o [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) APIs e discutiram como usar esses serviços para processar dados de PDF e gerar relatórios para decisões financeiras. Ele demonstrou como você pode integrar as APIs aos seus sistemas, usando [!DNL Java Spring Boot] como uma estrutura de exemplo, para mostrar como é fácil processar documentos PDF rapidamente.

Explorar [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) e veja o que os serviços da Adobe PDF podem fazer por sua empresa. Para saber mais sobre os recursos disponíveis no SDK, consulte o [Repositório GitHub](https://github.com/adobe/pdftools-java-sdk-samples) para as amostras e explorar como [API de incorporação do PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) pode ajudá-lo a mostrar rapidamente PDF dentro de seus aplicativos.

Para combinar e manipular documentos facilmente, criar relatórios de PDF úteis para seus clientes financeiros, comece inscrevendo-se gratuitamente [conta de desenvolvedor do Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/) hoje.
