---
title: Revisões e aprovações
description: Saiba como criar um fluxo de trabalho de revisão e aprovação de documentos para colaboração entre equipes
type: Tutorial
role: Developer
level: Intermediate
thumbnail: KT-8094.jpg
kt: 8094
exl-id: d704620f-d06a-4714-9d09-3624ac0fcd3a
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 0%

---

# Revisões e aprovações

![Use Case Hero Banner](assets/UseCaseReviewsHero.jpg)

A colaboração remota entre equipes se tornou necessária para muitas empresas durante a pandemia de COVID-19, [compartilhamento e revisão de documentos digitais](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html) apresenta uma série de desafios para equipes e recursos multifuncionais.

Esses desafios incluem compartilhar documentos em diferentes formatos de arquivo, revisar e comentar o conteúdo com eficiência e sincronizar com as edições mais recentes. [!DNL Adobe Acrobat Services] As APIs foram projetadas para permitir que desenvolvedores de aplicativos solucionem esses desafios para seus usuários.

## O que você pode aprender

Este tutorial prático mostra como criar um fluxo de trabalho de revisão e aprovação de documentos em um aplicativo Web Node.js e Express. Para acompanhar este tutorial, você precisa de alguma experiência com Node.js.

O aplicativo tem os seguintes recursos:

* Converter tipos de arquivo diferentes em PDF

* Ativar uploads de arquivos

* Dê aos usuários a capacidade de adicionar comentários e anotações

* Exibir as PDF junto com esses comentários

* Permitir que perfis de usuário identifiquem autores de comentários

* Combine arquivos em um PDF final que os usuários podem baixar

## APIs e recursos relevantes

