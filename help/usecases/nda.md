---
title: Criando um NDA
description: Saiba como criar um PDF de não divulgação dinâmico para colaboração
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8098.jpg
kt: 8098
exl-id: f4ec0182-a46e-43aa-aea3-bf1d19f1a4ec
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1164'
ht-degree: 3%

---

# Criando um NDA

![Use Case Hero Banner](assets/UseCaseNDAHero.jpg)

As organizações colaboram com colaboradores externos para criar seus serviços e produtos. Um contrato de confidencialidade (NDA) é uma parte importante dessas colaborações. Vincula todas as partes de divulgarem quaisquer informações confidenciais que possam prejudicar qualquer uma das entidades.

O formato NDA mais usado é um documento PDF. As organizações preparam um contrato de confidencialidade e o enviam a todas as partes. Depois, depois de todos terem assinado, iniciam o contrato. Em uma equipe de alta velocidade, a criação manual de PDF atrasa o progresso.

## O que você pode aprender

Este tutorial prático explica como criar um modelo de NDA do Microsoft Word especializado para sua empresa. Suplemento sem Adobe para Microsoft Word, [Marcador de Geração de Documento Adobe](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo), insere &quot;tags&quot; para inserir os valores dinâmicos. Saiba como passar os dados JSON para o modelo e criar um PDF dinâmico. O PDF resultante pode ser enviado por email ou mostrado aos colaboradores no navegador, dependendo dos requisitos e objetivos da empresa. Para acompanhar, você só precisa de uma pequena experiência com Node.js, JavaScript, Express.js, HTML e CSS.

## APIs e recursos relevantes

