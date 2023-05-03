---
title: Controle sua experiência online com o PDF e obtenha análises
description: Neste tutorial prático, você aprenderá a usar a API incorporada do Adobe PDF para controlar a aparência, ativar a colaboração e coletar análises sobre como os usuários interagem com PDF, incluindo o tempo gasto em uma página e pesquisas
type: Tutorial
role: Developer
level: Intermediate
thumbnail: KT-7487.jpg
kt: 7487
exl-id: 64ffdacb-d6cb-43e7-ad10-bbd8afc0dbf4
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1496'
ht-degree: 0%

---

# Controle sua experiência online de PDF e reúna análises

Sua organização publica PDF em seu site? Saiba como usar a API incorporada do Adobe PDF para controlar a aparência, permitir a colaboração e coletar análises sobre como os usuários interagem com PDF, incluindo o tempo gasto em uma página e pesquisas. Para começar este tutorial prático de 4 partes, selecione *Introdução à API de incorporação do PDF*.

<table style="table-layout:fixed">
<tr>
  <td>
    <a href="controlpdfexperience.md#part1">
        <img alt="Parte 1: Introdução à API de incorporação do PDF" src="assets/ControlPDFPart1_thumb.png" />
    </a>
    <div>
    <a href="controlpdfexperience.md#part1"><strong>Parte 1: Introdução à API de incorporação do PDF</strong></a>
    </div>
  </td>
  <td>
    <a href="controlpdfexperience.md#part2">
        <img alt="Parte 2: Adição da API PDF Embed a uma página da Web" src="assets/ControlPDFPart2_thumb.png" />
    </a>
    <div>
    <a href="controlpdfexperience.md#part2"><strong>Parte 2: Adição da API PDF Embed a uma página da Web</strong></a>
    </div>
  </td>
  <td>
   <a href="controlpdfexperience.md#part3">
      <img alt="Parte 3: Acesso a APIs do Analytics" src="assets/ControlPDFPart3_thumb.png" />
   </a>
    <div>
    <a href="controlpdfexperience.md#part3"><strong>Parte 3: Acesso a APIs do Analytics</strong></a>
    </div>
  </td>
  <td>
   <a href="controlpdfexperience.md#part4">
      <img alt="Parte 4: Adicionar interatividade com base em eventos" src="assets/ControlPDFPart4_thumb.png" />
   </a>
    <div>
    <a href="controlpdfexperience.md#part4"><strong>Parte 4: Adicionar interatividade com base em eventos</strong></a>
    </div>
  </td>
</tr>
</table>

## Parte 1: Introdução à API de incorporação do PDF {#part1}

Na parte 1, saiba como começar com tudo o que você precisa para as partes 1 a 3. Você começará obtendo credenciais de API.

**O que você precisa**

