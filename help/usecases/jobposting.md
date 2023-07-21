---
title: Lançamento de Cargo
description: Saiba como desenvolver uma experiência online simples e consistente para candidatos a emprego e empregadores
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8092.jpg
jira: KT-8092
exl-id: 0e24c8fd-7fda-452c-96f9-1e7ab1e06922
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1527'
ht-degree: 0%

---

# Lançamento de trabalho

![Use Case Hero Banner](assets/UseCaseJobHero.jpg)

Ao operar um site com vários usuários, é fundamental criar uma experiência que garanta uma experiência tranquila para todos.

Imagine o seguinte cenário: você tem um site que permite que os empregadores [carregar lançamentos de trabalho](https://www.adobe.io/apis/documentcloud/dcsdk/job-posting.html). Para candidatos à procura de emprego, é conveniente visualizar facilmente todos os documentos relacionados a uma publicação em um formato consistente. No entanto, é conveniente para os empregadores anexar informações em qualquer formato de arquivo que eles tenham. Para oferecer conveniência aos dois tipos de usuários, você pode converter automaticamente todos os documentos carregados em PDF e incorporá-los em linha na publicação.

## O que você pode aprender

Este tutorial prático percorre um exemplo de Node.js que usa [!DNL Adobe Acrobat Services] e [SDK Node.js](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) para adicionar esses recursos a um site de publicação de vagas. Isso cria um site que é mais fácil de usar e mais atraente para empregadores e candidatos a emprego. Aqui está o [concluído](https://github.com/contentlab-io/adobe_job_posting) [código do projeto](https://github.com/contentlab-io/adobe_job_posting), caso queira acompanhar enquanto lê.

Para iniciar, configure um aplicativo Web Node.js simples baseado em Express. [Express](https://expressjs.com/) é uma estrutura minimalista de aplicativos da Web que oferece recursos como roteamento e modelagem. O código do aplicativo está disponível em [GitHub](https://github.com/contentlab-io/adobe_job_posting). Além disso, instale o [Banco de dados PostgreSQL](https://www.postgresql.org/) e configure-o para armazenar o PDF.

## Relevante [!DNL Acrobat Services] APIs

* [API de incorporação do PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API de serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

## Criando credenciais de API Adobe

Primeiro, você deve [criar credenciais](https://www.adobe.com/go/dcsdks_credentials) para Adobe PDF Embed API (gratuito para uso) e Adobe PDF Services API (gratuito por seis meses, depois [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) por apenas \$0,05 por transação de documento). Ao criar credenciais para a API de serviços do PDF, selecione a opção &quot;Criar amostra de código personalizada&quot;. Salve o arquivo ZIP e extraia pdftools-api-credentials.json e private.key no diretório raiz do projeto Node.js Express.

Você também precisa de uma chave de API para a API incorporada disponível gratuitamente. De [Projetos](https://console.adobe.io/projects), vá para o projeto que você criou. Em seguida, clique em **Adicionar ao projeto** e selecione **API**. Por fim, clique em **API de incorporação do PDF**.

Especifique o domínio para a API PDF Embed. A chave da API deve ser pública (encontre-a no código executado pelo navegador). Ao especificar um domínio, você garante que outra pessoa em um domínio diferente não possa usar a chave de API.

Você não pode usar &quot;localhost&quot; como um domínio. Especifique um domínio, como &quot;testing.local&quot;, e edite o arquivo hosts no seu computador para redirecionar esse domínio para 127.0.0.1, que é o seu computador. Em seguida, em vez de testar seu aplicativo em localhost:3000, você pode testá-lo em testing.local:3000. Quando terminar, localize a chave de API para PDF Embed API na página do projeto.

## Adicionar um formulário de upload e um manipulador

Com um aplicativo Express funcionando e credenciais de API, você também precisa de um formulário que permita aos usuários fazer upload de seus documentos no site. Edite o modelo index.jade para essa finalidade.

Crie um campo de entrada para o nome da publicação de trabalho carregada e para um documento contendo mais informações.

No bloco de conteúdo do modelo, adicione o seguinte formulário:

```
extends layout

block content
  h1= title

  form(action="/upload", enctype="multipart/form-data", method="POST")
    label Job posting name:&nbsp;
    input(type="text", name="name", required="required")
    br
    br
    label Describing document:&nbsp;
    input(type="file", name="attachment", required="required")
    br
    br
    input(type="submit", value="Submit job posting")
```

Em seguida, adicione um manipulador para a solicitação POST à ação /upload. Em seguida, adicione uma rota para /upload ao arquivo route/index.js. Você pode criar um novo arquivo para esta rota, mas terá que atualizar o arquivo app.js para refletir o novo arquivo. Dentro deste manipulador de rota, você pode acessar o nome fornecido e o arquivo carregado.

```
router.post('/upload', async function (req, res, next) {
    const name = req.body.name;
    const fileContents = req.files.attachment.data;

    // code to work with the uploaded document
  });
```

A função é assíncrona para que você possa usar a palavra-chave await na função, o que é conveniente ao chamar os métodos que executam chamadas de API.

![Captura de tela do site de publicação de vagas](assets/jobs_1.png)

## Uso da API de serviços PDF

Antes de usar a API de serviços do PDF, você deve adicionar as seguintes importações à parte superior do arquivo de rotas:

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
  const { Readable } = require('stream');
```

Logo abaixo das importações, você pode carregar credenciais de API e criar um [conteúdo de execução](https://www.javascripttutorial.net/javascript-execution-context/). Como você pode reutilizar um contexto de execução para operações diferentes, faz sentido fazê-lo apenas uma vez.

```
  const credentials = PDFToolsSdk.Credentials
  .serviceAccountCredentialsBuilder()
  .fromFile("pdftools-api-credentials.json")
  .build();

  const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
```

Agora, volte a escrever o código no manipulador de solicitações no comentário no `router.post` bloco. Comece convertendo o documento em PDF.

```
  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);

  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

A maioria das operações segue os mesmos quatro passos. Primeiro, inicialize o tipo de operação, usando o método createNew da classe apropriada. Em seguida, crie a entrada para a operação, que é FileRef. As operações subsequentes podem ignorar esta etapa porque o resultado de uma operação também é um FileRef. Para esta operação inicial, crie um FileRef dos bytes do arquivo carregado. Terceiro, você deve atribuir a entrada à operação. Finalmente, a operação é executada, com o contexto de execução como um parâmetro no método execute. Esse método retorna uma Promessa para que você possa aguardar o resultado.

O código salva o PDF retornado em um arquivo e envia uma resposta simples de &quot;sucesso&quot; ao navegador. A parte &quot;Data&quot; do nome de arquivo garante um nome de arquivo exclusivo. SaveAsFile retorna um erro se o arquivo de destino existir.

## Conversão de imagens em texto e compactação do PDF

Agora, use o reconhecimento óptico de caracteres (OCR) para converter imagens em texto e compactar o resultado. Isso é feito usando as operações OCR e CompressPDF semelhantes à operação CreatePDF. Adicione o seguinte ao arquivo de rotas, em `router.post`:

```
  const name = req.body.name;
  const fileContents = req.files.attachment.data;

  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);
  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  const ocrOperation = PDFToolsSdk.OCR.Operation.createNew();
  ocrOperation.setInput(result);
  result = await ocrOperation.execute(executionContext);

  const compressPdfOperation = PDFToolsSdk.CompressPDF.Operation.createNew();
  compressPdfOperation.setInput(result);
  result = await compressPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

Só é necessário executar essa operação uma vez, pois o resultado é um FileRef, que o código pode passar para setInput.

Há uma alternativa melhor do que salvar o arquivo em um disco rígido e retornar uma resposta HTTP supersimplificada. Em vez disso, armazene o PDF em um banco de dados e exiba uma página da Web que incorpore o PDF usando a API de incorporação PDF livre de Adobe. Dessa forma, a publicação ou o folheto de vagas do empregador fica visível no site para os candidatos a emprego encontrarem e exibirem, com os logotipos da empresa e outros elementos de design.

## Armazenando o PDF em um banco de dados

Armazene os PDF em um banco de dados PostgreSQL. Obtenha o pacote node-postgres para se conectar a Postgres em Node.js. Instale o pacote stream-buffers porque, em algum momento, você deve armazenar o conteúdo do PDF em um buffer, e FileRef funciona apenas com fluxos. Portanto, use o pacote stream-buffers para gravar o conteúdo em um buffer.

```
npm install pg stream-buffers
```

Agora crie uma tabela de banco de dados para publicações de cargos. É necessária uma coluna para um identificador exclusivo, uma coluna para um nome e uma coluna para o PDF anexado. Você pode criar uma tabela de banco de dados a partir da interface de linha de comando (CLI) do Postgres:

```
CREATE TABLE job_postings (id TEXT PRIMARY KEY, name TEXT NOT NULL, attachment
BYTEA NOT NULL);
```

Volte para os arquivos Node.js. Adicione algumas importações na parte superior do arquivo:

```
  const { Client } = require('pg');
  const streamBuffers = require('stream-buffers');
```

Para armazenar o PDF na tabela do banco de dados, modifique a função de upload. Substitua as duas últimas linhas (saveAsFile e send) por este snippet de código:

```
  const pgClient = new Client();
  pgClient.connect();

  const id = Math.random().toString(36).substr(2, 6); // not securely random at all,
  but serves the purpose for this demo

  const writableStream = new streamBuffers.WritableStreamBuffer();
  writableStream.on("finish", async () => {    
    await pgClient.query("INSERT INTO job_postings VALUES ($1, $2, $3)", [
      id,
      name,
      writableStream.getContents()
    ]);
    res.redirect(`/job/${id}`);
  })
  result.writeToStream(writableStream);
```

Para gravar o conteúdo, crie um WritableStreamBuffer. Com o evento finish, é hora de executar a consulta SQL. O pacote node-postgres converte automaticamente o parâmetro Buffer no formato BYTEA. A consulta redireciona o usuário para /job/{id}, um ponto de extremidade criado posteriormente.

Para a API de incorporação do PDF, você também precisa de um ponto final que retorne apenas o conteúdo do PDF:

```
  router.get('/pdf/:id', async function (req, res, next) {
    const id = req.params.id;
 
    const pgClient = new Client();
    pgClient.connect();

  const pgResult = await pgClient.query("SELECT attachment FROM job_postings WHERE id
  = $1", [id]);
  const buffer = pgResult.rows[0].attachment;
  res.type('pdf');
    return res.send(buffer);
  });
```

## Incorporando o PDF

Agora, crie o /job/{id} ponto de extremidade, que renderiza um modelo contendo o nome da publicação de trabalho solicitada e um PDF incorporado.

```
router.get('/job/:id', async function(req, res, next) {
    const id = req.params.id;

    const pgClient = new Client();
    pgClient.connect();

    const pgResult = await pgClient.query("SELECT name FROM job_postings WHERE id =
  $1", [id]);
    const name = pgResult.rows[0].name;

    res.render('job', { pdf_url: `/pdf/${id}`, name });
  });
```

No diretório views/, crie um arquivo job.jade com este conteúdo:

```
  extends layout

  block content
    h1= name
    div(id='adobe-dc-view')
    script(src='https://documentcloud.adobe.com/view-sdk/main.js')
    script.
      window.embedUrl = "!{pdf_url}";
    script(src='/javascripts/embed-pdf.js')
```

O primeiro script é o Adobe View SDK, que facilita a incorporação do PDF. O segundo script é um one-liner in-line que define o valor de window.embedUrl para o URL do PDF fornecido pelo manipulador de rota Express. Crie você mesmo o terceiro script da seguinte maneira:

```
  document.addEventListener("adobe_dc_view_sdk.ready", function () {
    var adobeDCView = new AdobeDC.View({ clientId: "YOUR API KEY HERE", divId:
   "adobe-dc-view" });
    adobeDCView.previewFile({
      content: { location: { url: '//' + window.location.host + window.embedUrl }
         },
      metaData: { fileName: "Job posting" }
    });
  });
```

Agora, você pode testar todo o processo de upload de um documento, sendo redirecionado para a página /job/id e exibindo o PDF incorporado. Os usuários seguem as mesmas etapas para adicionar uma publicação de trabalho ou outro documento ao seu site.

![Captura de tela de teste de um documento PDF carregado](assets/jobs_2.png)

Para ver uma incorporação em linha em ação, confira esta [demonstração ao vivo](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/IN_LINE/Bodea%20Brochure.pdf).

## Próximas etapas

Este tutorial prático mostra como usar Node.js com [!DNL Acrobat Services] para converter um upload [lançamento de trabalho](https://www.adobe.io/apis/documentcloud/dcsdk/job-posting.html) em vários formatos para um PDF. O PDF resultante foi então incorporado em uma página da Web. Agora você pode adicionar a mesma função ao seu site, facilitando para os empregadores fazer upload de descrições de cargos, panfletos e muito mais para os candidatos a emprego encontrarem. Esses recursos ajudam todos a obter as informações necessárias para encontrar o emprego dos seus sonhos.

[!DNL Acrobat Services] ajuda a adicionar funções importantes de manipulação de documentos ao seu site ou aplicativo. Se quiser saber mais sobre o que essas APIs podem fazer, consulte a seguinte documentação do quickstart:

* [API de incorporação do PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API de serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

Para começar a adicionar recursos fáceis de usar para o manuseio de documentos ao seu site, [inscreva-se para obter a teste grátis](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html). A API incorporada do Adobe PDF é sempre gratuita e a API de serviços do Adobe PDF é gratuita por seis meses, portanto, é apenas \$0,05 por transação de documento, para que você possa [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) à medida que sua empresa cresce.