Com [!DNL Adobe Acrobat Services], você pode gerar documentos PDF em tempo real usando dados dinâmicos. [!DNL Acrobat Services] oferece um conjunto de ferramentas PDF, incluindo a API de geração de documentos Adobe para automatizar [Criação de NDA](https://www.adobe.io/apis/documentcloud/dcsdk/nda-creation.html).

* [API de geração de documento Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [API do Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Marcador de Geração de Documento Adobe](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

* [Código do projeto](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)

* [[!DNL Acrobat Services] teclas](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred)

## Criando o modelo JSON

O modelo do Microsoft Word depende do modelo JSON, então você o cria primeiro. Para o tutorial, use uma estrutura JSON básica que contenha detalhes da empresa, como informações de contato.

```
{
"vendor": {
"companyName": "GlobalCorp",
"street": "123 Any Street",
"street2": "",
"city":"Anywhere",
"state":"CA",
"primaryContact": {
"firstName":"John",
"lastName":"Doe",
"email":"john-doe@example.com",
"phone":"123-456-7890"
}
},
"authorizedSigner": {
"firstName": "Sarah",
"lastName": "Rose",
"email": "sarah@example.com",
"phone":"555-555-1234"
}
}
```

Use essa estrutura dentro do Microsoft Word para gerar um modelo. Esses dados podem vir de qualquer fonte de dados, desde que estejam no formato JSON. Para simplificar, você cria vários arquivos dentro do aplicativo Node.js, mas seu caso de uso pode exigir uma conexão de banco de dados para extrair informações do fornecedor.

## Criação do modelo do Microsoft Word

Crie o modelo de não divulgação em um documento do Microsoft Word. A API de serviços do Adobe PDF espera que o documento do Microsoft Word contenha marcas nas quais o serviço possa injetar valores de documentos JSON. Embora o modelo seja o mesmo para todas as solicitações para Adobe, os dados dinâmicos em JSON são alterados. Nesse caso, essas tags ajudam a criar documentos PDF para cada fornecedor, usando um único modelo do Microsoft Word e agilizando o processo automatizando a geração de documentos NDA.

Você pode instalar o [suplemento gratuito do Marcador de Geração de Documentos](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) para o Microsoft Word. Se você faz parte de uma organização, pode solicitar ao administrador do Microsoft Office que instale o suplemento gratuito para todos.

Depois de instalar o suplemento, você pode encontrá-lo na guia Início na categoria Adobe. Para abrir a guia, selecione **Geração de documento**:

![Captura de tela do suplemento Geração de documento no Word](assets/nda_1.png)

Dentro da guia, você pode carregar o documento JSON de amostra. Este documento pode ser um exemplo porque você só o usa para criar um modelo do Microsoft Word.

![Captura de tela de dados de amostra no suplemento Geração de documento](assets/nda_2.png)

Selecionar **Gerar marcas** para exibir itens que você pode usar dentro do modelo. Aqui estão as propriedades extraídas da estrutura JSON, prontas para uso no modelo:

![Captura de tela de tags de texto no suplemento de geração de documento](assets/nda_3.png)

Esses são os recursos do `authorizedSigner` campo. Outros campos são dispostos e você pode expandir a exibição no Microsoft Word. O suplemento também oferece opções de dados avançadas, como tabelas, listas, valores calculados e muito mais.

## Criação das tags

Fique à vontade para criar um modelo ou importar um [modelo existente](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html#sample-blade) no Microsoft Word. Depois de configurar o documento, adicione marcas a cada campo clicando nos tokens correspondentes no suplemento.

O seguinte modelo em um arquivo do Microsoft Word:

![Captura de tela do modelo de amostra](assets/nda_4.png)

Este arquivo contém várias marcas. Quando você executa o programa, esses campos são preenchidos com as informações do fornecedor.

O Marcador de geração de documento se integra à API do Adobe Sign. Devido a essa integração, você pode criar automaticamente tags de texto do Sign para que o documento gerado possa ser enviado para assinatura na Adobe Sign.

## Gerando o contrato de confidencialidade para fornecedores

Dentro do aplicativo de amostra, você preparou pastas para as entradas e saídas. Como mencionado anteriormente, você usa arquivos JSON, para que haja dois arquivos para mostrar os fornecedores disponíveis no sistema. Os arquivos são mostrados dentro de um formulário impresso no navegador:

```
<h1><b>NDA</b>: Generate for vendor.</h1>
<hr />
<p>Following ({{files.length}}) vendors are ready, select to generate NDA and deliver for signature:</p>
<form method="POST">
<ul>
{{#each files }}
<li><input type="checkbox" name="vendor" value="{{this}}" id="file-{{@index}}" /> <label for="file-{{@index}}">{{this}}</label></li>
{{/each}}
</ul>
<input type="submit" value="Create NDA" />
</form>
```

Esse código gera a seguinte interface do usuário (UI) no navegador:

![Captura de tela da interface de usuário Criar NDA](assets/nda_5.png)

Quando o administrador seleciona uma pessoa, o aplicativo usa os Serviços da Adobe PDF para gerar o contrato de confidencialidade em qualquer lugar.

```
async function compileDocFile(json, inputFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
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

Use este código dentro do roteador Express:

```
// Create one report and send it back
try {
console.log(`[INFO] generating the report...`);
const fileContent = fs.readFileSync(`./public/documents/raw/${vendor}`, 'utf-8');
const parsedObject = JSON.parse(fileContent);
await pdf.compileDocFile(parsedObject, `./public/documents/template/Adobe-NDA-Sample.docx`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("preview", { page: 'nda', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

Você pode exibir [o código de amostra completo](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample) no GitHub.

Esse código usa um documento JSON e o modelo do Microsoft Word na chamada de API para o [!DNL Adobe Acrobat Services] SDK. Na resposta, você recebe a saída e a salva no sistema de arquivos do aplicativo. Você pode encaminhar o documento gerado para seus clientes por email ou mostrar uma visualização dentro do navegador usando o [API incorporada do Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

Esta chamada cria o seguinte documento NDA:

![Captura de tela da visualização do documento NDA](assets/nda_6.png)

[!DNL Adobe Acrobat Services] As APIs inserem conteúdo para criar um documento PDF. Sem essas ferramentas, talvez seja necessário escrever o código para processar documentos do Office e trabalhar com formatos de arquivo PDF raw. Com a ajuda dos serviços da Adobe PDF, você pode executar todas essas etapas com uma única chamada de API.

Agora use [API do Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html) para solicitar assinaturas nos contratos de confidencialidade e entregar o documento final assinado a todas as partes. O Adobe Sign notifica você [usando um Webhook](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md). Ao ouvir este webhook, você pode obter o status do contrato de não divulgação.

Para obter uma explicação mais detalhada do processo do Adobe Sign, [consulte a documentação](https://www.adobe.io/apis/documentcloud/sign/docs.html) ou leia esta publicação do blog detalhada.

## Próximas etapas

Neste tutorial prático, o Marcador de geração de documento Adobe foi usado para gerar dinamicamente documentos PDF usando modelos do Microsoft Word e arquivos de dados JSON. O suplemento ajudou a [criar NDAs automaticamente](https://www.adobe.io/apis/documentcloud/dcsdk/nda-creation.html) personalizado para cada parte e colete assinaturas usando a API do Sign.

Você pode usar essas técnicas para criar dinamicamente seus próprios contratos de confidencialidade ou outros documentos, liberando o tempo de sua equipe para se concentrar em trabalhos produtivos. Explorar [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) para encontrar APIs e SDKs para seu idioma e tempo de execução de sua escolha, de modo que você possa adicionar funções PDF diretamente aos seus aplicativos para criar rapidamente documentos PDF. [Começar](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) com uma teste grátis de seis meses,
[pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) por apenas US$ 0,05 por transação de documento.
