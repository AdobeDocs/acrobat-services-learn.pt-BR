---
title: Fluxos de trabalho do contrato no Node.js
description: “[!DNL Adobe Acrobat Services] APIs incorporam facilmente recursos de PDF em seus aplicativos Web”
feature: Use Cases
role: Developer
level: Beginner
type: Tutorial
jira: KT-7473
thumbnail: KT-7473.jpg
keywords: Destacado
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: f8a31b8f98d99bf1f3787e0f0f19cc9f26e24d8d
workflow-type: tm+mt
source-wordcount: '2094'
ht-degree: 0%

---

# Fluxos de trabalho do contrato no Node.js

![Banner do herói do caso de uso](assets/UseCaseAgreementHero.jpg)

Muitos aplicativos e processos de negócios exigem documentação, como propostas e contratos. Os documentos PDF garantem que os arquivos sejam mais seguros e menos modificáveis. Eles também oferecem suporte à assinatura digital para que seus clientes possam preencher seus documentos de forma rápida e fácil. As APIs do [!DNL Adobe Acrobat Services] incorporam facilmente recursos de PDF em seus aplicativos Web.

## O que você pode aprender

Neste tutorial prático, saiba como adicionar serviços PDF a um aplicativo Node.js para digitalizar um processo de contrato.

## APIs e recursos relevantes