* [API de serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API de incorporação do PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Código do projeto](https://github.com/contentlab-io/adobe_reviews_and_approvals)

## Criando credenciais de API Adobe

Antes de iniciar o código, você deve [criar credenciais](https://www.adobe.com/go/dcsdks_credentials) para a API incorporada do Adobe PDF e a API de serviços do Adobe PDF. A API de incorporação do PDF é gratuita para uso. A API de serviços do PDF é gratuita para seis meses, então você pode mudar para um [plano pré-pago](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) por apenas \$0,05 por transação de documento.

Ao criar credenciais para a API de serviços do PDF, selecione o **Criar amostra de código personalizada** e selecione Node.js para o idioma. Salve o arquivo ZIP e extraia pdftools-api-credentials.json e private.key no diretório raiz do projeto Node.js Express.

## Configurando um projeto e dependências

Configure seu projeto Node.js e Express para fornecer arquivos estáticos de uma pasta chamada &quot;public&quot;. Você pode configurar os modos dos seus projetos, dependendo das suas preferências. Para começar a trabalhar rapidamente, você pode usar o [Gerador de aplicativos Express](https://expressjs.com/en/starter/generator.html). Ou se quiser simplificar, você pode [começar do zero](https://expressjs.com/en/starter/hello-world.html) e mantenha seu código em um único arquivo JavaScript. No projeto de amostra vinculado acima, você está usando a abordagem de um arquivo e mantendo todo o código em index.js.

Copie o `pdftools-api-credentials.json` e `private.key` arquivos da amostra de código personalizado para o diretório raiz do projeto. Além disso, adicione-os ao arquivo .gitignore, se você tiver um, para que seus arquivos de credencial não sejam enviados acidentalmente para um repositório.

Em seguida, execute `npm install @adobe/documentservices-pdftools-node-sdk` para instalar o Node.js SDK para Serviços PDF. Importe este módulo e crie o objeto de credenciais de API dentro do código (index.js em seu projeto de amostra), após o restante das importações de dependência como esta:

```
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();
```

O código inicial deve ter esta aparência:

```
  
  const express = require( "express" );
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();

  const app = express();

  app.use( express.static( "public" ) );

  app.listen( 8889, function() {
      console.log( "Server started on port", 8889 );
  } );
```

Agora você está pronto para trabalhar com [!DNL Acrobat Services] APIs.

## Convertendo um arquivo em PDF

Na primeira parte do fluxo de trabalho de documentos, o usuário final deve fazer upload dos documentos para compartilhar. Para ativar isso, adicione uma função de upload e consolide os diferentes formatos de arquivo de documento em PDF para prepará-los para o processo de revisão.

Comece criando uma função para converter documentos em PDF com base no [exemplo de snippet para a API de serviços do PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html). Este exemplo também mostra snippets para muitos outros recursos vitais, incluindo reconhecimento óptico de caracteres (OCR), proteção e remoção de senha e compactação.

```
function fileToPDF( filename, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

      // Set operation input from a source file.
      const input = PDFToolsSdk.FileRef.createFromLocalFile( filename );
      createPdfOperation.setInput( input );

      // Execute the operation and Save the result to the specified location.
      createPdfOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
  }
```

Agora você pode usar essa função para criar PDF a partir de documentos carregados.

## Manipulando uploads de arquivos

Em seguida, o servidor precisa de um ponto de extremidade de upload de arquivo no servidor Web para receber e processar os documentos.

Primeiro, crie uma pasta dentro de uma pasta de uploads e nomeie-a de &quot;rascunhos&quot;. Você armazena aqui os arquivos carregados e os arquivos PDF convertidos. Em seguida, execute `npm install express-fileupload` para instalar o módulo Express-FileUpload e adicionar o middleware ao Express em seu código:

```
const fileUpload = require( "express-fileupload" );
app.use( fileUpload() );
```

Agora, adicione um `/upload `e salve o arquivo carregado dentro da pasta rascunhos usando o mesmo nome de arquivo. Em seguida, chame a função que você escreveu anteriormente para criar um arquivo PDF do mesmo documento se ele ainda não estiver no formato PDF. É possível gerar um nome de arquivo para o novo arquivo PDF com base no nome do documento carregado original:

```
// Create a PDF file from an uploaded file
app.post( "/upload", ( req, res ) => {
    if( !req.files || Object.keys( req.files ).length === 0 ) {
        return res.status( 400 ).send( "No files were uploaded." );
    }
    
    // Create PDF from the uploaded file
    let file = req.files.myFile;
    file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
        if( err ) {
            return res.status( 500 ).send( err );
        }
        if( file.name.endsWith( ".pdf" ) ) {
            res.redirect( "/" );
        }
        else {
            // Convert to PDF
            fileToPDF( __dirname + "/uploads/drafts/" + file.name, __dirname + "/uploads/drafts/" + file.name.replace( /\./g, "-" ) + ".pdf", ( file ) => {
                res.redirect( "/" );
            } );
        }
    });
} );
```

## Criando uma página de upload

Agora, para carregar arquivos do aplicativo da Web, crie um `index.html` página da Web dentro da pasta uploads. Na página, adicione um formulário de upload de arquivo que envie o arquivo para o ponto de extremidade /upload:

```
<form ref="uploadForm" 
      action="/upload"
      method="post" 
      encType="multipart/form-data">
      <input type="file" name="myFile" accept=".doc,.docx,.ppt,.pptx,.xls,.xlsx,.txt,.rtf,.bmp,.jpg,.gif,.tiff,.png">
      <input type="submit" value="Upload File" />
  </form>
```

![Captura de tela da capacidade de carregar arquivos na página da Web](assets/reviews_1.png)

Agora você pode carregar documentos no servidor Node.js. O servidor salva o arquivo dentro da pasta uploads/rascunhos e cria uma versão de formato PDF ao lado dele.

Agora você está pronto para incorporar os documentos carregados, portanto use a API Incorporada do PDF para permitir que os usuários adicionem comentários e anotações aos documentos facilmente.

## Enumerando arquivos PDF

Como um fluxo de trabalho de documento típico pode envolver vários documentos, você deve mostrar uma lista de documentos e vincular cada um a uma nova página de revisão de documento no aplicativo.

Primeiro, dentro do código do servidor, adicione um ponto final /files que obtém e retorna uma lista de todos os arquivos PDF armazenados na pasta uploads/drafts:

```
const fs = require( "fs" );

app.get( "/files", ( req, res ) =\> {

fs.readdir( \_\_dirname + "/uploads/drafts/", ( err, files ) =\> {

if( err ) {

return res.status( 500 ).send( err );using

}

return res.json( files.filter( f =\> f.endsWith( ".pdf" ) ) );

} );

} );
```

Adicionar um `/download/:file` rota que fornece acesso ao arquivo PDF carregado para incorporação na página da Web.

>[!NOTE]
>
>Em um aplicativo de produção, você deve adicionar autenticação e autorização para garantir que a solicitação seja proveniente de um usuário válido e que o usuário tenha permissão para acessar o documento.

```
app.get( "/download/:file", function( req, res ){
    // Note: In production code, this should check authentication and user access permissions
    res.download( __dirname + "/uploads/drafts/" + req.params[ "file" ] );
});
```

Atualize a página index.html com um elemento de lista de arquivos que é preenchido no momento do carregamento. Cada item pode ser vinculado a uma página da Web draft.html e você passa o nome do arquivo para a página usando parâmetros de sequência de caracteres de consulta.

>[!NOTE]
>
>Você usa o jQuery para anexar cada item, de modo que você deve carregar a biblioteca do jQuery em sua página da Web ou anexar o elemento usando um método diferente.

```
  <ul id="filelist">
      <li>Loading documents...</li>
  </ul>

  ...

  <script>
      // Load current files
      fetch( "/files" )
      .then( r => r.json() )
      .then( files => {
          if( files && files.length > 0 ) {
              $( "#filelist" ).empty();
              files.forEach( file => {
                  $( "#filelist" ).append( `<li><a
  href="/draft.html?file=${file}">${file}</a></li>` );
              })
          } else {
                  $("#filelist").append("<div>No documents found.</div>");
                }
      });
  </script>
```

![Captura de tela de seleção de um arquivo para revisão](assets/reviews_2.png)

## Incorporação de um PDF

Você está pronto para incorporar e mostrar arquivos PDF no aplicativo da Web.

Crie uma página da Web chamada &quot;draft.html&quot; e adicione um elemento div na página para o PDF incorporado:

```
  <div id="adobe-dc-view"></div>
```

Inclua o [!DNL Acrobat Services] biblioteca:

```
  <script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

Em uma tag de script personalizada, analise o nome do arquivo nos parâmetros da string de consulta para saber qual arquivo incorporar na página:

```
  <script type="text/javascript">
          let params = new URLSearchParams( window.location.search );
          let filename = params.get( "file" );
  </script>
```

Adicione um ouvinte de eventos de documento para o evento adobe_dc_view_sdk.ready que carrega o arquivo PDF especificado em uma exibição incorporada dentro do elemento div. Use a ID de cliente das credenciais de API incorporadas do PDF. Você deseja ativar comentários e anotações, portanto, incorpore a exibição no modo FULL_WINDOW e defina a opção showAnnotationsTools como true.

```
  document.addEventListener( "adobe_dc_view_sdk.ready", () => { 
      var adobeDCView = new AdobeDC.View( { 
          clientId: "YOUR CLIENT ID HERE",
          divId: "adobe-dc-view",
          locale: "en-US",
      } );
      adobeDCView.previewFile( {
          content: { location: { url: "download/" + filename } },
          metaData: { fileName: "Draft Version.pdf" }
      }, {
          embedMode: "FULL_WINDOW",
          showAnnotationTools: true,
          showPageControls: true
      } );
  });
```

## Criar um perfil de usuário

Por padrão, comentários e anotações são exibidos como &quot;Convidado&quot; nessa exibição. É possível definir o nome do revisor atual para os comentários e anotações registrando um retorno de chamada do perfil do usuário no código da página para a exibição PDF. Veja a seguir um perfil de exemplo. Em um aplicativo completo que inclui autenticação de usuário, as informações de perfil da sessão do usuário conectada podem ser definidas dessa maneira para identificar cada comentarista do documento no fluxo de trabalho de revisão.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.GET_USER_PROFILE_API,
      () => {
          return new Promise( ( resolve, reject ) => {
              resolve({
                  code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                  data: {
                      userProfile: {
                          name: "YOUR NAME",
                          firstName: "FIRST",
                          lastName: "LAST",
                          email: "document.editor@adobe.com"
                      }
                  }
              });
          });
      }
  );
```

Seu perfil o identifica como um usuário específico quando você vê e anota em qualquer documento carregado usando esta página da Web.

## Salvar feedback do documento

Depois que um usuário comenta em um documento, ele clica em **Economize.** Por padrão, clicar em **Salvar** baixa o arquivo PDF atualizado. Altere esta ação para atualizar o arquivo PDF atual no servidor.

Adicionar um `/save` endpoint para o código do servidor que substitui o arquivo PDF na pasta uploads/rascunhos:

```
  // Overwrite the PDF file with latest PDF changes and annotations
  app.post( "/save", ( req, res ) => {
      if( !req.files || Object.keys( req.files ).length === 0 ) {
          return res.status( 400 ).send( "No files were uploaded." );
      }

      let file = req.files.pdf;
      file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          res.send( "File uploaded" );
      });
  } );
```

Registre um retorno de chamada de exibição de PDF para a SAVE_API que carrega o conteúdo no ponto de extremidade /save. Você pode alterar o valor de autoSaveFrequency para permitir que seu aplicativo salve automaticamente as alterações em um timer e inclua metadados adicionais no arquivo atualmente incorporado ao concluir, se desejar.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.SAVE_API,
      ( metaData, content, options ) => {
          return new Promise( ( resolve, reject ) => {
              let formData = new FormData();
              formData.append( "pdf", new Blob( [ content ] ), "drafts/" + filename
  );
              fetch( "/save", {
                  method: "POST",
                  body: formData
              }).then( resp => {
                  resolve({
                      code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                      data: {
                          /* Updated file metadata after successful save operation */
                          metaData: Object.assign( metaData, {} )
                      }
                  });
              });
          });
      },
      {
          autoSaveFrequency: 0,
          enableFocusPolling: false,
          showSaveButton: true
      }
  );
```

Comentários e anotações nos documentos de rascunho agora são salvos no servidor. Você pode [leia mais sobre como retornos de chamada](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#callbacks-workflows) se ajuste ao seu fluxo de trabalho. Por exemplo, [retornos de chamada de status](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#status-callback) ajude a lidar com conflitos de arquivo se várias pessoas quiserem revisar e comentar no mesmo documento simultaneamente.

Na etapa final, você combina todos os documentos editados em um arquivo PDF usando a API de serviços do PDF.

## Combinar arquivos PDF

O código de combinação PDF é semelhante ao código de criação PDF, mas usa a operação CombineFiles e adiciona cada arquivo como entrada.

```
  function combineFilesToPDF( files, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          combineFilesOperation = PDFToolsSdk.CombineFiles.Operation.createNew();

      // Set operation inputs from source files.
      files.forEach( file => {
          const input = PDFToolsSdk.FileRef.createFromLocalFile( file );
          combineFilesOperation.addInput( input );
      } );

      // Execute the operation and Save the result to the specified location.
      combineFilesOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
 }
```

## Baixando o PDF final

Adicione um ponto de extremidade chamado /finalize que chame a função para combinar todos os arquivos PDF dentro do `uploads/drafts` em um `Final.pdf` e o baixa.

```
  app.get( "/finalize", ( req, res ) => {
      fs.readdir( __dirname + "/uploads/drafts/", ( err, files ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          combineFilesToPDF(
              files.filter( f => f.endsWith( ".pdf" ) ).map( f => __dirname + 
  "/uploads/drafts/" + f ),
              __dirname + "/uploads/Final.pdf", ( file ) => {
              res.download( file );
          } );
      } );
  } );
```

Finalmente, adicione um link na página da Web principal index.html a este ponto final /finalize. Esses links permitem que os usuários baixem o resultado do fluxo de trabalho do documento.

```
<a href="/finalize">Download final PDF</a>
```

![Captura de tela do download do documento final](assets/reviews_3.png)

## Próximas etapas

Este tutorial prático mostrou como [!DNL Acrobat Services] As APIs integram um [fluxo de trabalho de compartilhamento e revisão de documentos](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html) em uma aplicação Web. O aplicativo permite que trabalhadores remotos compartilhem arquivos e colaborem com seus colegas de equipe, o que é especialmente útil para funcionários e prestadores de serviço que trabalham em casa.

Você pode usar essas técnicas para ativar a colaboração em seu aplicativo ou explorar [Exemplos do SDK do Nó de Serviços PDF](https://github.com/adobe/pdftools-node-sdk-samples) e [Amostras de API incorporadas do PDF](https://github.com/adobe/pdf-embed-api-samples) no GitHub para obter inspiração sobre como usar as APIs do Adobe.

Pronto para ativar o compartilhamento e a revisão de documentos em seu próprio aplicativo? Inscreva-se no seu [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) conta de desenvolvedor. Acesse o Adobe PDF Embed gratuitamente e aproveite uma teste grátis de seis meses das outras APIs. Após a avaliação, você pode [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) por apenas \$0,05 por transação de documento, à medida que sua empresa cresce.
