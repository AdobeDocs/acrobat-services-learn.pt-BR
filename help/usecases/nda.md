---
title: Criando um NDA
description: Saiba como criar um PDF NDA dinâmico para colaboração
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8098
thumbnail: KT-8098.jpg
exl-id: f4ec0182-a46e-43aa-aea3-bf1d19f1a4ec
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1072'
ht-degree: 0%

---

# Criando um NDA

![Banner do herói do caso de uso](assets/UseCaseNDAHero.jpg)

As organizações colaboram com colaboradores externos para desenvolver seus serviços e produtos. Um acordo de confidencialidade (NDA) é uma parte importante dessas colaborações. Vincula todas as partes de divulgar qualquer informação confidencial que possa prejudicar qualquer entidade.

O formato NDA mais amplamente utilizado é um documento PDF. As organizações preparam um contrato de não divulgação e o enviam a todas as partes. Então, uma vez que todos assinaram, eles iniciam o contrato. Em uma equipe de alta velocidade, a criação manual de PDF retarda o progresso.

## O que você pode aprender

Este tutorial prático explica como criar um modelo de não divulgação do Microsoft Word especializado para sua empresa. O complemento Adobe gratuito para Microsoft Word, [Marcador de geração de documento Adobe](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo), insere “tags” para inserir os valores dinâmicos. Saiba como passar os dados JSON para o modelo e criar um PDF dinâmico. O PDF resultante pode ser enviado por e-mail ou mostrado aos colaboradores no navegador, dependendo dos requisitos e objetivos da sua empresa. Para acompanhar, basta ter uma pequena experiência com Node.js, JavaScript, Express.js, HTML e CSS.

## APIs e recursos relevantes

Com o [!DNL Adobe Acrobat Services], você pode gerar documentos PDF sem interrupções usando dados dinâmicos. O [!DNL Acrobat Services] oferece um conjunto de ferramentas de PDF, incluindo a API de Geração de Documento de Adobe para automatizar a [criação de NDA](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation).

