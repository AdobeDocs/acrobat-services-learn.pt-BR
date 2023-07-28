---
title: Gerenciando Propostas de Vendas e Contratos
description: Saiba como criar um fluxo de trabalho eficiente para automatizar e simplificar propostas de vendas
role: Developer
level: Intermediate
type: Tutorial
feature: Use Cases
thumbnail: KT-8099.jpg
jira: KT-8099
exl-id: 219c70de-fec1-4946-b10e-8ab5812562ef
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1395'
ht-degree: 2%

---

# Gerenciamento de propostas de vendas e contratos

![Banner do herói do caso de uso](assets/UseCaseSalesHero.jpg)

As propostas de vendas são o primeiro passo na jornada de um negócio em direção à aquisição de clientes. Como em tudo, as primeiras impressões duram. Portanto, sua primeira interação com os clientes definiu suas expectativas para seus negócios. Sua proposta deve ser concisa, precisa e conveniente.

Os contratos e as propostas contêm diferentes tipos de dados na sua estrutura de documentos. Eles contêm dados dinâmicos (nome do cliente, valor da cotação e assim por diante) e dados estáticos (texto padrão como recursos de firma, perfis de equipe e termos padrão da SOW). A criação de documentos de modelo, como propostas de vendas, geralmente envolve tarefas monótonas, como a substituição manual de detalhes do projeto em um modelo padrão. Neste tutorial, você usa dados dinâmicos e fluxos de trabalho para criar um processo eficiente para [criando propostas de vendas](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

## O que você pode aprender

Neste tutorial prático, aprenda a implementar dados dinâmicos e fluxos de trabalho usando várias ferramentas, as mais importantes das quais são [!DNL Adobe Acrobat Services] APIs. Essas APIs são usadas para tornar propostas de vendas e contratos mais convenientes para você e sua empresa. Este tutorial demonstra técnicas práticas para mostrar como criar, mesclar e exibir documentos PDF automaticamente. Executar essas tarefas manualmente é demorado e tedioso. Ao tirar partido da [!DNL Acrobat Services] APIs, você pode reduzir o tempo gasto nessas tarefas.

## APIs e recursos relevantes

* [Microsoft Word](https://www.office.com/)

* [Node.js](https://nodejs.org/en/)

* [npm](https://www.npmjs.com/get-npm)

* [[!DNL Acrobat Services] APIs](https://www.adobe.io/apis/documentcloud/dcsdk/)

* [API de geração de documento Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [API do Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Adobe Document Generation Tagger](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

## Resolvendo o problema

Agora que você tem as ferramentas instaladas, pode começar a resolver o problema. As propostas têm conteúdo estático e conteúdo dinâmico exclusivos para cada cliente. Os gargalos ocorrem porque os dois tipos de dados são necessários sempre que você faz uma proposta. É demorado inserir o texto estático, então você vai automatizá-lo e lidar apenas manualmente com os dados dinâmicos de cada cliente.

Primeiro, crie um formulário de captura de dados no [Microsoft Forms](https://www.office.com/launch/forms?auth=1) (ou seu criador de formulários preferido). Este formulário é para os dados dinâmicos dos clientes que são adicionados a uma proposta de vendas. Preencha este formulário com perguntas para obter os detalhes necessários dos clientes — por exemplo, nome da empresa, data, endereço, escopo do projeto, preços e comentários adicionais. Para criar o seu próprio, use este [formulário](https://forms.office.com/Pages/ShareFormPage.aspx id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAAN__rtiGj5UNElTR0pCQ09ZNkJRUlowSjVQWDNYUEg2RC4u&amp;sharetoken=1AJeMavBAzzxuISRKmUy). O objetivo é que os clientes em potencial preencham o formulário e exportem suas respostas como arquivos JSON, que são passados para a próxima parte do fluxo de trabalho.

Alguns criadores de formulários só permitem exportar dados como arquivos CSV. Assim, você pode achar útil [converter](http://csvjson.com/csv2json) o arquivo CSV gerado em um arquivo JSON.

Os dados estáticos são reutilizados em todas as propostas de vendas. Portanto, você pode usar um modelo de proposta de vendas no Microsoft Word para fornecer o texto estático. Você pode usar este [modelo](https://1drv.ms/w/s!AiqaN2pp7giKkmhVu2_2pId9MiPa?e=oeqoQ2), mas você pode criar o seu próprio ou usar um [modelo Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html).

Agora, você precisa de algo que tire os dados dinâmicos dos clientes no formato JSON e o texto estático no modelo do Microsoft Word para fazer uma proposta de vendas exclusiva para um cliente. O [!DNL Acrobat Services] As APIs são usadas para mesclar os dois e gerar um PDF que pode ser assinado.

Para fazer isso funcionar, use tags. Tags são strings fáceis de usar que podem representar números, palavras, matrizes ou até mesmo objetos complexos. As tags atuam como um espaço reservado para dados dinâmicos, que nesse caso são os dados do cliente inseridos no formulário. Depois de inserir tags no modelo, é possível mapear os campos de formulário do arquivo JSON para o modelo do Word.

## Uso de tags

Abra o modelo de proposta de vendas e selecione o **Inserir** guia. No menu **Suplementos** grupo, selecione **Obter Suplementos**. Depois, selecione **Suplemento de Geração de Documento do Adobe** para adicioná-lo. Depois de adicionado, você verá o Document Generation Tagger no **Página inicial** na guia **Adobe** grupo.

Na guia **Página inicial** na guia **Adobe** grupo, selecione **Geração de documento** para começar a marcar o documento. Um vídeo útil de demonstração aparece em um painel no lado direito da janela.

![Captura de tela do complemento Document Tagger dentro do Word](assets/sales_1.png)

Selecionar **Começar**. Você será solicitado a fornecer dados de amostra. Cole ou faça upload do arquivo JSON de resposta do formulário conforme mostrado abaixo.

![Captura de tela de como colar o código de amostra](assets/sales_2.png)

Selecionar **Gerar tags** para obter uma lista de campos do arquivo JSON que você colou ou enviou. As tags são exibidas abaixo, na barra lateral direita.

![Captura de tela das tags disponíveis](assets/sales_3.png)

Depois de gerar as tags, você pode inseri-las no documento. Tags são adicionadas ao documento no local do cursor. Como mostrado acima, você deve adicionar o **Escopo do projeto** tag logo abaixo da **Escopo do projeto** subtítulo. Dessa forma, quando um cliente entra no escopo do projeto no formulário, sua resposta vai abaixo do **Escopo do projeto** subtítulo, substituindo a tag que você acabou de adicionar. Após terminar de adicionar marcas, parte do documento deve ter a aparência da captura de tela abaixo.

![Captura de tela da adição de marcas ao documento do Word](assets/sales_4.png)

## Usar as APIs

Acesse o menu [!DNL Acrobat Services] APIs [página inicial](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html). Para começar a usar o [!DNL Acrobat Services] APIs, você precisa de credenciais para seu aplicativo. Desça até o fim e selecione **Iniciar avaliação gratuita** para criar credenciais. Você pode usar esses serviços [grátis por seis meses, depois pague conforme usa](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) por apenas US$ 0,05 por transação de documento, para que você pague apenas pelo que precisar.

Selecionar **API de serviços PDF** como seu serviço de escolha e preencha os outros detalhes conforme mostrado abaixo.

![Captura de tela de como obter credenciais](assets/sales_5.png)

Depois de criar suas credenciais, você obtém alguns exemplos de código. Selecione seu idioma preferido (este tutorial usa Node.js). Suas credenciais de API estão em um arquivo zip. Extraia os arquivos para PDFToolsSDK-Node.jsSamples.

Para começar, crie uma pasta vazia chamada doc automático\*\*.\*\* Na pasta, execute o seguinte comando para inicializar um projeto Node.js: `npm init`. Nomeie seu projeto de “doc automático”*.*

Na pasta ./PDFToolsSDK-Node.jsSamples/adobe-dc-pdf-tools-sdk-node-samples, há um arquivo chamado pdftools-api-credentials.json. Mova-o e private.key para a pasta autodoc. Ele contém suas credenciais de API. Além disso, na pasta autodoc, crie uma subpasta chamada “resources.” Ele armazena os dados formatados em JSON recebidos dos clientes sempre que você gera uma proposta de vendas. Na mesma pasta, salve o modelo de proposta de vendas do Microsoft Word.

Agora você está pronto para fazer alguma mágica! Como você está usando o Node.js neste tutorial, você deve instalar o Node.js [!DNL Acrobat Services] SDK. Para fazer isso, na pasta autodoc, execute yarn add @adobe/documentservices-pdftools-node-sdk.

Agora crie um arquivo chamado merge.js e cole o seguinte código nele.

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

Esse código obtém o arquivo JSON do formulário do Microsoft com a ajuda das tags criadas com o [!DNL Acrobat Services]. Em seguida, ele mescla os dados com o modelo de proposta de vendas criado no Microsoft Word para gerar um novo PDF. O PDF é salvo no arquivo recém-criado .pasta /output.

Além disso, o código usa [API do Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html) para que ambas as empresas assinem a proposta de vendas gerada. Confira esta publicação do blog para obter uma explicação detalhada desta API.

## Próximas etapas

Você começou com um processo ineficiente e tedioso que precisava de automação. Você passou de criar documentos manualmente para cada cliente a criar um fluxo de trabalho simplificado para automatizar e simplificar [o processo de proposta de vendas](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

Com o Microsoft Forms, você obtém dados importantes de seus clientes que incluiriam suas propostas exclusivas. Você criou um modelo de proposta de vendas no Microsoft Word para fornecer o texto estático que não deseja recriar todas as vezes. Você usou [!DNL Acrobat Services] APIs para mesclar dados do formulário e do modelo para criar um PDF de proposta de vendas para seus clientes de uma maneira mais eficiente.

Este tutorial prático é apenas uma amostra do que é possível com essas APIs. Para descobrir mais soluções, acesse [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) página APIs. Use todas essas ferramentas gratuitamente por seis meses. Depois, pague apenas US$ 0,05 por transação de documento no [pré-pago](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) pague apenas à medida que sua equipe adicionar mais clientes potenciais ao seu pipeline de vendas.
