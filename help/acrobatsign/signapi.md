---
title: Introdução à API do Acrobat Sign
description: Saiba como incluir a API do Acrobat Sign em seu aplicativo para coletar assinaturas e outras informações
role: Developer
level: Intermediate
type: Tutorial
feature: Acrobat Sign API
thumbnail: KT-8089.jpg
jira: KT-8089
exl-id: ae1cd9db-9f00-4129-a2a1-ceff1c899a83
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '2058'
ht-degree: 2%

---

# Introdução à API do Adobe Sign

![Banner do herói do caso de uso](assets/UseCaseStartedHero.jpg)

[API do Acrobat Sign](https://www.adobe.io/apis/documentcloud/sign.html) é uma ótima maneira de aprimorar a forma de gerenciar contratos assinados. Os desenvolvedores podem integrar facilmente seus sistemas com a API do Sign, que fornece uma maneira confiável e fácil de fazer upload de documentos, enviá-los para assinatura, enviar lembretes e coletar assinaturas eletrônicas.

## O que você pode aprender

Este tutorial prático explica como os desenvolvedores podem usar a API do Sign para aprimorar aplicativos e fluxos de trabalho criados com o [!DNL Adobe Acrobat Services]. [!DNL Acrobat Services] inclui [API de Serviços do Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html), [API incorporada do Adobe PDF](https://www.adobe.io/apis/documentcloud/viesdk) (livre), e [API de geração de documento Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html).

Mais especificamente, saiba como incluir a API do Acrobat Sign no seu aplicativo para coletar assinaturas e outras informações, como informações de funcionários em um formulário de seguro. Etapas genéricas com solicitações e respostas HTTP simplificadas são usadas. Você pode implementar essas solicitações em seu idioma favorito. É possível criar um PDF usando uma combinação de [[!DNL Acrobat Services] APIs](https://www.adobe.io/apis/documentcloud/dcsdk/), faça upload para a API do Sign como [transiente](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/overview/terminology.md) e solicite assinaturas do usuário final usando o contrato ou [widget](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/overview/terminology.md) fluxo de trabalho.

## Criação de um documento PDF

Comece criando um modelo do Microsoft Word e salvando-o como um PDF. Ou você pode automatizar seu pipeline usando a API de geração de documento para carregar um modelo criado no Word e depois gerar um documento PDF. A API de geração de documento faz parte de [!DNL Acrobat Services], [grátis por seis meses, depois pague conforme usa por apenas US$ 0,05 por transação de documento](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html).

Neste exemplo, o modelo é apenas um documento simples com alguns campos de signatário para preencher. Nomeie os campos por enquanto e insira os campos reais neste tutorial.

![Captura de tela do formulário de seguro com alguns campos](assets/GSASAPI_1.png)

## Descobrindo o ponto de acesso válido da API

Antes de trabalhar com a API do Sign, [crie uma conta gratuita de desenvolvedor](https://acrobat.adobe.com/ca/en/sign/developer-form.html) para acessar a API, teste a troca e a execução de documentos e teste o recurso de envio por email.

O Adobe distribui a API do Acrobat Sign em várias unidades de implantação, chamadas de “fragmentos”. Cada fragmento atende à conta de um cliente, como NA1, NA2, NA3, EU1, JP1, AU1, IN1 e outros. Os nomes dos fragmentos correspondem a localizações geográficas. Esses fragmentos compõem o URI base (pontos de acesso) dos endpoints da API.

Para acessar a API do Sign, primeiro você deve descobrir o ponto de acesso correto para sua conta, que pode ser api.na1.adobesign.com, api.na4.adobesign.com, api.eu1.adobesign.com ou outros, dependendo de sua localização.

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

O Adobe Sign permite que você crie diferentes fluxos que preparam documentos para assinaturas ou coleta de dados. Independentemente do fluxo do aplicativo, primeiro você deve fazer upload de um documento, que permanece disponível por apenas sete dias. As chamadas de API subsequentes devem fazer referência a este documento temporário.

O documento é carregado usando uma solicitação POST para o `/transientDocuments` ponto de extremidade. A solicitação com várias partes consiste no nome do arquivo, em um fluxo de arquivos e no tipo MIME (mídia) do arquivo de documento. A resposta de ponto de extremidade contém uma ID que identifica o documento.

Além disso, o aplicativo pode especificar um URL de retorno de chamada para o Acrobat Sign fazer ping, notificando o aplicativo quando o processo de assinatura for concluído.


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

Formulários web (conhecidos anteriormente como widgets de assinatura) são documentos hospedados que qualquer pessoa com acesso pode assinar. Exemplos de formulários web incluem folhas de inscrição, isenções e outros documentos que muitas pessoas acessam e assinam online.

Para criar um novo formulário web usando a API do Sign, primeiro carregue um documento temporário. A solicitação de POST para o `/widgets` O ponto de extremidade usa o `transientDocumentId` .

Neste exemplo, o formulário da Web é `ACTIVE`, mas você pode criá-lo em um de três estados diferentes:

* RASCUNHO — para criar progressivamente o formulário da Web

* AUTHORING — para adicionar ou editar campos de formulário no formulário da Web

* ATIVO — para hospedar imediatamente o formulário da Web

As informações sobre os participantes do formulário também devem ser definidas. O `memberInfos` contém dados sobre os participantes, como e-mail. No momento, esse conjunto não dá suporte a mais de um membro. Mas, como o email do signatário do formulário da Web é desconhecido no momento da criação do formulário, o email deve ficar vazio, como no exemplo a seguir. O `role` propriedade define a função assumida pelos membros em `memberInfos` (como SIGNATÁRIO e APROVADOR).

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

Você pode criar um formulário da Web como `DRAFT` ou `AUTHORING`e, em seguida, altere seu estado à medida que o formulário passa pelo pipeline do aplicativo. Para alterar o estado de um formulário web, consulte o [PUT /widgets/{widgetId}/state](https://secure.na4.adobesign.com/public/docs/restapi/v6#!/widgets/updateWidgetState) ponto de extremidade.

## Leitura do URL de hospedagem do Formulário Web

A próxima etapa é descobrir o URL que hospeda o formulário da Web. O ponto de extremidade /widgets recupera uma lista de dados de formulário da Web, incluindo o URL hospedado do formulário da Web que você encaminha aos usuários, para coletar assinaturas e outros dados de formulário.

Este ponto final retorna uma lista, de modo que você possa localizar o formulário específico por sua ID na `userWidgetList` antes de obter o URL que hospeda o formulário da Web:

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

## Gerenciando seu formulário da Web

Este formulário é um documento PDF para os usuários preencherem. No entanto, você ainda precisa informar ao editor do formulário quais campos os usuários devem preencher e onde eles estão localizados no documento:

![Captura de tela do formulário de seguro com alguns campos](assets/GSASAPI_1.png)

O documento acima ainda não mostra os campos. Eles são adicionados durante a definição de quais campos coletam as informações do signatário, bem como seu tamanho e posição.

Agora, vá para a [Formulários web](https://secure.na4.adobesign.com/public/agreements/#agreement_type=webform) na página “Seus contratos” e localize o formulário criado.

![Captura de tela da guia Gerenciar do Acrobat Sign](assets/GSASAPI_2.png)

![Captura de tela da guia Gerenciar do Acrobat Sign com Formulários web selecionados](assets/GSASAPI_3.png)

Clique em **Editar** para abrir a página de edição do documento. Os campos predefinidos disponíveis estão no painel direito.

![Captura de tela do ambiente de criação de formulários do Acrobat Sign](assets/GSASAPI_4.png)

O editor permite arrastar e soltar texto e campos de assinatura. Depois de adicionar todos os campos necessários, você pode redimensioná-los e alinhá-los para polir seu formulário. Por fim, clique em **Salvar** para criar o formulário.

![Captura de tela do ambiente de criação de formulários do Acrobat Sign com campos de formulário adicionados](assets/GSASAPI_5.png)

## Enviar um formulário web para assinatura

Depois de concluir o formulário da Web, você deve enviá-lo para que os usuários possam preenchê-lo e assiná-lo. Depois de salvar o formulário, você pode exibir e copiar o URL e o código incorporado.

**Copiar URL do formulário da Web**: use este URL para enviar usuários para uma versão hospedada deste contrato para revisão e assinatura. Por exemplo:

[https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3...babw\*](https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3AAABLblqZhCndYscuKcDMPiVfQlpaGPb-5D7ebE9NUTQ6x6jK7PIs8HCtTzr3HOx8U6D5qqbabw*)

**Copiar código incorporado do formulário da Web**: adicione o contrato ao seu site copiando este código e colando-o em seu HTML.

Por exemplo:

```
<iframe
src="https://secure.na4.adobesign.com/public/esignWidget?wid=CBFC
...yx8*&hosted=false" width="100%" height="100%" frameborder="0"
style="border: 0;
overflow: hidden; min-height: 500px; min-width: 600px;"></iframe>
```

![Captura de tela do formulário web final](assets/GSASAPI_6.png)

Quando os usuários acessam a versão hospedada do formulário, eles revisam o documento temporário carregado primeiro com os campos posicionados conforme especificado.

![Captura de tela do formulário web final](assets/GSASAPI_7.png)

O usuário preenche os campos e assina o formulário.

![Captura de tela do usuário selecionando o campo de assinatura](assets/GSASAPI_8.png)

Em seguida, o usuário assina o documento com uma assinatura armazenada anteriormente ou com uma nova.

![Captura de tela da experiência de assinatura](assets/GSASAPI_9.png)

![Captura de tela da assinatura](assets/GSASAPI_10.png)

Quando o usuário clicar em **Aplicar**, o Adobe instrui o usuário a abrir o email e confirmar a assinatura. A assinatura permanece pendente até que a confirmação chegue.

![Captura de tela de apenas mais uma etapa](assets/GSASAPI_11.png)

Essa autenticação adiciona autenticação de vários fatores e fortalece a segurança do processo de assinatura.

![Imagem da mensagem de confirmação](assets/GSASAPI_12.png)

![Captura de tela da mensagem de conclusão](assets/GSASAPI_13.png)

## Leitura de formulários web concluídos

Agora é hora de obter os dados do formulário que os usuários preencheram. O `/widgets/{widgetId}/formData` o ponto de extremidade recupera os dados inseridos pelo usuário em um formulário interativo quando ele assinou o formulário.

```
GET /api/rest/v6/widgets/{widgetId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
```

O fluxo de arquivos CSV resultante contém dados de formulário.

```
Response Body:
"Agreement
name","completed","email","role","first","last","title","company","agreementId",
"email verified","web form signed/approved"
"Insurance Form","","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","","","2021-03-07 19:32:59"
```

## Criar um contrato

Como alternativa aos Formulários web, você pode criar contratos. As seções a seguir demonstram algumas etapas simples para gerenciar contratos usando a API do Sign.

Enviar um documento para destinatários especificados para assinatura ou aprovação cria um contrato. Você pode monitorar o status e a conclusão de um contrato usando APIs.

Você pode criar um contrato usando um [documento temporário](https://helpx.adobe.com/sign/kb/how-to-send-an-agreement-through-REST-API.html), [documento da biblioteca](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/samples/send_using_library_doc.md), ou URL. Neste exemplo, o contrato tem como base o `transientDocumentId`, assim como o formulário da Web criado anteriormente.

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

* RASCUNHO — para criar o contrato de forma incremental antes de enviá-lo

* AUTHORING — para adicionar ou editar campos de formulário no contrato

* IN_PROCESS — para enviar imediatamente o contrato

Para alterar o estado de um contrato, use o `PUT /agreements/{agreementId}/state` ponto de extremidade para executar uma das transições de estado permitidas abaixo:

* RASCUNHO PARA CRIAÇÃO

* AUTHORING to IN_PROCESS

* IN_PROCESS a SER CANCELADO

O `participantSetsInfo` a propriedade acima fornece emails de pessoas que devem participar do contrato e quais ações executar (assinar, aprovar, reconhecer e assim por diante). No exemplo acima, há apenas um participante: o signatário. As assinaturas manuscritas estão limitadas a quatro por documento.

Diferentemente dos Formulários web, ao criar um contrato, o Adobe o envia automaticamente para assinatura. O ponto de extremidade retorna o identificador exclusivo do contrato.


```
  Response Body:

  {
     id (string): The unique identifier of the agreement
  }
```

## Recuperando informações sobre membros do contrato

Depois de criar um contrato, você pode usar o `/agreements/{agreementId}/members` ponto de extremidade para recuperar informações sobre membros do contrato. Por exemplo, é possível verificar se um participante assinou o contrato.

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

## Enviar lembretes de contrato

Dependendo das regras de negócios, um prazo pode impedir que os participantes assinem o contrato após uma data específica. Se o contrato tiver uma data de expiração, você pode lembrar os participantes quando essa data se aproximar.

Com base nas informações dos membros do contrato recebidas após a chamada para o `/agreements/{agreementId}/members` ponto de extremidade na última seção, você pode emitir lembretes de email para todos os participantes que ainda não assinaram o contrato.

Uma solicitação de POST para o `/agreements/{agreementId}/reminders` cria um lembrete para os participantes especificados de um contrato identificado pelo `agreementId` parâmetro.

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

![Captura de tela da mensagem de Lembrete](assets/GSASAPI_14.png)

## Leitura de contratos concluídos

Assim como os Formulários web, você pode ler os detalhes dos contratos que os destinatários assinaram. O `/agreements/{agreementId}/formData` O ponto de extremidade recupera os dados inseridos pelo usuário ao assinar o formulário da Web.

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

A API do Acrobat Sign permite gerenciar documentos, formulários web e contratos. Os fluxos de trabalho simplificados, mas completos, criados usando formulários web e contratos são feitos de uma maneira genérica que permite aos desenvolvedores implementá-los em qualquer idioma.

Para obter uma visão geral de como a API do Sign funciona, você pode encontrar exemplos na seção [Guia do Desenvolvedor de Uso da API](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/api_usage.md). Esta documentação contém artigos curtos sobre muitas das etapas seguidas ao longo do artigo, além de outros tópicos relacionados.

A API do Acrobat Sign está disponível por vários níveis de [planos de assinatura eletrônica para um único usuário e vários usuários](https://acrobat.adobe.com/br/pt/sign/pricing/plans.html), para que você possa escolher o modelo de preço mais adequado às suas necessidades. Agora que você sabe como é fácil incorporar a API do Sign em seus aplicativos, pode estar interessado em outros recursos, como [Acrobat Sign Webhooks](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md), um modelo de programação baseado em push. Em vez de exigir que o aplicativo execute verificações frequentes em eventos do Acrobat Sign, os webhooks permitem que você registre um URL HTTP para o qual a API do Sign executa uma solicitação de retorno de chamada de POST sempre que um evento ocorre. Os webhooks permitem programação robusta ao potencializar seu aplicativo com atualizações instantâneas e em tempo real.

Confira o [preços pré-pagos](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html), por quando seu teste grátis de seis meses da API de serviços do Adobe PDF terminar, e a API incorporada gratuita do Adobe PDF.

Para adicionar recursos interessantes, como criação automática de documentos e assinatura de documentos, ao seu aplicativo, comece a usar [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html).