* Recursos do tutorial [baixar](https://github.com/benvanderberg/adobe-pdf-embed-api-tutorial)
* Adobe ID [obter um aqui](https://accounts.adobe.com/br)
* Servidor Web (Node JS, PHP etc.)
* Conhecimento prático de HTML / JavaScript / CSS

**O que estamos usando**

* Um servidor Web básico (Nó)
* Código do Visual Studio
* GitHub

### Obtendo credenciais

1. Vá para a [site Adobe.io](https://www.adobe.io/).
1. Clique em **[!UICONTROL Saiba mais]** em Crie experiências envolventes com documentos.

   ![Captura de tela do botão Saiba mais](assets/ControlPDF_1.png)

   Isso leva você ao [!DNL Adobe Acrobat Services] página inicial.

1. Clique em **[!UICONTROL Introdução]** na barra de navegação.

   Você verá uma opção em **Comece a usar o(a) [!DNL Acrobat Services] APIs** para **Criar Novas Credenciais** ou **Gerenciar credenciais existentes**.

1. Clique em **[!UICONTROL Introdução]** botão abaixo **[!UICONTROL Criar Novas Credenciais]**.

   ![Captura de tela do botão Introdução](assets/ControlPDF_2.png)

1. Escolha o **[!UICONTROL API de incorporação do PDF]** e adicione um nome de credencial de sua escolha e um domínio de aplicativo na próxima janela.

   >[!NOTE]
   >
   >Essas credenciais só podem ser usadas no domínio do aplicativo listado aqui. Você pode usar qualquer domínio escolhido.

   ![Captura de tela das credenciais](assets/ControlPDF_3.png)

1. Clique em **[!UICONTROL Criar credenciais]**.

   A página final do assistente fornece os detalhes da credencial do cliente. Deixe esta janela aberta para que você possa voltar a ela e copiar a ID do cliente (chave de API) para uso posterior.

1. Clique em **[!UICONTROL Exibir Documentação]** para acessar a documentação com informações detalhadas sobre como usar essa API.

   ![Captura de tela do botão Criar credenciais](assets/ControlPDF_4.png)

## Parte 2: Adição da API PDF Embed a uma página da Web {#part2}

Na parte 2, você aprenderá a incorporar facilmente a API PDF Embed a uma página da Web. Você fará isso usando a demonstração online da API incorporada do Adobe PDF para criar nosso código.

### Obtenha o código de exercício

Criamos código para você utilizar. Embora você possa usar seu próprio código, as demonstrações estarão no contexto dos recursos do tutorial. Baixar código de amostra [aqui](https://github.com/benvanderberg/adobe-pdf-embed-api-tutorial).

1. Ir para [[!DNL Adobe Acrobat Services] site](https://www.adobe.io/apis/documentcloud/dcsdk/).

   ![Captura de tela de [!DNL Adobe Acrobat Services] site](assets/ControlPDF_6.png)

1. Clique em **[!UICONTROL APIs]** na barra de navegação, vá para a **[!UICONTROL API de incorporação do PDF]** no link suspenso.

   ![Captura de tela do menu suspenso da API incorporada PDF](assets/ControlPDF_7.png)

1. Clique em **[!UICONTROL Experimente a demonstração]**.

   Uma nova janela é exibida com a caixa de proteção do desenvolvedor para a API de incorporação do PDF.

   ![Captura de tela de Experimente a demonstração](assets/ControlPDF_8.png)

   Aqui você pode ver as opções para os diferentes modos de visualização.

1. Clique nos diferentes modos de visualização para Full Window, Sized Container, In-Line e Lightbox.

   ![Captura de tela dos modos de visualização](assets/ControlPDF_9.png)

1. Clique em **[!UICONTROL Janela inteira]** modo de exibição e clique no botão **[!UICONTROL Personalizar]** para ativar e desativar as opções.

   ![Captura de tela do botão Personalizar](assets/ControlPDF_10.png)

1. Desativar **[!UICONTROL Baixar]** opção PDF.
1. Clique em **[!UICONTROL Gerar código]** para visualizar o código.
1. Copiar **[!UICONTROL ID do cliente]** a partir da janela Credenciais do cliente da Parte 1.

   ![Captura de tela da ID do cliente](assets/ControlPDF_11.png)

1. Abra o **[!UICONTROL Web]** -> **[!UICONTROL recursos]** -> **[!UICONTROL js]** -> **[!UICONTROL dc-config.js]** no editor de código.

   Você verá que a variável clientID está lá.

1. Cole as credenciais do cliente entre aspas duplas para definir a clientID para a credencial.

1. Voltar para a visualização de código da área restrita do desenvolvedor.

1. Copie a segunda linha que tem o script Adobe:

   ```
   <script src=https://documentccloud.adobe.com/view-sdk/main.js></script>
   ```

   ![Captura de tela do script](assets/ControlPDF_12.png)

1. Acesse o editor de código e abra o **[!UICONTROL Web]** -> **[!UICONTROL exercício]** -> **[!UICONTROL index.html]** arquivo.

1. Cole o código de script no `<head>` do arquivo na linha 18 abaixo do comentário que diz: **TODO: EXERCÍCIO 1: INSERIR MARCA DE SCRIPT DE API INCORPORADA**.

   ![Captura de tela de onde colar o código de script](assets/ControlPDF_13.png)

1. Volte para a visualização do código da sandbox do desenvolvedor e copie a primeira linha de código que possui:

   ```
   <div id="adobe-dc-view"></div>
   ```

   ![Captura de tela do local para copiar o código](assets/ControlPDF_14.png)

1. Acesse o editor de código e abra o **[!UICONTROL Web]** -> **[!UICONTROL exercício]** -> **[!UICONTROL index.html]** novamente.

1. Cole o `<div>` código no `<body>` do processo na linha 67 sob o comentário que diz **TODO: EXERCÍCIO 1: INSERIR CÓDIGO DE API INCORPORADO PDF**.

   ![Captura de tela do código onde colar](assets/ControlPDF_15.png)

1. Volte para a visualização do código da sandbox do desenvolvedor e copie as linhas de código da `<script>` abaixo:

   ```
   <script type="text/javascript">
       document.addEventListener("adobe_dc_view_sdk.ready",             function(){ 
           var adobeDCView = new AdobeDC.View({clientId:                     "<YOUR_CLIENT_ID>", divId: "adobe-dc-view"});
           adobeDCView.previewFile({
               content:{location: {url: "https://documentcloud.                adobe.com/view-sdk-demo/PDFs/Bodea Brochure.                    pdf"}},
               metaData:{fileName: "Bodea Brochure.pdf"}
           }, {showDownloadPDF: false});
       });
   </script>
   ```

1. Acesse o editor de código e abra o **[!UICONTROL Web]** -> **[!UICONTROL exercício]** -> **[!UICONTROL index.html]** novamente.

1. Cole o `<script>` código no `<body>` do processo na linha 68 do `<div>` tag.

1. Modifique a linha 70 do mesmo **index.html** para incluir a variável clientID criada anteriormente.

   ![Captura de tela da linha 70](assets/ControlPDF_16.png)

1. Modifique a linha 72 do mesmo **index.html** para atualizar o local do arquivo PDF para usar um arquivo local.

   Há um arquivo disponível no tutorial em **/resources/pdfs/whitepaper.pdf**.

1. Salve os arquivos modificados e visualize seu site navegando até **`<your domain>`/summit21/web/exercises/**.

   Você deve ver o white paper técnico renderizado em um modo de janela cheia no navegador.

## Parte 3: Acesso a APIs do Analytics {#part3}

Agora que você criou com êxito uma página da Web que tem a API de incorporação PDF renderizando um PDF, na parte 3 você pode explorar como usar eventos JavaScript para medir a análise para entender como os usuários estão usando PDF.

### Localizando documentação

Há vários eventos JavaScript diferentes disponíveis como parte da API PDF Embed. Você pode acessá-los em [!DNL Adobe Acrobat Services] documentação.

1. Navegue até o [documentação](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html) local.
1. Revise os diferentes tipos de evento disponíveis como parte da API. Eles são úteis para referência e também serão úteis para seus projetos futuros.

   ![Captura de tela do guia de referência](assets/ControlPDF_17.png)

1. Copie o código de exemplo listado no site.

   Use-o como base para nosso código e modifique-o.

   ![Captura de tela de onde copiar o código de amostra](assets/ControlPDF_18.png)

   ```
   const eventOptions = {
     //Pass the PDF analytics events to receive.
      //If no event is passed in listenOn, then all PDF         analytics events will be received.
   listenOn: [ AdobeDC.View.Enum.PDFAnalyticsEvents.    PAGE_VIEW, AdobeDC.View.Enum.PDFAnalyticsEvents.DOCUMENT_DOWNLOAD],
     enablePDFAnalytics: true
   }
   
   
   adobeDCView.registerCallback(
     AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
     function(event) {
       console.log("Type " + event.type);
       console.log("Data " + event.data);
     }, eventOptions
   );
   ```

1. Localize a seção de código adicionada anteriormente que se parece com a abaixo e anexe o código acima após este código no **index.html**:

   ![Captura de tela do código onde colar](assets/ControlPDF_19.png)

1. Carregue a página no navegador da Web e abra o Console para exibir as saídas do console dos diferentes eventos conforme você interage com o visualizador de PDF.

   ![Captura de tela do carregamento da página](assets/ControlPDF_20.png)

   ![Captura de tela do código para carregar a página](assets/ControlPDF_21.png)

### Adicionar opção para capturar eventos

Agora que os eventos estão sendo gerados para console.log, vamos alterar o comportamento com base em quais eventos. Para fazer isso, você usará um exemplo de switch.

1. Navegue até **snippets/eventsSwitch.js** e copie o conteúdo do arquivo no código do tutorial.

   ![Captura de tela do local para copiar o código](assets/ControlPDF_22.png)

1. Cole o código na função de ouvinte de evento.

   ![Captura de tela do código onde colar](assets/ControlPDF_23.png)

1. Confirme se o console é gerado corretamente quando a página é carregada e se você interage com o visualizador de PDF.

### Adobe Analytics

Se quiser adicionar o suporte da Adobe Analytics ao seu visualizador, siga as instruções documentadas no site.

>[!IMPORTANT]
>
>Sua página da Web já precisa ter o Adobe Analytics carregado na página no cabeçalho.

Navegue até o [Documentação do Adobe Analytics](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtodata.html#adobe-analytics) e revise se você já tem o Adobe Analytics ativado em sua página da Web. Siga as instruções para configurar um conjunto de relatórios.

### Google Analytics

![Captura de tela de como integrar com Google Analytics](assets/ControlPDF_24.png)

A API incorporada do Adobe PDF fornece integração imediata com o Adobe Analytics. No entanto, como todos os eventos estão disponíveis como eventos JavaScript, é possível integrar com Google Analytics capturando eventos PDF e usando a função ga() para adicionar o evento ao Adobe Analytics.

1. Navegue até **snippets/eventsSwitchGA.js** para ver como você pode se integrar a Google Analytics.
1. Revise e use esse código como um exemplo se a página da Web for controlada por meio do Adobe Analytics e já estiver incorporada à página da Web.

   ![Captura de tela do código Adobe Analytics](assets/ControlPDF_25.png)

## Parte 4: Adicionar interatividade com base em eventos {#part4}

Na parte 4, você aprenderá a colocar em camadas um paywall no topo do visualizador de PDF que será exibido depois de passar pela segunda página.

### Exemplo do Paywall

Navegue até [exemplo de um PDF atrás de um paywall](https://www3.technologyevaluation.com/research/white-paper/the-forrester-wave-digital-decisioning-platforms-q4-2020.html). Neste exemplo, você aprenderá a adicionar interatividade sobre uma experiência de visualização de PDF.

### Adicionar código do paywall

1. Vá para snippets/paywallCode.html e copie o conteúdo.
1. Procurar `<!-- TODO: EXERCISE 3: INSERT PAYWALL CODE -->` em exercise/index.html.

   ![Captura de tela do local para copiar o código](assets/ControlPDF_26.png)

1. Cole o código copiado após o comentário.
1. Ir para **snippets/paywallCode.js** e copie o conteúdo.

   ![Captura de tela do código onde colar](assets/ControlPDF_27.png)

1. Cole o código nesse local.

### Experimente a demonstração no Paywall

Agora você pode ver a demonstração.

1. Recarregar **index.html** no seu site.
1. Role para baixo até uma página > 2.
1. Mostrar a caixa de diálogo que aparece para desafiar o usuário após a segunda página.

   ![Captura de tela da demonstração](assets/ControlPDF_28.png)

## Recursos adicionais

Recursos adicionais podem ser encontrados [aqui](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html).
