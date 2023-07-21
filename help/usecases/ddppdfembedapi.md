---
title: Publicação de documentos digitais
description: Saiba como exibir documentos PDF incorporados em páginas da Web usando a API incorporada do Adobe PDF
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8090.jpg
jira: KT-8090
exl-id: 3aa9aa40-a23c-409c-bc0b-31645fa01b40
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1903'
ht-degree: 0%

---

# Publicação de documentos digitais

![Use Case Hero Banner](assets/UseCaseDigitalHero.jpg)

Os documentos eletrônicos estão por toda parte. Na verdade, provavelmente [trilhões de PDF](https://itextpdf.com/en/blog/technical-notes/do-you-know-how-many-pdf-documents-exist-world) globalmente, e esse número aumenta todos os dias. Ao incorporar um visualizador de PDF em suas páginas da Web, você permite que os usuários exibam documentos sem recriar seu HTML e CSS ou impedir o acesso ao seu site.

Vamos explorar um cenário popular. Uma empresa publica [white papers em seu site](https://www.adobe.io/apis/documentcloud/dcsdk/digital-content-publishing.html)
fornecer contexto para seus aplicativos e serviços. O profissional de marketing do site quer entender melhor como os usuários interagem com seu conteúdo baseado em PDF e incorporá-lo à sua página da Web e marca. Decidiram publicar os white papers como [conteúdo controlado](https://whatis.techtarget.com/definition/gated-content-ungated-content#:~:text=Gated%20content%20is%20online%20materials,about%20their%20jobs%20and%20organizations.), controlando quem pode baixá-los.

## O que você pode aprender

Neste tutorial prático, saiba como exibir documentos do PDF incorporados em páginas da Web usando [API incorporada do Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html), que é gratuito e fácil de usar. Esses exemplos usam alguns JavaScript, Node.js, Express.js, HTML e CSS. Você pode exibir o código completo do projeto em [GitHub](https://www.google.com/url?q=https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app&amp;sa=D&amp;source=editors&amp;ust=1617129543031000&amp;usg=AOvVaw2rzSwYuJ_JI7biVIgbNMw1).

## APIs e recursos relevantes

* [API de incorporação do PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API de serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Código do projeto](https://www.google.com/url?q=https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app&amp;sa=D&amp;source=editors&amp;ust=1617129543031000&amp;usg=AOvVaw2rzSwYuJ_JI7biVIgbNMw1)

## Criando um Nó Web App

Vamos começar criando um site usando Node.js e Express que usa um modelo bonito e oferece várias PDF para download.

Primeiro, [baixar e instalar Node.js](https://nodejs.org/en/download/).

Para criar facilmente um projeto Node.js com uma estrutura mínima de aplicação Web, instale a ferramenta gerador de aplicações `` `express-generator` ``.

```
npm install express-generator -g
```

Em seguida, crie o novo aplicativo Express chamado pdf-app, escolhendo como o mecanismo de exibição.

```
express pdf-app --view=ejs
```

Agora, vá para o diretório \\pdf-app e instale todas as dependências do projeto.

```
cd pdf-app
npm install
```

Em seguida, inicie o servidor Web local e execute o aplicativo.

```
npm start
```

Finalmente, abra o site em <http://localhost:3000>.

![Captura de tela do site básico](assets/ddp_1.png)

Agora você tem um site básico.

## Renderização de dados do white paper

Para publicar white papers no site, os dados do white paper são definidos e preparados no site para exibir esses documentos. Primeiro, crie uma nova pasta \\data na raiz do projeto. As informações sobre white papers disponíveis vêm de um novo arquivo chamado [data.json](https://github.com/marcelooliveira/EmbedPDF/blob/main/pdf-app/data/data.json), que é colocado na pasta de dados.

Para dar ao aplicativo Web um visual bonito e elegante, instale o [Bootstrap](https://getbootstrap.com/) e [Fonte impressionante](https://fontawesome.com/) bibliotecas front-end.

```
npm install bootstrap
npm install font-awesome
```

Abra o arquivo app.js e inclua esses diretórios como origens para arquivos estáticos, colocando-os após o `` `express.static` `` linha.

```
app.use(express.static(path.join(__dirname, '/node_modules/bootstrap/dist')));
app.use(express.static(path.join(__dirname, '/node_modules/font-awesome')));
```

Para incluir os documentos PDF, crie uma pasta denominada \\pdfs na pasta \\public do projeto. Em vez de criar os PDF e as miniaturas por você mesmo, você pode copiá-los desta [Pasta de repositório do GitHub](https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app/public) às pastas \\pdfs e \\image.

A pasta \\public\\pdfs agora contém os documentos do PDF:

![Captura de tela dos ícones do arquivo PDF](assets/ddp_2.png)

Enquanto a pasta \\public\\images deve conter as miniaturas de cada um dos documentos do PDF:

![Captura de tela das miniaturas de PDF](assets/ddp_3.png)

Agora, abra o arquivo \\route\\index.js, que contém a lógica de roteamento da home page. Para usar os dados do white paper do arquivo data.json, você deve carregar o módulo Node.js responsável por acessar e interagir com o sistema de arquivos. Em seguida, declare `fs` constante na primeira linha do arquivo \\route\\index.js, da seguinte forma:

```
const fs = require('fs');
```

Em seguida, leia e analise o arquivo data.json e armazene-o na variável papers:

```
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
```

Agora modifique a linha para chamar o método de renderização para a exibição de índice, passando a coleção de papéis como o modelo para a exibição de índice.

```
res.render('index', { title: 'Embedding PDF', papers: papers });
```

Para renderizar a coleção de white papers na página inicial, abra o arquivo \\views\\index.ejs e substitua o código existente pelo código do [arquivo de índice](https://github.com/marcelooliveira/EmbedPDF/blob/main/pdf-app/views/index.ejs).

Agora, execute o npm start e abra novamente <http://localhost:3000> para exibir sua coleção de white papers disponíveis.

![Captura de tela de miniaturas para white papers](assets/ddp_4.png)

Nas próximas seções, é necessário aprimorar o site e usar [API de incorporação do PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) para exibir os documentos PDF na página da Web. A API incorporada do PDF é gratuita, basta obter uma credencial de API.

## Obter uma credencial de API incorporada PDF

Para obter uma credencial de API incorporada PDF gratuita, visite o [Introdução](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) após se inscrever em uma nova conta ou fazer logon na conta existente.

Clique em **Criar Novas Credenciais** e depois **Começar:**

![Captura de tela de como criar novas credenciais](assets/ddp_5.png)

Nesse momento, você será solicitado a se registrar para obter uma conta gratuita, caso não tenha uma.

Selecionar **API de incorporação do PDF** e digite o nome de suas credenciais e o domínio do aplicativo. Use o **localhost** domínio devido ao teste do aplicativo Web localmente.

![Captura de tela de criação de novas credenciais para a API de incorporação PDF](assets/ddp_6.png)

Clique no botão **Criar credenciais** para acessar suas credenciais de PDF e obter a ID do cliente (CHAVE da API).

![Captura de tela de como copiar novas credenciais](assets/ddp_7.png)

No projeto Node.js, crie um arquivo chamado .ENV na pasta raiz do aplicativo e declare a variável de ambiente para a ID de cliente incorporada do PDF com o valor da credencial da API KEY da etapa anterior.

```
PDF_EMBED_CLIENT_ID=**********************************************
```

Posteriormente, você usará essa ID de cliente para acessar a API incorporada PDF. Instale o pacote dotenv para acessar esta variável de ambiente usando o código Node.js.

```
npm install dotenv
```

Agora, abra o arquivo app.js e adicione a seguinte linha na parte superior do arquivo para que Node.js possa carregar o módulo dotenv:

```
require('dotenv').config();
```

## Exibindo PDF no aplicativo da Web

Agora use a API incorporada do PDF para exibir PDF no site. Abra o live [Demonstração da API incorporada PDF](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf).

![Captura de tela da demonstração da API de incorporação de PDF ao vivo](assets/ddp_8.png)

No painel esquerdo, você pode escolher o modo de incorporação que melhor atende às necessidades do seu site:

* **Janela inteira**: o PDF cobre todo o espaço da página da web

* **Contêiner Dimensionado**: o PDF é exibido dentro da página da web, uma página por vez, em uma div com tamanho limitado

* **Em linha**: o PDF inteiro é exibido em um div dentro da página da web

* **Lightbox**: o PDF é exibido como uma camada no topo da página da web

É recomendável usar o modo incorporado in-line para white papers e o gerador de código para incorporar posteriormente um PDF no aplicativo.

## Criação de uma página no modo incorporado linha

Para incorporar um visualizador de PDF em sua página da Web e exibir todas as páginas simultaneamente, crie uma nova página usando o modo incorporado em linha.

Crie uma nova exibição no arquivo \\views\\in-line.ejs usando o mecanismo de exibição EJS.

```
<! html DOCTYPE >
<html>
<head>
<title>
<%= title %>
</title>
<link rel='stylesheet' href='/stylesheets/style.css' />
<link rel='stylesheet' href='/css/bootstrap.min.css'/>
<link rel='stylesheet' href='/css/font-awesome.min.css' />
<style type="text/css">
p {
font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}
</style>
</head>
<body class="m-0">
<div>
<main>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<h3>
<p class="text-center">Grow your business, establish your brand,<br
/>
```

E coloque seus clientes em primeiro lugar.

```
</p>
</h3>
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
</div>
</main>
<footer>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<p class="text-center">Bodea Inc. Your trusted partner since 2008</p>
</div>
</div>
</footer>
</div>
</div>
</body>
</html>
```

Em seguida, modifique \\views\\index.ejs para criar um botão para abrir a exibição em linha.

```
<div class="card-body">
<h5 class="card-title">
<span>
<%= paper.title %>
</span>
</h5>
<p>
<a class="btn btn-sm btn btn-danger" href="/in-line/<%=
paper.id %>">
<span type="button"></span>
<span class="fa fa-file-pdf-o"></span>&nbsp;View Document</button>
</a>
</p>
</div>
```

Abra o arquivo app.js e declare um novo roteador após a declaração indexRouter:

```
var indexRouter = require('./routes/index');
var inLineRouter = require('./routes/in-line');
```

Em seguida, adicione esse código após app.use(&#39;/&#39;, indexRouter); para associar a exibição do modo incorporado em linha com seu roteador:

```
app.use('/', indexRouter);
app.use('/in-line', inLineRouter);
```

Agora, crie um novo arquivo in-line.js em \\route para criar uma nova lógica de roteador. Inclui o Express, um módulo Node que habilita um back-end de aplicativo da Web.

```
var express = require('express');
const fs = require('fs');
var router = express.Router();
```

Em seguida, crie um ponto de extremidade que manipule solicitações de GET para uma ID de white paper específica e renderize a exibição in-line.ejs.

```
router.all('/:id', function(req, res, next) {
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
let paper = papers.filter(p => p.id == parseInt(req.params.id))[0];
res.render('in-line', { title: paper.title, paper: paper });
});
module.exports = router;
```

Olhe novamente para o [demonstração ao vivo](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) para gerar automaticamente o código de API incorporado PDF. Clique em **Em linha** no painel esquerdo:

![Captura de tela da demonstração da API de incorporação de PDF ao vivo](assets/ddp_8.png)

Clique em **Gerar código** para ver o código de HTML necessário para exibir um visualizador PDF de contêiner dimensionado.

![Captura de tela da visualização do código](assets/ddp_9.png)

Clique em **Copiar código** e cole o código no arquivo in-line.ejs.

```
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
<div class="row align-items-center border border-primary">
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function(){
var adobeDCView = new AdobeDC.View({clientId: "<YOUR_CLIENT_ID>", divId: "adobe-dc-view"});
adobeDCView.previewFile({
content:{location: {url: "https://documentcloud.adobe.com/view-sdk-demo/PDFs/Bodea Brochure.pdf"}},
metaData:{fileName: "Bodea Brochure.pdf"}
}, {embedMode: "IN_LINE"});
});
</script>
</div>
```

No entanto, os parâmetros do documento ainda são codificados. Vamos substituí-los pela sintaxe de colchete EJS (\&lt;%= someValue %\>) para renderizar a página de acordo com os dados do modelo do white paper.

```
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function () {
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url: "<%= paper.pdf %>" } },
metaData: { fileName: "<%= paper.fileName %>" }
}, {
embedMode: "IN_LINE"
});
});
</script>
```

Agora execute o aplicativo com o comando npm start e abra o site em <http://localhost:3000>.

![Captura de tela das miniaturas do PDF](assets/ddp_10.png)

Por fim, escolha um white paper e clique em **Exibir documento** para abrir uma nova página com o PDF incorporado in-line:

![Captura de tela do white paper PDF ](assets/ddp_11.png)

Observe como as opções Baixar PDF e Imprimir PDF agora estão presentes.

![Captura de tela das opções de download e impressão](assets/ddp_12.png)

Você deseja controlar esses sinalizadores no back-end. Posteriormente, você poderá implementar controles de autorização com base na identidade do usuário e restringir o acesso de acordo com suas regras de negócios. Essa complexidade não é necessária aqui, então vamos apenas modificar \\route\\in-line.js para incluir as propriedades autenticadas e de permissões no objeto de modelo.

```
let authenticated = false;
res.render('in-line', {
title: paper.title,
paper: paper,
authenticated: authenticated,
permissions: {
showDownloadPDF: true,
showPrintPDF: true,
showFullScreen: true
}
});
```

Em seguida, modifique \\views\\in-line.ejs para que sua página da Web possa renderizar os valores de sinalizador provenientes do back-end.

```
embedMode: "IN_LINE",
showDownloadPDF: <%= permissions.showDownloadPDF %>,
showPrintPDF: <%= permissions.showPrintPDF %>,
showFullScreen: <%= permissions.showFullScreen %>
Now, open the in-line.js route file and modify it to disallow the printing, downloading, and full-screen controls.
permissions: {
showDownloadPDF: false,
showPrintPDF: false,
showFullScreen: false
}
```

Em seguida, execute novamente o aplicativo para ver como essa alteração é refletida no visualizador de PDF.

![Captura de tela do arquivo PDF](assets/ddp_13.png)

## Criação de conteúdo controlado

De acordo com o cenário do usuário final, o profissional de marketing do site da empresa quer entender melhor como os usuários interagem com seu conteúdo baseado em PDF e incorporar o conteúdo ao restante de sua página da Web e marca.

Nosso foco é a incorporação de PDF, portanto, você não está criando um recurso de autenticação de usuário. Em vez disso, basta implementar um paywall simples e falso usando um formulário da Web que aceita algumas informações do usuário e, em seguida, exibe o documento do PDF depois que o usuário envia o formulário.

Substitua o arquivo \\route\\in-line.js pelo conteúdo abaixo para fornecer ao modelo de exibição as informações do usuário:

```
var express = require('express');
const fs = require('fs');
var router = express.Router();
router.all('/:id', function(req, res, next) {
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
let paper = papers.filter(p => p.id == parseInt(req.params.id))[0];
let authenticated = false;
let user = {};
if (req.body.firstName) {
user = {
firstName: req.body.firstName,
lastName: req.body.lastName,
jobTitle: req.body.jobTitle,
email: req.body.email,
};
authenticated = true;
}
res.render('in-line', {
title: paper.title,
paper: paper,
user: user,
authenticated: authenticated,
permissions: {
showDownloadPDF: false,
showPrintPDF: false,
showFullScreen: false
}
});
});
module.exports = router;
```

Em seguida, substitua o conteúdo de \\views\\in-line.ejs pelo código abaixo. Exibe o formulário de dados do usuário ou o visualizador de PDF, dependendo se é um usuário autenticado.

```
<!DOCTYPE html>
<html>
<head>
<title>
<%= title %>
</title>
<link rel='stylesheet' href='/css/bootstrap.min.css'/>
<link rel='stylesheet' href='/css/font-awesome.min.css' />
<style type="text/css">
p {
font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}
</style>
</head>
<body class="m-0">
<% if (authenticated) { %>
<header class="bg-dark text-white">
<div class="text-right mr-4">Hello, <%= user.firstName %> <%= user.lastName%></div>
</header>
<% } %>
<div>
<main>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<h3>
<p class="text-center">Grow your business, establish your brand,<br
/>
```

E coloque seus clientes em primeiro lugar.

```
</p>
</h3>
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
<% if (!authenticated) { %>
<div class="row">
<form method="POST" class="center-panel text offset-md-3 col-md-6 border">
<fieldset class="offset-md-1">
<legend>Submit your info to<br/>access the whitepaper</legend>
<p><input name="firstName" placeholder="first name"/></p>
<p><input name="lastName" placeholder="last name"/></p>
<p><input name="jobTitle" placeholder="job title"/></p>
<p><input name="email" placeholder="email"/></p>
<p><button type="submit" class="btn btn-sm btn btn-primary">Submit</button></p>
</fieldset>
</form>
</div>
<% } %>
<% if (authenticated) { %>
<div class="row align-items-center border border-primary">
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function () {
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url: "<%= paper.pdf %>" } },
metaData: { fileName: "<%= paper.fileName %>" }
}, {
embedMode: "IN_LINE",
showDownloadPDF: <%= permissions.showDownloadPDF %>,
showPrintPDF: <%= permissions.showPrintPDF %>,
showFullScreen: <%= permissions.showFullScreen %>
});
});
</script>
<% } %>
</div>
</div>
</main>
<footer>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<p class="text-center">Bodea Inc. Your trusted partner since 2008</p>
</div>
</div>
</footer>
</div>
</div>
</body>
</html>
```

![Captura de tela de conteúdo de controle](assets/ddp_14.png)

Os visitantes do site agora só podem acessar PDF após o envio de suas informações:

![Captura de tela do conteúdo PDF no visualizador incorporado](assets/ddp_15.png)

## Ativação de eventos

Vamos ver como integrar facilmente os eventos do visualizador de PDF com seu aplicativo para coletar dados de análise para o profissional de marketing. Para estender o visualizador usando PDF EmbedAPI, adicione as seguintes linhas de código após declarar a variável adobeDCView e antes de chamar o método previewFile:

```
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.registerCallback(
AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
function(event) {
console.log(event);
},
{ enablePDFAnalytics: true }
);
```

Agora, execute novamente o aplicativo e abra as ferramentas de desenvolvedor do navegador para ver os dados do evento.

![Captura de tela do código](assets/ddp_16.png)

Você pode enviar esses dados para [Adobe Analytics](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=view) ou outras ferramentas de análise.

## Próximas etapas

[!DNL Acrobat Services] APIs ajudam desenvolvedores a resolver desafios de publicação digital com facilidade usando um fluxo de trabalho centrado em PDF. Você viu como criar um aplicativo Web de exemplo Node para exibir uma coleção de white papers. Em seguida, adquira um [credencial de API gratuita](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) acesso restrito e incorporado aos white papers, que podem ser exibidos em um dos quatro [modos de incorporação](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf).

A junção desse fluxo de trabalho ajuda a [profissional de marketing hipotético](https://www.adobe.io/apis/documentcloud/dcsdk/digital-content-publishing.html) reúna informações de contato de clientes potenciais em troca de downloads de white papers e visualize estatísticas sobre quem está interagindo com as PDF. Você pode incorporar esses recursos em seu site para impulsionar e monitorar o envolvimento do usuário.

Se você é um desenvolvedor de Angular ou React, pode gostar de tentar [amostras adicionais](https://github.com/adobe/pdf-embed-api-samples) apresentando como integrar a API PDF Embed a projetos React e Angular.

O Adobe permite que você desenvolva uma experiência do cliente completa com soluções inovadoras. Check-out [API incorporada do Adobe PDF](https://www.adobe.io/apis/documentcloud/viesdk) gratuitamente. Para explorar o que mais você pode fazer, experimente a API de serviços da Adobe PDF com [pay-as-you-gopr](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)[gelo](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html).

[Começar](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) com [!DNL Adobe Acrobat Services] APIs hoje.