* [API de Geração de Documento do Adobe](https://developer.adobe.com/document-services/apis/doc-generation)

* [API DO Adobe Sign](https://developer.adobe.com/adobesign-api/)

* [Marcador de Geração de Documento do Adobe](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

* [Código do projeto](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)

* [[!DNL Acrobat Services] chaves](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred)

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

Use essa estrutura dentro do Microsoft Word para gerar um modelo. Esses dados podem vir de qualquer fonte de dados, desde que estejam no formato JSON. Para simplificar, você cria vários arquivos dentro do aplicativo Node.js, mas seu caso de uso pode exigir uma conexão de banco de dados para obter informações do fornecedor.

## Criação do modelo do Microsoft Word

Crie o modelo de não divulgação em um documento do Microsoft Word. A API de serviços do Adobe PDF espera que o documento do Microsoft Word contenha tags nas quais o serviço possa injetar valores de documentos JSON. Embora o modelo seja o mesmo para todas as solicitações ao Adobe, os dados dinâmicos no JSON são alterados. Essas etiquetas ajudam a criar documentos PDF para cada fornecedor nesse caso, usando um único modelo do Microsoft Word e acelerando o processo automatizando a geração de documentos NDA.

Você pode instalar o [complemento Document Generation Tagger](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) gratuito para o Microsoft Word. Se você faz parte de uma organização, pode solicitar ao administrador do Microsoft Office que instale o suplemento gratuito para todos.

Depois de instalar o suplemento, você pode encontrá-lo na guia Início, na categoria Adobe. Para abrir a guia, selecione **Geração de Documento**:

![Captura de tela do suplemento Geração de Documento no Word](assets/nda_1.png)

Dentro da guia, você pode fazer upload do documento JSON de amostra. Este documento pode ser uma amostra porque você só pode usá-lo para criar um modelo do Microsoft Word.

![Captura de tela de dados de exemplo no suplemento Document Generation](assets/nda_2.png)

Selecione **Gerar Marcas** para exibir itens que você pode usar dentro do modelo. Estas são as propriedades extraídas da estrutura JSON, prontas para uso no modelo:

![Captura de tela de tags de texto no suplemento de geração de documento](assets/nda_3.png)

Estes são os recursos do campo `authorizedSigner`. Outros campos são quebrados e você pode expandir a exibição no Microsoft Word. O suplemento também oferece opções avançadas de dados, como tabelas, listas, valores calculados e muito mais.

## Criar as tags

Você pode criar um modelo ou importar um [modelo existente](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade) para o Microsoft Word. Depois de configurar o documento, adicione tags a cada campo clicando nos tokens correspondentes no suplemento.

O seguinte modelo em um arquivo do Microsoft Word:

![Captura de tela do modelo de amostra](assets/nda_4.png)

Esse arquivo contém várias tags. Quando você executa o programa, esses campos são preenchidos com as informações do fornecedor.

O Document Generation Tagger se integra à API do Adobe Sign. Devido a essa integração, você pode criar automaticamente tags de texto do Sign para que o documento gerado possa ser enviado ao Adobe Sign para assinatura.

## Gerar o NDA para fornecedores

Dentro do aplicativo de amostra, você preparou pastas para as entradas e saídas. Como mencionado anteriormente, você usa arquivos JSON, de modo que haja dois arquivos para mostrar os fornecedores disponíveis no sistema. Os arquivos são mostrados dentro de um formulário que é impresso no navegador:

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

Esse código gera a seguinte interface de usuário (UI) no navegador:

![Captura de tela da interface de usuário Criar NDA](assets/nda_5.png)

Quando o administrador seleciona uma pessoa, o aplicativo usa os Serviços da Adobe PDF para gerar o NDA em qualquer lugar.

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

Use esse código dentro do roteador Express:

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

Você pode exibir [o código de exemplo completo](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample) no GitHub.

Este código usa um documento JSON e o modelo do Microsoft Word na chamada da API para o SDK [!DNL Adobe Acrobat Services]. Na resposta, você recebe a saída e a salva no sistema de arquivos do aplicativo. Você pode encaminhar o documento gerado para seus clientes por email ou mostrar uma visualização dentro do navegador usando a [API de incorporação do Adobe PDF](https://developer.adobe.com/document-services/apis/pdf-embed) gratuita.

Esta chamada cria o seguinte documento de não divulgação:

![Captura de tela da visualização do documento não divulgação](assets/nda_6.png)

[!DNL Adobe Acrobat Services] APIs inserem conteúdo para criar um documento PDF. Sem essas ferramentas, talvez seja necessário gravar o código para processar documentos do Office e trabalhar com formatos de arquivo PDF raw. Com a ajuda dos Serviços da Adobe PDF, você pode executar todas essas etapas com uma única chamada de API.

Agora use a [API do Adobe Sign](https://developer.adobe.com/adobesign-api/) para solicitar assinaturas nos NDAs e entregar o documento assinado final a todas as partes. O Adobe Sign notifica você [usando um webhook](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/webhooks.md). Ao ouvir este webhook, você pode obter o status do NDA.

Para obter uma explicação mais detalhada sobre o processo do Adobe Sign, [consulte a documentação](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html) ou leia esta postagem de blog detalhada.

## Próximas etapas

Neste tutorial prático, o Adobe Document Generation Tagger foi usado para gerar dinamicamente documentos PDF usando modelos do Microsoft Word e arquivos de dados JSON. O suplemento ajudou a [criar NDAs](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation) personalizados automaticamente para cada parte e coletar assinaturas usando a API do Sign.

Você pode usar essas técnicas para criar dinamicamente seus próprios NDAs ou outros documentos, liberando o tempo da sua equipe para se concentrar no trabalho produtivo. Explore o [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/apis/pdf-services) para encontrar APIs e SDKs para seu idioma e o tempo de execução de sua escolha para que você possa adicionar funções de PDF diretamente aos seus aplicativos para criar rapidamente documentos de PDF. [Comece](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) com uma avaliação gratuita de seis meses depois
[pague conforme usa](https://developer.adobe.com/document-services/pricing/main) por apenas US$ 0,05 por transação de documento.
