---
title: Controle de NFFs
description: Saiba como gerar, proteger e entregar faturas de clientes automaticamente
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8145.jpg
jira: KT-8145
exl-id: 5871ef8d-be9c-459f-9660-e2c9230a6ceb
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1427'
ht-degree: 1%

---

# Controle de faturas

![Use Case Hero Banner](assets/UseCaseInvoicesHero.jpg)

É ótimo quando os negócios estão crescendo, mas a produtividade é prejudicada quando chega a hora de preparar todas essas faturas. Gerar faturas manualmente é demorado, além de você correr o risco de cometer um erro, potencialmente perder dinheiro ou enfurecer um cliente com uma quantia incorreta.

Pense em Danielle, por exemplo, trabalhando no [departamento de contabilidade](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html) [de uma empresa de assistência médica](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html). É o fim do mês, então ela está extraindo informações de vários sistemas diferentes, verificando sua precisão e formatando as faturas. Depois de todo esse trabalho, ela finalmente está pronta para converter os documentos em PDF (para que qualquer pessoa possa visualizá-los sem comprar um software específico) e enviar a cada cliente sua fatura personalizada.

Mesmo quando o faturamento mensal está completo, Danielle não pode escapar dessas faturas. Alguns clientes têm ciclos de faturamento não mensais, por isso ela está sempre criando uma fatura para alguém. Ocasionalmente, um cliente edita sua fatura e paga menos. Danielle, então, gasta tempo solucionando esta incompatibilidade de fatura. Nesse ritmo, ela precisa contratar um assistente para acompanhar todo o trabalho!

O que Danielle precisa é de uma forma de gerar faturas com rapidez e precisão, tanto em lote no final do mês como ad hoc em outros momentos. O ideal é que, se ela pudesse proteger essas faturas de edições, não precisaria se preocupar em solucionar problemas de valores incompatíveis.

## O que você pode aprender

Neste tutorial prático, aprenda a usar a API de geração de documentos do Adobe para gerar faturas automaticamente, proteger os PDF por senha e entregar uma fatura para cada cliente. Basta conhecer Node.js, JavaScript, Express.js, HTML e CSS.

O código completo deste projeto é [disponível no GitHub](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation). Você deve configurar o diretório público com seu modelo e as pastas de dados brutos. Em produção, você deve buscar os dados de uma API externa. Você também pode explorar esta versão arquivada do aplicativo que contém os recursos do modelo.

## APIs e recursos relevantes

