---
title: Introdução à API do Acrobat Sign
description: Saiba como incluir a API do Acrobat Sign em seu aplicativo para coletar assinaturas e outras informações
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8089.jpg
jira: KT-8089
exl-id: ae1cd9db-9f00-4129-a2a1-ceff1c899a83
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '2058'
ht-degree: 2%

---

# Introdução à API do Adobe Sign

![Use Case Hero Banner](assets/UseCaseStartedHero.jpg)

[API do Acrobat Sign](https://www.adobe.io/apis/documentcloud/sign.html) é uma ótima maneira de aprimorar a maneira como você gerencia os contratos assinados. Os desenvolvedores podem integrar facilmente seus sistemas com a API do Sign, que fornece uma maneira confiável e fácil de fazer upload de documentos, enviá-los para assinatura, enviar lembretes e coletar assinaturas eletrônicas.

## O que você pode aprender

Este tutorial prático explica como os desenvolvedores podem usar a API do Sign para aprimorar aplicativos e fluxos de trabalho criados com o [!DNL Adobe Acrobat Services]. [!DNL Acrobat Services] inclui [API de serviços da Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html), [API incorporada do Adobe PDF](https://www.adobe.io/apis/documentcloud/viesdk) (gratuito) e [API de geração de documento Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html).

Mais especificamente, saiba como incluir a API do Acrobat Sign em seu aplicativo para coletar assinaturas e outras informações, como informações de funcionários em um formulário de seguro. São usadas etapas genéricas com solicitações e respostas HTTP simplificadas. Você pode implementar essas solicitações no seu idioma favorito. É possível criar um PDF usando uma combinação de [[!DNL Acrobat Services] APIs](https://www.adobe.io/apis/documentcloud/dcsdk/), faça upload para a API do Sign como um [transitório](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/overview/terminology.md) documento e solicitar assinaturas do usuário final usando o contrato ou [widget](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/overview/terminology.md) fluxo de trabalho.

## Criação de um documento PDF

Comece criando um modelo do Microsoft Word e salvando-o como um PDF. Ou você pode automatizar seu pipeline usando a API de geração de documento para carregar um modelo criado no Word e, em seguida, gerar um documento PDF. A API de geração de documento faz parte da [!DNL Acrobat Services], [gratuito por seis meses e depois por apenas US$ 0,05 por transação de documento](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html).

Neste exemplo, o modelo é apenas um documento simples com alguns campos de signatário para preencher. Nomeie os campos por enquanto e insira os campos reais mais tarde neste tutorial.

![Captura de tela do formulário de seguro com alguns campos](assets/GSASAPI_1.png)

## Descobrindo o ponto de acesso válido da API

Antes de trabalhar com a API do Sign, [crie uma conta de desenvolvedor gratuita](https://acrobat.adobe.com/ca/en/sign/developer-form.html) para acessar a API, teste o intercâmbio e a execução de documentos e teste o recurso de email.

O Adobe distribui a API da Acrobat Sign no mundo todo em muitas unidades de implantação chamadas &quot;shards&quot;. Cada fragmento serve uma conta de cliente, como NA1, NA2, NA3, EU1, JP1, AU1, IN1 e outros. Os nomes dos fragmentos correspondem a localizações geográficas. Esses compartilhamentos compõem o URI base (pontos de acesso) dos endpoints da API.

Para acessar a API do Sign, primeiro você deve descobrir o ponto de acesso correto para sua conta, que pode ser api.na1.adobesign.com, api.na4.adobesign.com, api.eu1.adobesign.com ou outros, dependendo do seu local.

```
  GET /api/rest/v6/baseUris HTTP/1.1
  Host: https://api.adobesign.com
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body (example):

  {
    "apiAccessPoint": "https://api.na4.adobesign.com/", 
    "webAccessPoint": "https://secure.na4.adobesign.com/" 
  }
```

No exemplo acima, há uma resposta com o valor como o ponto de acesso.

>[!IMPORTANT]
>
>Nesse caso, todas as solicitações subsequentes feitas à API do Sign devem usar esse ponto de acesso. Se você usar um ponto de acesso que não atende à sua região, receberá um erro.

## Carregamento de um documento temporário

O Adobe Sign permite criar diferentes fluxos que preparam documentos para assinaturas ou coleta de dados. Independentemente do fluxo do aplicativo, você deve primeiro fazer upload de um documento, que permanece disponível por apenas sete dias. As chamadas de API subsequentes devem fazer referência a este documento temporário.

O documento é carregado usando uma solicitação POST para o `/transientDocuments` ponto de extremidade. A solicitação de várias partes consiste no nome do arquivo, em um fluxo de arquivo e no tipo MIME (mídia) do arquivo de documento. A resposta do ponto de extremidade contém uma ID que identifica o documento.

Além disso, o aplicativo pode especificar um URL de retorno de chamada para que o Acrobat Sign faça ping, notificando o aplicativo quando o processo de assinatura for concluído.


```
  POST /api/rest/v6/transientDocuments HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: multipart/form-data
  File-Name: "Insurance Form.pdf"
  File: "[path]\Insurance Form.pdf"
  Accept: application/json

  Response Body (example):

  {
     "transientDocumentId": "3AAA...BRZuM"
  }
```

## Criação de um formulário web

Os formulários web (anteriormente conhecidos como widgets de assinatura) são documentos hospedados que qualquer pessoa com acesso pode assinar. Os exemplos de formulários web incluem folhas de inscrição, dispensas de atividade e outros documentos que muitas pessoas acessam e assinam online.

Para criar um novo formulário da Web usando a API do Sign, primeiro você deve carregar um documento temporário. A solicitação de POST ao `/widgets` ponto de extremidade usa o `transientDocumentId` .

Neste exemplo, o formulário da Web é `ACTIVE`, mas você pode criá-lo em um dos três estados diferentes:

* RASCUNHO — para criar incrementalmente o formulário da Web

* CRIAÇÃO — para adicionar ou editar campos de formulário no formulário da Web

* ATIVO — para hospedar imediatamente o formulário da Web

As informações sobre os participantes do formulário também devem ser definidas. O `memberInfos` contém dados sobre os participantes, como email. Atualmente, esse conjunto não oferece suporte a mais de um membro. Mas, como o email do signatário do formulário da Web é desconhecido no momento da criação do formulário da Web, o email deve ser deixado em branco, como no exemplo a seguir. O `role` define a função assumida pelos membros em `memberInfos` (como SIGNATÁRIO e APROVADOR).

```
  POST /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: application/json

  Request Body:

  {
    "fileInfos": [
      {
      "transientDocumentId": "YOUR-TRANSIENT-DOCUMENT-ID"
      }
     ],
    "name": "Insurance Form",
      "widgetParticipantSetInfo": {
          "memberInfos": [{
              "email": ""
          }],
      "role": "SIGNER"
      },
      "state": "ACTIVE"
  }

  Response Body (example):

  {
     "id": "CBJ...PXoK2o"
  }
```

Você pode criar um formulário da Web como `DRAFT` ou `AUTHORING`, em seguida, altere seu estado à medida que o formulário passa pelo pipeline do aplicativo. Para alterar um estado do formulário da Web, consulte o [PUT /widgets/{widgetId}/state](https://secure.na4.adobesign.com/public/docs/restapi/v6#!/widgets/updateWidgetState) ponto de extremidade.

## Lendo a URL de hospedagem do Formulário da Web

A próxima etapa é descobrir o URL que hospeda o formulário da Web. O ponto de extremidade /widgets recupera uma lista de dados de Formulário da Web, incluindo o URL hospedado do Formulário da Web que você encaminha aos usuários, para coletar assinaturas e outros dados de formulário.

Este ponto final retorna uma lista, para que você possa localizar o formulário específico por sua ID na `userWidgetList` antes de obter o URL que hospeda o formulário da Web:

```
  GET /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body:

  {
    "userWidgetList": [
      {
        "id": "CBJCHB...FGf",
        "name": "Insurance Form",
        "groupId": "CBJCHB...W86",
        "javascript": "<script type='text/javascript' ...
        "modifiedDate": "2021-03-13T15:52:41Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...Rag*",
        "hidden": false
      },
      {
        "id": "CBJCHB...I8_",
        "name": "Insurance Form",
        "groupId": "CBJCHBCAABAAyhgaehdJ9GTzvNRchxQEGH_H1ya0xW86",
        "javascript": "<script type='text/javascript' language='JavaScript'
        src='https://sec
        "modifiedDate": "2021-03-13T02:47:32Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...AAB",
        "hidden": false
      },
      {
        "id": "CBJCHB...Wmc",
```

## Gerenciamento do formulário da Web

Este formulário é um documento PDF a ser preenchido pelos usuários. No entanto, ainda é necessário informar ao editor do formulário quais campos os usuários devem preencher e onde eles estão localizados no documento:

![Captura de tela do formulário de seguro com alguns campos](assets/GSASAPI_1.png)

O documento acima ainda não mostra os campos. Eles são adicionados ao definir quais campos coletam as informações do signatário, bem como seu tamanho e posição.

Agora, vá para a [Formulários web](https://secure.na4.adobesign.com/public/agreements/#agreement_type=webform) na página &quot;Seus contratos&quot; e localize o formulário criado.

![Captura de tela da guia Acrobat Sign Manage](assets/GSASAPI_2.png)

![Captura de tela da guia Gerenciar do Acrobat Sign com Formulários da Web selecionados](assets/GSASAPI_3.png)

Clique em **Editar** para abrir a página de edição do documento. Os campos predefinidos disponíveis estão no painel direito.

![Captura de tela do ambiente de criação de formulários do Acrobat Sign](assets/GSASAPI_4.png)

O editor permite arrastar e soltar texto e campos de assinatura. Depois de adicionar todos os campos necessários, você pode redimensioná-los e alinhá-los para aprimorar seu formulário. Por fim, clique em **Salvar** para criar o formulário.

![Captura de tela do ambiente de criação de formulários do Acrobat Sign com campos adicionados](assets/GSASAPI_5.png)

## Enviar um formulário web para assinatura

Depois de concluir o formulário da Web, você deve enviá-lo para que os usuários possam preenchê-lo e assiná-lo. Depois de salvar o formulário, você pode exibir e copiar o URL e o código incorporado.

**Copiar URL do formulário da Web**: use esta URL para enviar usuários a uma versão hospedada deste contrato para revisão e assinatura. Por exemplo:

[https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3...babw\*](https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3AAABLblqZhCndYscuKcDMPiVfQlpaGPb-5D7ebE9NUTQ6x6jK7PIs8HCtTzr3HOx8U6D5qqbabw*)

**Copiar código incorporado do Formulário da Web**: adicione o contrato ao seu site copiando esse código e colando-o em seu HTML.

Por exemplo:

```
<iframe
src="https://secure.na4.adobesign.com/public/esignWidget?wid=CBFC
...yx8*&hosted=false" width="100%" height="100%" frameborder="0"
style="border: 0;
overflow: hidden; min-height: 500px; min-width: 600px;"></iframe>
```

![Captura de tela do formulário da Web final](assets/GSASAPI_6.png)

Quando os usuários acessam a versão hospedada do formulário, eles revisam o documento temporário carregado primeiro com os campos posicionados conforme especificado.

![Captura de tela do formulário da Web final](assets/GSASAPI_7.png)

O usuário preenche os campos e assina o formulário.

![Captura de tela do usuário selecionando o campo de assinatura](assets/GSASAPI_8.png)

Em seguida, o usuário assina o documento com uma assinatura armazenada anteriormente ou com uma nova.

![Captura de tela da experiência de assinatura](assets/GSASAPI_9.png)

![Captura de tela da assinatura](assets/GSASAPI_10.png)

Quando o usuário clicar em **Aplicar**, o Adobe instrui-os a abrir o email e confirmar a assinatura. A assinatura permanece pendente até que a confirmação chegue.

![Captura de tela de Apenas mais uma etapa](assets/GSASAPI_11.png)

Essa autenticação adiciona a autenticação de vários fatores e fortalece a segurança do processo de assinatura.

![Captura de tela da mensagem de confirmação](assets/GSASAPI_12.png)

![Captura de tela da mensagem de conclusão](assets/GSASAPI_13.png)

## Leitura de formulários web concluída

Agora é hora de obter os dados de formulário que os usuários preencheram. O `/widgets/{widgetId}/formData` ponto de extremidade recupera os dados inseridos pelo usuário em um formulário interativo quando ele assinou o formulário.

```
GET /api/rest/v6/widgets/{widgetId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
```

O fluxo de arquivo CSV resultante contém dados de formulário.

```
Response Body:
"Agreement
name","completed","email","role","first","last","title","company","agreementId",
"email verified","web form signed/approved"
"Insurance Form","","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","","","2021-03-07 19:32:59"
```

## Criar um contrato

Como alternativa aos Formulários da Web, você pode criar contratos. As seções a seguir demonstram algumas etapas simples para gerenciar contratos usando a API do Sign.

Enviar um documento para destinatários especificados para assinatura ou aprovação cria um contrato. Você pode monitorar o status e a conclusão de um contrato usando APIs.

Você pode criar um contrato usando um [documento temporário](https://helpx.adobe.com/sign/kb/how-to-send-an-agreement-through-REST-API.html), [documento da biblioteca](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/samples/send_using_library_doc.md)ou URL. Neste exemplo, o contrato é baseado no `transientDocumentId`, assim como o formulário da Web criado anteriormente.

```
POST /api/rest/v6/agreements HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
Request Body:
{
    "fileInfos": [
      {
      "transientDocumentId": "{transientDocumentId}"
      }
     ],
    "name": "{agreementName}",
    "participantSetsInfo": [
      {
      "memberInfos": [
          {
          "email": "{signerEmail}"
          }
        ],
        "order": 1,
        "role": "SIGNER"
      }
    ],
    "signatureType": "ESIGN",
    "state": "IN_PROCESS"
  }
```

Neste exemplo, o contrato é criado como IN_PROCESS, mas você pode criá-lo em um dos três estados diferentes:

* RASCUNHO — para criar incrementalmente o contrato antes de enviá-lo

* CRIAÇÃO — para adicionar ou editar campos de formulário no contrato

* IN_PROCESS — para enviar imediatamente o contrato

Para alterar um estado do contrato, use o `PUT /agreements/{agreementId}/state` ponto de extremidade para executar uma das transições de estado permitidas abaixo:

* RASCUNHO para CRIAÇÃO

* CRIANDO em IN_PROCESS

* IN_PROCESS a CANCELADO

O `participantSetsInfo` a propriedade acima fornece emails de pessoas que devem participar do contrato e quais ações devem ser executadas (assinar, aprovar, confirmar etc.). No exemplo acima, há apenas um participante: signatário. As assinaturas manuscritas estão limitadas a quatro por documento.

Diferentemente dos formulários web, quando você cria um contrato, o Adobe o envia automaticamente para assinatura. O ponto de extremidade retorna o identificador exclusivo do contrato.


```
  Response Body:

  {
     id (string): The unique identifier of the agreement
  }
```

## Recuperando informações sobre membros do contrato

Depois de criar um contrato, você pode usar o `/agreements/{agreementId}/members` ponto de extremidade para recuperar informações sobre membros do contrato. Por exemplo, você pode verificar se um participante assinou o contrato.

```
GET /api/rest/v6/agreements/{agreementId}/members HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: application/json
```

O corpo da resposta JSON resultante contém informações sobre os participantes.

```
  Response Body:

  {
     "participantSets":[
        {
           "memberInfos":[
              {
                 "id":"CBJ...xvM",
                 "email":"participant@email.com",
                 "self":false,
                 "securityOption":{
                    "authenticationMethod":"NONE"
                 },
                 "name":"John Doe",
                 "status":"ACTIVE",
                 "createdDate":"2021-03-16T03:48:39Z",
                 "userId":"CBJ...vPv"
              }
           ],
           "id":"CBJ...81x",
           "role":"SIGNER",
           "status":"WAITING_FOR_MY_SIGNATURE",
           "order":1
        }
     ],
```

## Enviar lembretes de contratos

Dependendo das regras de negócios, um prazo pode impedir que os participantes assinem o contrato após uma data específica. Se o contrato tiver uma data de expiração, você poderá lembrar os participantes à medida que a data se aproxima.

Com base nas informações dos membros do contrato que você recebeu após a chamada à `/agreements/{agreementId}/members` ponto de extremidade na última seção, você pode emitir lembretes por email para todos os participantes que ainda não assinaram o contrato.

Uma solicitação de POST ao `/agreements/{agreementId}/reminders` ponto de extremidade cria um lembrete para os participantes especificados de um contrato identificado pelo `agreementId` parâmetro.

```
POST /agreements/{agreementId}/reminders HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
  Request Body:

  {
    "recipientParticipantIds": [{agreementMemberIdList}],
    "agreementId": "{agreementId}",
    "note": "This is a reminder that you haven't signed the agreement yet.",
    "status": "ACTIVE"
  }

  Response Body:

  {
     id (string, optional): An identifier of the reminder resource created on the
     server. If provided in POST or PUT, it will be ignored
  }
```

Depois de publicar o lembrete, os usuários receberão um email com os detalhes do contrato e um link para o contrato.

![Captura de tela da mensagem de lembrete](assets/GSASAPI_14.png)

## Leitura de contratos concluídos

Assim como os formulários web, você pode ler detalhes sobre contratos assinados pelos destinatários. O `/agreements/{agreementId}/formData` ponto de extremidade recupera os dados inseridos pelo usuário quando ele assinou o formulário da Web.

```
GET /api/rest/v6/agreements/{agreementId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
Response Body:
"completed","email","role","first","last","title","company","agreementId"
"2021-03-16 18:11:45","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","CBJCHBCAABAA5Z84zy69q_Ilpuy5DzUAahVfcNZillDt"
```

## Próximas etapas

A API do Acrobat Sign permite gerenciar documentos, formulários web e contratos. Os fluxos de trabalho simplificados, mas completos, criados usando formulários web e contratos são feitos de maneira genérica, permitindo que os desenvolvedores os implementem usando qualquer idioma.

Para obter uma visão geral de como a API do Sign funciona, você pode encontrar exemplos no [Guia do desenvolvedor de uso da API](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/api_usage.md). Esta documentação contém artigos curtos sobre muitas das etapas seguidas ao longo do artigo e outros tópicos relacionados.

A API do Acrobat Sign está disponível em vários níveis de [planos de assinatura eletrônica para um ou vários usuários](https://acrobat.adobe.com/br/pt/sign/pricing/plans.html), para que você escolha o modelo de precificação mais adequado às suas necessidades. Agora que você sabe como é fácil incorporar a API do Sign aos seus aplicativos, pode gostar de outros recursos, como [Acrobat Sign Webhooks](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md), um modelo de programação baseado em push. Em vez de exigir que o aplicativo execute verificações frequentes em eventos do Acrobat Sign, os webhooks permitem que você registre um URL HTTP para o qual a API do Sign executa uma solicitação de retorno de chamada POST sempre que ocorre um evento. Os webhooks possibilitam uma programação robusta ao viabilizar o aplicativo com atualizações instantâneas e em tempo real.

Confira o [preço por uso](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html), para quando sua avaliação gratuita de seis meses da API de serviços da Adobe PDF terminar, e a API Adobe PDF Embed gratuita.

Para adicionar recursos incríveis, como criação automática de documentos e assinatura de documentos, ao seu aplicativo, comece a usar o [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html).
