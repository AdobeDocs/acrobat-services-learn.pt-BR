---
title: Gerenciando Propostas de Vendas e Contratos
description: Saiba como criar um fluxo de trabalho eficiente para automatizar e simplificar propostas de vendas
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8099.jpg
jira: KT-8099
exl-id: 219c70de-fec1-4946-b10e-8ab5812562ef
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1395'
ht-degree: 2%

---

# Gerenciamento de propostas de vendas e contratos

![Use Case Hero Banner](assets/UseCaseSalesHero.jpg)

As propostas de vendas são o primeiro passo na jornada de uma empresa rumo à aquisição de clientes. Como em tudo, as primeiras impressões duram. Assim, sua primeira interação com os clientes define suas expectativas para sua empresa. Sua proposta deve ser concisa, precisa e conveniente.

Os contratos e as propostas contêm diferentes tipos de dados na sua estrutura documental. Eles contêm dados dinâmicos (nome do cliente, valor da cotação e assim por diante) e dados estáticos (texto padrão, como recursos firmes, perfis de equipe e termos padrão da SOW). Criar documentos de modelo, como propostas de vendas, geralmente envolve tarefas monótonas, como substituir manualmente os detalhes do projeto em um modelo padrão. Neste tutorial, você usa dados dinâmicos e fluxos de trabalho para criar um processo eficiente para [criação de propostas de vendas](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

## O que você pode aprender

Neste tutorial prático, aprenda a implementar dados e fluxos de trabalho dinâmicos usando várias ferramentas, as mais importantes das quais: [!DNL Adobe Acrobat Services] APIs. Essas APIs são usadas para tornar propostas de vendas e contratos mais convenientes para você e sua empresa. Este tutorial demonstra técnicas práticas para mostrar como criar, mesclar e exibir documentos do PDF automaticamente. Realizar essas tarefas manualmente é demorado e tedioso. Ao tirar partido [!DNL Acrobat Services] APIs, você pode reduzir o tempo gasto nessas tarefas.

## APIs e recursos relevantes

* [Microsoft Word](https://www.office.com/)

* [Node.js](https://nodejs.org/en/)

* [npm](https://www.npmjs.com/get-npm)

* [[!DNL Acrobat Services] APIs](https://www.adobe.io/apis/documentcloud/dcsdk/)

* [API de geração de documento Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [API do Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Marcador de Geração de Documento Adobe](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

## Resolvendo o problema

Agora que você tem as ferramentas instaladas, pode começar a resolver o problema. As propostas têm conteúdo estático e conteúdo dinâmico exclusivos para cada cliente. Os gargalos ocorrem porque ambos os tipos de dados são necessários sempre que você faz uma proposta. A inserção do texto estático é demorada, portanto ele será automatizado e só será possível lidar manualmente com os dados dinâmicos de cada cliente.

Primeiro, crie um formulário de captura de dados no [Microsoft Forms](https://www.office.com/launch/forms?auth=1) (ou o criador de formulários preferido). Este formulário é para os dados dinâmicos dos clientes que são adicionados a uma proposta de vendas. Preencha este formulário com perguntas para obter os detalhes necessários dos clientes — por exemplo, nome da empresa, data, endereço, escopo do projeto, preços e comentários adicionais. Para criar o seu próprio, use este [formulário](https://forms.office.com/Pages/ShareFormPage.aspx id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAAAAN__rtiGj5UNElTR0pCQ09ZNkJRUlowSjVQWDNYUEg2RC4u&amp;sharetoken=1AJeMavBAzzxuISRKmUy). O objetivo é que clientes potenciais preencham o formulário e exportem suas respostas como arquivos JSON, que são passados para a próxima parte do fluxo de trabalho.

Alguns criadores de formulários permitem exportar dados apenas como arquivos CSV. Então, você pode achar útil [converter](http://csvjson.com/csv2json) o arquivo CSV gerado em um arquivo JSON.

Os dados estáticos são reutilizados em todas as propostas de vendas. Portanto, você pode usar um modelo de proposta de vendas no Microsoft Word para fornecer o texto estático. Você pode usar isso [modelo](https://1drv.ms/w/s!AiqaN2pp7giKkmhVu2_2pId9MiPa?e=oeqoQ2), mas você pode criar sua própria [modelo Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html).

Agora, você precisa de algo que tire os dados dinâmicos dos clientes no formato JSON e o texto estático no modelo do Microsoft Word para fazer uma proposta de vendas exclusiva para um cliente. O [!DNL Acrobat Services] As APIs são usadas para mesclar os dois e gerar um PDF que pode ser assinado.

Para que isso funcione, use tags. Tags são sequências de caracteres fáceis de usar que podem representar números, palavras, matrizes ou até mesmo objetos complexos. As marcas atuam como um espaço reservado para dados dinâmicos, que, nesse caso, são dados de cliente inseridos no formulário. Depois de inserir tags no modelo, você pode mapear os campos de formulário do arquivo JSON para o modelo do Word.

## Uso de tags

Abra o modelo de proposta de vendas e selecione o **Inserir** guia. No menu **Suplementos** grupo, selecione **Obter Suplementos**. Em seguida, selecione **Suplemento de geração de documento Adobe** para adicioná-lo. Depois de adicionado, você verá o Marcador de geração de documento na **Início** na guia **Adobe** grupo.

No menu **Início** na guia **Adobe** grupo, selecione **Geração de documento** para começar a marcar o documento. Um vídeo de demonstração útil aparece em um painel no lado direito da janela.

![Captura de tela do suplemento Marcador de documento no Word](assets/sales_1.png)

Selecionar **Introdução**. Você será solicitado a fornecer dados de amostra. Cole ou faça upload do arquivo JSON de resposta de formulário conforme mostrado abaixo.

![Captura de tela de colagem do código de amostra](assets/sales_2.png)

Selecionar **Gerar marcas** para obter uma lista de campos do arquivo JSON que você colou ou carregou. As tags são exibidas abaixo, na barra lateral direita.

![Captura de tela das tags disponíveis](assets/sales_3.png)

Depois de gerar as marcas, você pode inseri-las no documento. As marcas são adicionadas ao documento no local do cursor. Como mostrado acima, você deve adicionar o **Escopo do projeto** logo abaixo do **Escopo do projeto** legenda. Dessa forma, quando um cliente insere o escopo do projeto no formulário, sua resposta fica abaixo do **Escopo do projeto** , substituindo a tag que acabou de adicionar. Depois de terminar de adicionar marcas, parte do documento deve ter a mesma aparência da captura de tela abaixo.

![Captura de tela da adição de tags a um documento do Word](assets/sales_4.png)

## Uso das APIs

Vá para a [!DNL Acrobat Services] APIs [homepage](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html). Para começar a usar [!DNL Acrobat Services] APIs, você precisa de credenciais para seu aplicativo. Role para baixo até o fim e selecione **Experimente grátis** para criar credenciais. Você pode usar esses serviços [gratuitamente por seis meses, depois pague conforme precisar](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) por apenas US$ 0,05 por transação de documento, assim você paga apenas pelo que precisa.

Selecionar **API de serviços PDF** como seu serviço de escolha e preencha os outros detalhes conforme mostrado abaixo.

![Captura de tela de obtenção de credenciais](assets/sales_5.png)

Depois de criar suas credenciais, você obtém alguns exemplos de código. Selecione seu idioma preferido (este tutorial usa Node.js). Suas credenciais de API estão em um arquivo zip. Extraia os arquivos para PDFToolsSDK-Node.jsSamples.

Para iniciar, crie uma pasta vazia chamada autoDoc\*\*.\*\* Na pasta, execute o seguinte comando para inicializar um projeto Node.js: `npm init`. Nomeie seu projeto como &quot;auto-doc&quot;*.*

Na pasta ./PDFToolsSDK-Node.jsSamples/adobe-dc-pdf-tools-sdk-node-samples, há um arquivo chamado pdftools-api-credentials.json. Mova-o e private.key para a pasta autodoc. Ele contém suas credenciais de API. Além disso, na pasta autodoc, crie uma subpasta chamada &quot;recursos&quot;. Ele armazena os dados formatados JSON recebidos dos clientes sempre que você gera uma proposta de vendas. Na mesma pasta, salve o modelo de proposta de vendas do Microsoft Word.

Agora você está pronto para fazer uma mágica! Como você está usando Node.js neste tutorial, é necessário instalar o Node.js [!DNL Acrobat Services] SDK. Para fazer isso, na pasta autodoc, execute yarn add @adobe/documentservices-pdftools-node-sdk.

Agora, crie um arquivo chamado merge.js e cole o seguinte código nele.

```
javascript
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk'),
fs = require('fs');
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Setup input data for the document merge process
const jsonString = fs.readFileSync('resources/Proposal.json'),
jsonDataForMerge = JSON.parse(jsonString);
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile('resources/Proposal.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/Proposal.pdf'))
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation', err);
} else {
console.log('Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
```

Esse código obtém o arquivo JSON do Formulário do Microsoft com a ajuda das tags criadas usando [!DNL Acrobat Services]. Em seguida, os dados são mesclados com o modelo de proposta de vendas criado no Microsoft Word para gerar um PDF novo. O PDF é salvo no arquivo recém-criado ./pasta de saída.

Além disso, o código usa [API do Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html) para que ambas as empresas assinem a proposta de vendas gerada. Confira esta publicação do blog para obter uma explicação detalhada sobre essa API.

## Próximas etapas

Você começou com um processo ineficiente e tedioso que precisava de automação. Você passou da criação manual de documentos para cada cliente à criação de um fluxo de trabalho simplificado para automatizar e simplificar [o processo de proposta de vendas](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

Com o Microsoft Forms, você obtém dados importantes de seus clientes que seriam inseridos em suas propostas exclusivas. Você criou um modelo de proposta de vendas no Microsoft Word para fornecer o texto estático que não deseja recriar toda vez. Você usou [!DNL Acrobat Services] APIs para mesclar dados do formulário e do modelo para criar um PDF de proposta de vendas para seus clientes de maneira mais eficiente.

Este tutorial prático é apenas uma prévia do que é possível fazer com essas APIs. Para descobrir mais soluções, visite o [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) página APIs. Use todas essas ferramentas gratuitamente por seis meses. Em seguida, pague apenas US$ 0,05 por transação de documento no [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) para que você pague apenas à medida que sua equipe adiciona mais clientes potenciais ao seu pipeline de vendas.