* [API de Serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API de inserção de PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API DO Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Código do projeto](https://github.com/adobe/pdftools-node-sdk-samples)

## Configurando [!DNL Adobe Acrobat Services]

Para começar, configure credenciais para usar [!DNL Adobe Acrobat Services]. Registre uma conta e use o [Node.js Quickstart](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js) para verificar se suas credenciais funcionam antes de integrar a funcionalidade em um aplicativo maior.

Primeiro, obtenha uma conta de desenvolvedor de Adobe. Em seguida, na página [Introdução](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK), selecione a opção *Introdução* em Criar Novas Credenciais. Você pode se inscrever para a avaliação gratuita que fornece 1.000 transações de documentos que podem ser usadas durante seis meses.

![Imagem de Criação de Novas Credenciais](assets/AWNjs_1.png)

Na página Criar novas credenciais a seguir, você será solicitado a decidir entre a API de incorporação de PDF e a API de serviços de PDF.

Selecione *API de Serviços PDF*.

Insira um nome para o aplicativo e marque a caixa denominada *Criar amostra de código personalizada*. Marcar essa caixa incorpora automaticamente suas credenciais na amostra de código. Se você deixar esta caixa desmarcada, adicione manualmente suas credenciais ao aplicativo.

Selecione *Node.js* para o tipo de aplicativo e clique em *Criar Credenciais*.

Alguns momentos depois, um arquivo .zip começa a ser baixado com um projeto de amostra que inclui suas credenciais. O pacote Node.js para [!DNL Acrobat Services] já está incluído como parte do código de projeto de amostra.

![Imagem de Seleção de Credenciais de API dos Serviços PDF](assets/AWNjs_2.png)

## Configurando o projeto de amostra manualmente

Se você optar por não baixar um projeto de amostra da página Criar novas credenciais, também poderá configurar o projeto manualmente.

Baixe o código (sem suas credenciais inseridas) de [GitHub](https://github.com/adobe/pdftools-node-sdk-samples). Se usar esta versão do código, você deve adicionar suas credenciais ao arquivo pdftools-api-credentials.json antes de usar:

```
{
  "client_credentials": {
    "client_id": "<client_id>",
    "client_secret": "<client_secret>"
  },
  "service_account_credentials": {
    "organization_id": "<organization_id>",
    "account_id": "<technical_account_id>",
    "private_key_file": "<private_key_file_path>"
  }
}
```

Para seu próprio aplicativo, você precisa copiar o arquivo de chave privada e os arquivos de credenciais para a origem do aplicativo.

Você deve instalar o pacote Node.js para [!DNL Acrobat Services]. Para instalar o pacote, use o seguinte comando:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## Configurando o registro

As amostras aqui usam Express para a estrutura do aplicativo. Eles também usam log4js para o registro de aplicativos. Com o log4js, você pode facilmente direcionar o log para o console ou para um arquivo:

```
const log4js = require('log4js');
const logger = log4js.getLogger();
log4js.configure( {
    appenders: { fileAppender: { type:'file', filename: './logs/applicationlog.txt'}},
    categories: { default: {appenders: ['fileAppender'], level:'info'}}
});
 
logger.level = 'info';
logger.info('Application started')
```

O código acima grava os dados registrados em um arquivo em ./logs/applicationlog.txt. Se quiser que ele grave no console, você pode comentar a chamada para log4js.configure.

## Convertendo arquivos do Word em PDF

Contratos e propostas geralmente são escritos em um aplicativo de produtividade, como o Microsoft Word. Para aceitar documentos nesse formato e converter o documento em PDF, você pode adicionar funcionalidades ao aplicativo. Vamos ver como fazer upload e salvar um documento em um aplicativo expresso e salvá-lo no sistema de arquivos.

No HTML do aplicativo, adicione um elemento de arquivo e um botão para iniciar o upload:

```
<input type="file" name="source" id="source" />
<button onclick="upload()" >Upload</button>
```

No JavaScript da página, faça upload do arquivo de forma assíncrona usando a função fetch:

```
function upload()
{
  let formData = new FormData();
  var selectedFile = document.getElementById('source').files[0];
  formData.append("source", selectedFile);
  fetch('documentUpload', {method:"POST", body:formData});
}
```

Escolha uma pasta para aceitar os arquivos carregados. O aplicativo precisa de um caminho para esta pasta. Localize o caminho absoluto usando um caminho relativo unido a \_\_dirname:

```
const uploadFolder = path.join(__dirname, "../uploads");
```

Como o arquivo é enviado via post, você deve responder a uma mensagem de post no lado do servidor:

```
router.post('/', (req, res, next) => {
  console.log('uploading')
  if(!req.files || Object.keys(req.files).length === 0) {
  return res.status(400).send('No files were uploaded');
  }
    
  const uploadPath = path.join(uploadFolder, req.files.source.name);
  var buffer = req.files.source.data;
  var result = {"success":true};
  fs.writeFile(uploadPath, buffer, 'binary', (err)=> {
    if(err) {
      result.success = false;
    }
    res.json(result);
  });       
});
```

Depois que essa função é executada, o arquivo é salvo na pasta de upload de aplicativos e fica disponível para processamento adicional.

Em seguida, converta o arquivo de seu formato nativo em PDF. O código de exemplo que você baixou anteriormente contém um script chamado `create-pdf-from-docx.js` para converter um documento em PDF. A seguinte função, `convertDocumentToPDF`, pega um documento carregado e o converte em um PDF em uma pasta diferente:

```
function convertDocumentToPDF(sourcePath, destinationPath)
{    
  try {   
    const credentials = PDFToolsSDK.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdftools-api-credentials.json")
    .build();
 
    const executionContext = 
      PDFToolsSDK.ExecutionContext.create(credentials),
    createPdfOperation = PDFToolsSDK.CreatePDF.Operation.createNew();
 
    const docxReadableStream = fs.createReadStream(sourcePath);
    const input = PDFToolsSDK.FileRef.createFromStream(
      docxReadableStream, 
      PDFToolsSDK.CreatePDF.SupportedSourceFormat.docx);
    createPdfOperation.setInput(input);
 
    createPdfOperation.execute(executionContext)
    .then(result => result.saveAsFile(destinationPath))
    .catch(err => {        
      logger.erorr('Exception encountered while executing operation');        
    })
  }
  catch(err) {        
    logger.error(err);
  }
}
```

Você pode notar um padrão geral com o código:

O código constrói um objeto de credenciais e um contexto de execução, inicializa alguma operação e, em seguida, executa a operação com o contexto de execução. Você pode ver esse padrão em todo o código de amostra.

Ao fazer algumas adições à função de upload para que ela chame essa função, os documentos do Word que os usuários carregam agora são automaticamente convertidos para PDF.

O código a seguir cria o caminho de destino para o PDF convertido e inicia a conversão:

```
const documentFolder = path.join(__dirname, "../docs");
var extPosition = req.files.source.name.lastIndexOf('.') - 1;
if(extPosition < 0 ) {
  extPosition = req.files.source.name.length
}
const destinationName = path.join(documentFolder,  
  req.files.source.name.substring(0, extPosition) + '.pdf');
console.log(destinationName);
 
logger.info('converting to ${destinationName}')
  convertDocumentToPDF(uploadPath, destinationName);
```

## Convertendo outros tipos de arquivo em PDF

O kit de ferramentas de documento converte outros formatos em PDF, como HTML estático, outro tipo de documento comum. O Toolkit aceita documentos HTML empacotados como um arquivo .zip com todos os recursos referenciados pelo documento (arquivos CSS, imagens e outros arquivos) no mesmo arquivo .zip. O documento HTML em si deve ser nomeado como index.html e colocado na raiz do arquivo .zip.

Para converter um arquivo .zip contendo HTML, use o seguinte código:

```
//Create an HTML to PDF operation and provide the source file to it
htmlToPDFOperation = PDFToolsSdk.CreatePDF.Operation.createNew();     
const input = PDFToolsSdk.FileRef.createFromLocalFile(sourceZipFile);
htmlToPDFOperation.setInput(input);
 
// custom function for setting options
setCustomOptions(htmlToPDFOperation);
 
// Execute the operation and Save the result to the specified location.
htmlToPDFOperation.execute(executionContext)
  .then(result => result.saveAsFile(destinationPdfFile))
  .catch(err => {
    logger.error('Exception encountered while executing operation');
});
```

A função `setCustomOptions` especifica outras configurações de PDF, como o tamanho da página. Aqui, você pode ver a função define o tamanho da página para 11,5 por 11 polegadas:

```
const setCustomOptions = (htmlToPDFOperation) => {    
  const pageLayout = new PDFToolsSdk.CreatePDF.options.PageLayout();
  pageLayout.setPageSize(11.5, 8);

  const htmlToPdfOptions = 
    new PDFToolsSdk.CreatePDF.options.html.CreatePDFFromHtmlOptions.Builder()
    .includesHeaderFooter(true)
    .withPageLayout(pageLayout)
    .build();
  htmlToPDFOperation.setOptions(htmlToPdfOptions);
};
```

Ao abrir um documento HTML contendo alguns termos, você obtém o seguinte no navegador:

![Imagem de Termos do Computador](assets/AWNjs_3.png)

A origem deste documento é composta por um arquivo CSS e um arquivo HTML:

![Imagem de CSS e arquivo HTML](assets/AWNjs_4.png)

Após processar o arquivo HTML, você terá o mesmo texto no formato PDF:

![Arquivo PDF de Termos do Computador](assets/AWNjs_5.png)

## Acrescentar páginas

Outra operação comum com arquivos PDF é acrescentar páginas ao final que podem ter texto padrão, como uma lista de termos. O kit de ferramentas de documentos pode combinar vários documentos PDF em um único documento. Se você tiver uma lista de caminhos de documento (aqui em `sourceFileList`), poderá adicionar as referências de arquivo de cada arquivo a um objeto para uma operação de combinação.

Quando a operação de combinação é executada, ela fornece um único arquivo com do conteúdo de origem. Você pode usar `saveAsFile` no objeto para manter o arquivo no armazenamento.

```
const executionContext = PDFToolsSDK.ExecutionContext.create(credentials);
var combineOperation = PDFToolsSDK.CombineFiles.Operation.createNew();
 
sourceFileList.forEach(f => {
  var combinedSource = PDFToolsSDK.FileRef.createFromLocalFile(f);
  console.log(f);
  combineOperation.addInput(combinedSource);
});
    
 
combineOperation.execute(executionContext)
  .then(result=>result.saveAsFile(destinationFile))
  .catch(err => {
    logger.error(err.message);
});    
```

## Exibição de documentos PDF

Você executou várias operações em arquivos PDF, mas no final das contas, o usuário deve visualizar os documentos. É possível incorporar o documento em uma página da Web usando a API incorporada Adobe PDF.

Na página que exibe o PDF, adicione um elemento `<div />` para manter o documento e forneça uma ID a ele. Use essa ID em breve. Na página da Web, inclua uma referência `<script />` à biblioteca Adobe JavaScript:

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

O último bit de código necessário é uma função que exibe o documento assim que o Adobe PDF Embed API JavaScript é carregado. Quando você receber a notificação de que o script foi carregado por meio de um evento adobe_dc_view\_sdk.ready, crie um novo objeto AdobeDC.View. Este objeto precisa da ID do cliente e da ID do elemento criado anteriormente. Localize sua ID de cliente no [Console do Adobe Developer](https://console.adobe.io/). Ao exibir as configurações do aplicativo que você criou ao gerar credenciais anteriormente, a ID do cliente é exibida.

![Imagem da Chave de Cliente de API](assets/AWNjs_6.png)

## Outras opções de PDF

A [Demonstração da API de Incorporação do Adobe PDF](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) permite visualizar as várias outras opções de incorporação de documentos PDF.

![Imagem de opções de PDF de incorporação ](assets/AWNjs_7.png)

É possível ativar e desativar várias opções e ver imediatamente como elas são renderizadas. Quando encontrar uma combinação de que você goste, clique no botão *\&lt;/\> Gerar Código* para gerar o código HTML real usando essas opções.

![Imagem de Visualização de Código](assets/AWNjs_8.png)

## Adição de assinaturas digitais e segurança

Assim que o documento estiver pronto, você pode adicionar assinaturas digitais para aprovação usando o Adobe Sign. Essa funcionalidade funciona de forma um pouco diferente da funcionalidade usada até agora. Para assinaturas digitais, um aplicativo deve ser configurado para usar o OAuth para autenticação de usuário.

A primeira etapa na configuração do aplicativo é [registrar o aplicativo](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md) para usar o OAuth para Adobe Sign. Depois de entrar, navegue até a tela de criação de aplicativos clicando em *Conta*, abra a seção *API Adobe Sign* e clique em *Aplicativos de API* para abrir a lista de aplicativos registrados.

![Imagem da primeira etapa no registro do aplicativo](assets/AWNjs_9.png)

Para criar uma nova entrada de aplicativo, clique no ícone de mais no canto superior direito.

![Imagem do ícone de adição no canto superior direito da tela](assets/AWNjs_10.png)

Na janela exibida, insira o nome do aplicativo e o nome de exibição. Selecione *Cliente* para o domínio e clique em *Salvar*.

![Imagem de onde inserir o nome do aplicativo e o nome para exibição](assets/AWNjs_11.png)

Depois que o aplicativo for criado, você poderá selecioná-lo na lista e clicar em *Configurar OAuth para o aplicativo*. Selecione as opções. No URL de redirecionamento, insira o URL do seu aplicativo. Você pode inserir vários URLs aqui. Para o aplicativo que você está testando, o valor é:

```
http://localhost:3000/signed-in 
```

O processo de usar o OAuth para obter um token é padrão. Seu aplicativo direciona um usuário a um URL para fazer logon. Depois que o usuário fizer logon com êxito,
eles são redirecionados de volta ao aplicativo com informações adicionais nos parâmetros de consulta da página.

Para o URL de logon, o aplicativo deve passar sua ID de cliente, o URL de redirecionamento e uma lista dos escopos necessários.

O padrão da URL é semelhante ao seguinte:

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

O usuário é solicitado a fazer logon em sua ID para Adobe Sign. Após o logon, é solicitado que eles forneçam permissões para o aplicativo.

![Imagem da tela de confirmação de acesso](assets/AWNjs_12.png)

Se o usuário clicar em *Permitir Acesso* na URL de redirecionamento, um parâmetro de consulta chamado code passará o código de autorização:

https://YourServer.com/?code=**\&lt;authorization_code\>**\&amp;api_access_point=https://api.adobesign.com&amp;web_access_point=https://secure.adobesign.com

A publicação desse código no servidor da Adobe Sign, junto com a ID do cliente e o segredo do cliente, fornece um token de acesso para acessar o serviço. Salve os valores nos parâmetros `api_access_point` e `web_access_point`. Esses valores são usados para solicitações adicionais.

```
var requestURL = ' ${api_access_point}oauth/token?code=${code}'
  +'&client_id=${client_id}'
  +'&client_secret=${client_secret}&'
  +'redirect_uri=${redirect_url}&'
  +'grant_type=authorization_code';
request.post(requestURL, {form: { }
}, (err,response,body)=>{                
    var token_response = JSON.parse(body)
    var access_token = token_response.access_token;
    console.log(access_token);
});
```

Quando um documento exigir uma assinatura, o upload do documento deverá ser feito primeiro. Seu aplicativo pode carregar o documento para o valor `api_access_point` que foi recebido ao solicitar o token OAUTH. Ponto de extremidade `/api/rest/v6/transientDocuments`. Os dados da solicitação são como os seguintes:

```
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.adobesign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"
<PDF CONTENT>
```

No aplicativo, crie a solicitação com o seguinte código:

```
var uploadRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/transientDocuments',
  'headers': {
    'Authorization': 'Bearer  ${auth_token}'
  },
  formData: {
    'File': {
      'value': fs.createReadStream(documentPath),
      'options': {
        'filename': fileName,
        'contentType': null
      }
    }
  }
};
 
request(uploadRequest, (error, response) => {
  if (error) throw new Error(error);
  var jsonResponse = JSON.parse(response.body);
  var transientDocumentId = jsonResponse.transientDocumentId;
  logger.info('transientDocumentId:', transientDocumentId)
});
```

A solicitação retorna um valor `transientID`. O documento foi carregado, mas ainda não foi enviado. Para enviar o documento, use o `transientID` para solicitar o envio do documento.

Comece criando um objeto JSON que contenha as informações do documento a ser assinado. A seguir, a variável `transientDocumentId` contém a ID do código acima e `agreementDescription` contém texto descrevendo o contrato que precisa ser assinado. As pessoas que devem assinar o documento estão listadas em `participantSetsInfo` por seu endereço de email e função.

```
var requestBody = {
  "fileInfos":[
    {"transientDocumentId":transientDocumentId}],
    "name":agreementDescription,
    "participantSetsInfo":[
      {"memberInfos":[{"email":"user@domain.com"}],
       "order":1,"role":"SIGNER"}
    ],
    "signatureType":"ESIGN","state":"IN_PROCESS"
};
```

Enviar esta solicitação da Web cria a solicitação de assinatura e retorna um objeto JSON com uma ID para a solicitação de contrato:

```
request(requestBody, function (error, response) {
  if (error) throw new Error(error);
  var JSONResponse = JSON.parse(response.body);
  var requestId = JSONResponse.id;
});
```

Se os signatários esquecerem de assinar e precisarem de outro email de notificação, envie as notificações novamente usando a ID recebida anteriormente. A única diferença é que você também deve adicionar as IDs de participante das partes. Você pode obter as IDs dos participantes enviando uma solicitação GET para `/agreements/{agreementID}/members`.

Para solicitar o envio do lembrete, primeiro crie um objeto JSON descrevendo a solicitação. O objeto mínimo precisa de uma lista das IDs de participante e um status para o lembrete (”ATIVO”, “CONCLUÍDO” ou “CANCELADO”).

Opcionalmente, a solicitação pode ter informações adicionais, como um valor para “observação” que será exibido ao usuário. Ou um atraso (em horas) para aguardar até o envio do lembrete (em `firstReminderDelay`) e uma frequência de lembrete (no campo “frequency”), que aceita valores como DAILY_UNTIL_SIGNED, EVERY_THIRD_DAY_UNTIL_SIGNED ou WEEKLY_UNTIL_SIGNED.

```
var requestBody = {
  //participantList is an array of participant ID strings
  "recipientParticipantIds":participantList
  ,"status":"ACTIVE",
  "note":"This is a reminder to sign out important agreement."
}
 
var reminderRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/agreements/${agreementID}/reminders',
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
 
};

request(reminderRequest, function (error, response) {
});
```

E isso é tudo o que é preciso para enviar um pedido de lembrete.

![Imagem de formulário da Web](assets/AWNjs_13.png)

## Criação de formulários web

Você também pode usar a API do Adobe Sign para criar formulários da Web. Os Formulários web permitem incorporar um formulário em uma página da Web ou vincular diretamente a ela. Depois que um formulário web é criado, ele também é exibido entre os formulários web no console do Adobe Sign. Você pode criar formulários web com status RASCUNHO para criação incremental, status AUTHORING para edição dos campos do formulário web e status ATIVO para hospedar imediatamente o formulário.

![Imagem de formulário da Web na tela Gerenciar do Adobe Sign](assets/AWNjs_14.png)

Para criar um formulário da Web, use o formulário `transientDocumentId`. Decida o título do formulário e o status para inicializá-lo.

```
var requestBody = {
  "fileInfos": [
    {
      "transientDocumentId": transientDocumentId
    }
  ],
  "name": webFormTitle,
  "state": status,
  "widgetParticipantSetInfo": {
    "memberInfos": [ { "email": "" } ],
    "role": "SIGNER"
  }
}
```

```
var createWebFormRequest = {
  'method': 'POST',
  'url': `${oauthParameters.signin_domain}/api/rest/v6/widgets`,
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
}
```

```
request(createWebFormRequest, function (error, response) {
  var jsonResp = JSON.parse(response.body);
  var webFormID = jsonResp.id;
});
```

Agora você pode incorporar ou vincular ao seu documento.

## Próximas etapas

Como você pode ver nas inicializações rápidas e no código fornecido, é fácil implementar processos de aprovação de documentos digitais e de PDF usando o Node com as APIs do [!DNL Adobe Acrobat Services]. As APIs do Adobe se integram perfeitamente aos aplicativos cliente existentes.

Para descobrir os escopos necessários para uma chamada ou para ver como a chamada é compilada, você pode criar chamadas de exemplo da [documentação da API Rest](https://secure.na4.adobesign.com/public/docs/restapi/v6). O [Quickstarts](https://github.com/adobe/pdftools-node-sdk-samples) também demonstra outras funcionalidades e formatos de arquivo para os processos de APIs do [!DNL Adobe Acrobat Services].

Você pode adicionar vários recursos de PDF aos aplicativos, permitindo que os usuários visualizem e assinem seus documentos de modo rápido e fácil, e muito mais. Para começar, confira [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) hoje.