* [API de serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API de geração de documento Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [API do Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Código do projeto](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)

## Preparando os dados

Este tutorial não mostra como os dados são importados de seus data warehouses. Seus pedidos de clientes podem estar em um banco de dados, API externa ou software personalizado. A API de geração de documentos do Adobe espera um documento JSON contendo os dados de faturamento, como informações do gerenciamento de relacionamento com o cliente (CRM) ou da plataforma de comércio eletrônico. Este tutorial pressupõe que os dados já estão no formato JSON.

Para simplificar, use a seguinte estrutura JSON para faturamento:

```
{ 
    "customerName": "John Doe", 
    "customerEmail": "john-doe@example.com", 
    "order": [ 
        { 
            "productId": 26, 
            "productTitle": "Bandages", 
            "price": 15.82 
        }, 
        { 
            "productId": 54, 
            "productTitle": "Masks", 
            "price": 25 
        }, 
        { 
            "productId": 76, 
            "productTitle": "Gloves", 
            "price": 7.59 
        } 
    ] 
} 
```

O documento JSON contém os detalhes do cliente, bem como as informações do pedido. Use este documento estruturado para elaborar sua fatura e exibir os elementos no formato PDF.

## Criando um modelo de fatura

A API de geração de documento do Adobe espera que um modelo baseado no Microsoft Word e um documento JSON criem um documento PDF ou Word dinâmico. Crie um modelo do Microsoft Word para o aplicativo de faturamento e use o [suplemento gratuito do Marcador de Geração de Documentos](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) para gerar as tags de modelo. Instale o suplemento e abra a guia no Microsoft Word.

![Captura de tela do suplemento Marcador de Geração de Documento](assets/invoices_1.png)

Depois de colar o conteúdo JSON no suplemento, conforme mostrado acima, clique em Gerar tags. Agora, este plug- in mostra o formato do seu objeto. Seu modelo básico pode usar o nome e o email do cliente, mas não mostra as informações do pedido. As informações da ordem são discutidas posteriormente neste tutorial.

![Captura de tela do modelo de Autor do Marcador de Geração de Documento](assets/invoices_2.png)

Em seu documento do Microsoft Word, comece a escrever o modelo de fatura. Deixe o cursor onde você deve inserir dados dinâmicos e, em seguida, selecione a tag na janela do suplemento Adobe. Clique em **Inserir texto** para que o suplemento Adobe Document Generation Tagger possa gerar e inserir as tags. Para personalização, vamos inserir o nome e o email do cliente.

Agora, passe para os dados que mudam a cada nova fatura. Selecione o **Avançado** do suplemento. Para ver as opções disponíveis para gerar uma tabela dinâmica com base nos produtos solicitados pelo cliente, clique em **Tabelas e Listas** .

Selecionar **Pedido** na primeira lista suspensa. Na segunda lista suspensa, selecione as colunas desta tabela. Neste tutorial, selecione as três colunas do objeto para renderizar a tabela.

![Captura de tela da guia Avançado do Marcador de Geração de Documento](assets/invoices_3.png)

A API de geração de documento também pode executar operações complexas, como agregar elementos dentro de uma matriz. No menu **Avançado** , selecione **Cálculos numéricos** e no **Agregação** , selecione o campo onde deseja aplicar o cálculo.

![Captura de tela dos cálculos numéricos do Marcador de geração de documento](assets/invoices_4.png)

Clique no botão **Inserir Cálculo** para inserir essa tag onde necessário dentro do documento. O seguinte texto agora é exibido no arquivo do Microsoft Word:

![Captura de tela de tags em um documento do Microsoft Word](assets/invoices_5.png)

Esta amostra de fatura contém informações do cliente, os produtos solicitados e o valor total devido.

## Gerando uma NFF usando a API de Geração de Documento Adobe

Use o kit de desenvolvimento de software (SDK) Node.js dos Serviços Adobe PDF para combinar os documentos do Microsoft Word e JSON. Crie um aplicativo Node.js para criar a fatura usando a API de geração de documento.

A API de serviços do PDF inclui o Serviço de geração de documentos, para que você possa usar as mesmas credenciais para ambos. Aproveite um [teste grátis de seis meses](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)e pague apenas US$ 0,05 por transação de documento.

Aqui está o código para mesclar o PDF:

```
async function compileDocFile(json, inputFile, outputPdf) { 
    try { 
        // configurations 
        const credentials =  adobe.Credentials 
            .serviceAccountCredentialsBuilder() 
            .fromFile("./src/pdftools-api-credentials.json") 
            .build(); 

        // Capture the credential from app and show create the context 
        const executionContext = adobe.ExecutionContext.create(credentials); 
  
        // create the operation 
        const documentMerge = adobe.DocumentMerge, 
            documentMergeOptions = documentMerge.options, 
            options = new documentMergeOptions.DocumentMergeOptions(json, documentMergeOptions.OutputFormat.PDF);

        const operation = documentMerge.Operation.createNew(options); 
  
        // Pass the content as input (stream) 
        const input = adobe.FileRef.createFromLocalFile(inputFile); 
        operation.setInput(input); 
  
        // Async create the PDF 
        let result = await operation.execute(executionContext); 
        await result.saveAsFile(outputPdf); 
    } catch (err) { 
        console.log('Exception encountered while executing operation', err); 
    } 
} 
```

Esse código obtém informações do documento JSON de entrada e do arquivo de modelo de entrada. Em seguida, ele cria uma operação de mesclagem de documento para combinar os arquivos em um único relatório PDF. Finalmente, ele executa a operação com suas credenciais de API. Se você ainda não os tiver, [criar credenciais](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getting-credentials) (A Geração de documento e a API de serviços PDF usam as mesmas credenciais).

Use esse código no roteador Express para processar a solicitação de documento:

```
// Create one report and send it back
try {
    console.log(\`[INFO] generating the report...\`);
    const fileContent = fs.readFileSync(\`./public/documents/raw/\${vendor}\`,
    'utf-8');
    const parsedObject = JSON.parse(fileContent);

    await pdf.compileDocFile(parsedObject,
    \`./public/documents/template/Adobe-Invoice-Sample.docx\`,
    \`./public/documents/processed/output.pdf\`);

    await pdf.applyPassword("p@55w0rd", './public/documents/processed/output.pdf',
    './public/documents/processed/output-secured.pdf');

    console.log(\`[INFO] sending the report...\`);
    res.status(200).render("preview", { page: 'invoice', filename: 'output.pdf' });
} catch(error) {
    console.log(\`[ERROR] \${JSON.stringify(error)}\`);
    res.status(500).render("crash", { error: error });
}
```

Quando esse código é executado, ele fornece um documento PDF contendo a fatura gerada dinamicamente com base nos dados fornecidos. Com os dados JSON de amostra (fornecidos acima), a saída desse código é:

![Captura de tela da fatura PDF gerada dinamicamente](assets/invoices_6.png)

Esta fatura inclui seus dados dinâmicos do documento JSON.

## Proteger faturas por senha

Como Danielle, a contadora, está preocupada com a alteração da fatura pelos clientes, aplique uma senha para restringir a edição. [API de serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html) O pode aplicar automaticamente uma senha aos documentos. Aqui, você usa o Adobe PDF Services SDK para proteger os documentos com uma senha. O código é:

```
async function applyPassword(password, inputFile, outputFile) {
    try {
        // Initial setup, create credentials instance.
        const credentials = adobe.Credentials
        .serviceAccountCredentialsBuilder()
        .fromFile("./src/pdftools-api-credentials.json")
        .build();

        // Create an ExecutionContext using credentials
        const executionContext = adobe.ExecutionContext.create(credentials);
        // Create new permissions instance and add the required permissions
        const protectPDF = adobe.ProtectPDF,
        protectPDFOptions = protectPDF.options;
        // Build ProtectPDF options by setting an Owner/Permissions Password, Permissions,
        // Encryption Algorithm (used for encrypting the PDF file) and specifying the type of content to encrypt.
        const options = new protectPDFOptions.PasswordProtectOptions.Builder()
        .setOwnerPassword(password)
        .setEncryptionAlgorithm(protectPDFOptions.EncryptionAlgorithm.AES_256)
        .build();

        // Create a new operation instance.
        const protectPDFOperation = protectPDF.Operation.createNew(options);

        // Set operation input from a source file.
        const input = adobe.FileRef.createFromLocalFile(inputFile);
        protectPDFOperation.setInput(input);

        // Execute the operation and Save the result to the specified location.
        let result = await protectPDFOperation.execute(executionContext);

        result.saveAsFile(outputFile);
    } catch (err) {
        console.log('Exception encountered while executing operation', err);
    }
}
```

Quando você usa esse código, ele protege seu documento com uma senha e carrega uma nova fatura no sistema. Para saber mais sobre como esse código é usado ou para testá-lo, consulte o [amostra de código](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation).

Quando terminar de usar a fatura, convém enviá-la automaticamente por email ao cliente. Há algumas maneiras de realizar automaticamente o envio de e-mails aos seus clientes. A maneira mais rápida é usar uma API de email de terceiros junto com uma biblioteca de ajuda como [sendgrid-nodejs](https://github.com/sendgrid/sendgrid-nodejs). Como alternativa, se você já tiver acesso a um servidor SMTP, poderá usar [nodemailer](https://www.npmjs.com/package/nodemailer) para enviar e-mails via SMTP.

## Próximas etapas

Neste tutorial prático, você criou um aplicativo simples para ajudar Danielle na contabilidade com [faturamento](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html). Usando a API de serviços do PDF e o SDK de geração de documentos, você preencheu um modelo do Microsoft Word com informações de ordem do cliente de um documento JSON, criando uma fatura PDF. Em seguida, cada documento protegido por senha usando serviços de proteção por senha por [API de serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html).

Como Danielle pode gerar faturas automaticamente e não precisa se preocupar com clientes editando suas faturas, ela não precisará contratar um assistente para ajudar com todo o trabalho manual. Ela pode usar seu tempo extra para encontrar economia de custos nos arquivos de contas a pagar.

Agora que você viu como é fácil, expanda esse aplicativo simples usando outras ferramentas do Adobe para incorporar faturas em seu site. Por exemplo, para que os clientes possam exibir suas faturas ou saldo a qualquer momento. [API incorporada do Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) é livre para usar. Você também pode acessar o departamento de recursos humanos ou de vendas, ajudando a automatizar os contratos e a coletar assinaturas eletrônicas.

Para explorar todas as possibilidades e começar a criar seu próprio aplicativo prático, crie um [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) conta para começar hoje. Aproveite uma teste grátis de seis meses e [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)
por apenas US$ 0,05 por transação de documento, à medida que sua empresa cresce.
