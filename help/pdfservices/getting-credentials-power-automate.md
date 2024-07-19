---
title: Como obter credenciais para o Microsoft Power Automate
description: Saiba como obter credenciais para começar a usar ou testar os Serviços da Adobe PDF
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-10382
thumbnail: KT-10382.jpg
exl-id: 68ec654f-74aa-41b7-9103-44df13402032
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---

# Como obter credenciais para o Microsoft Power Automate

O [Microsoft Power Automate](https://powerautomate.microsoft.com/pt-br/) oferece uma maneira avançada para desenvolvedores e desenvolvedores cidadãos criarem processos automatizados poderosos para melhorar seus negócios sem escrever código. O conector dos [Serviços do Adobe PDF](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/), como parte do [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services), permite que os usuários executem qualquer uma das ações disponíveis na API de Serviços do Adobe PDF no Microsoft Power Automate.

Neste tutorial, saiba como obter credenciais para começar a usar ou testar os Serviços do Adobe PDF. Dependendo se você é um usuário de avaliação ou um cliente existente, este tutorial mostra as etapas adequadas para obter as credenciais.

## Como os usuários do Microsoft Power Automate podem começar a usar o conector de Serviços do Adobe PDF?

Os usuários existentes do Microsoft Power Automate podem [obter credenciais de avaliação](https://www.adobe.com/go/powerautomate_getstarted) para os Serviços do Adobe PDF. O link acima é um link de inscrição especial para ajudar nesse processo especificamente para usuários do Microsoft Power Automate.

![Entrada de usuário da Adobe Developer](assets/credentials_1.png)


>[!IMPORTANT]
> Se estiver fazendo logon para uma avaliação, use uma Adobe ID, não um Enterprise ID. Se você não for um assinante atual da API de Serviços do Adobe PDF e tentar fazer logon com seu Enterprise ID, poderá receber um erro de permissão porque sua empresa não lhe concedeu direito a usar a API de Serviços do Adobe PDF. Por esse motivo, é recomendável usar uma Adobe ID pessoal gratuita.
>

1. Depois de fazer logon, você será solicitado a selecionar um nome para suas novas credenciais. Insira seu *Nome da Credencial*.
1. Marque a caixa de seleção para concordar com os termos do desenvolvedor.
1. Selecione **[!UICONTROL Criar Credenciais]**.

   ![Nomeando suas credenciais](assets/credentials_2.png)

Essas credenciais abrangem cinco valores diferentes:

* ID do cliente (chave de API)
* Segredo do cliente
* ID da organização
* ID da conta técnica
* Base64 (Chave Privada Codificada)

![Novas credenciais](assets/credentials_3.png)

Um arquivo JSON contendo todos esses valores também é baixado automaticamente para o sistema. Este arquivo chama-se `pdfservices-api-pa-credentials.json` e tem a seguinte aparência:

```json
{
 "client_id": "client id value",
 "client_secret": "client secret value",
 "organization_id": "organized id value",
 "account_id": "account id value",
 "base64_encoded_private_key": "base64 version of the private key"
}
```

Armazene este arquivo em um local seguro porque não é possível obter uma cópia da chave privada novamente.

### Adicionar conexão no Microsoft Power Automate

Agora que você tem suas credenciais, pode começar a usá-las nos fluxos do Microsoft Power Automate.

1. No menu da barra lateral, abra o menu **[!UICONTROL Dados]** e selecione **Conexões**:

   ![Menu Conexões no site do Microsoft Power Automate](assets/credentials_4.png)

1. Selecione **+ [!UICONTROL Nova Conexão]**.

1. A tela seguinte mostra uma lista de possíveis tipos de conexão. No canto superior direito, digite “adobe” para filtrar as opções:

   ![Lista de conexões Adobe](assets/credentials_5.png)

1. Selecione **[!UICONTROL Adobe PDF Services (visualização)]**.
1. Na janela modal, insira todos os cinco valores gerados anteriormente. Selecione **[!UICONTROL Criar]** quando terminar.

   ![Campos de formulário para inserir informações de credencial](assets/credentials_6.png)

Agora você está pronto para usar os Serviços do Adobe PDF no Microsoft Power Automate.

### Acessar as credenciais após sua criação

Se você já criou credenciais e não fez o download das credenciais baixadas, poderá recuperá-las novamente no [Adobe Developer Console](https://developer.adobe.com/console).

1. Depois de fazer logon no [Adobe Developer Console](https://developer.adobe.com/console), localize primeiro seu projeto e selecione-o.
1. No menu à esquerda, em *Credenciais*, selecione **Conta de Serviço (JWT)**:

   ![Credenciais existentes](assets/credentials_7.png)

1. Observe os cinco valores apresentados aqui: *ID do cliente*, *Segredo do cliente*, *ID da conta técnica*, *Email da conta técnica* e *ID da organização*.

Infelizmente, você não pode baixar a chave privada anterior, mas pode usar o botão “Gerar um par de chaves público/privado” para criar um novo.

## Usar as credenciais existentes dos Serviços do Adobe PDF

Se você tiver credenciais de API dos Serviços do Adobe PDF geradas no site [!DNL Adobe Acrobat Services], poderá usá-las com o Microsoft Power Automate. Se você baixou um SDK durante a inscrição, suas credenciais existentes vieram na forma de um arquivo JSON provavelmente chamado `pdfservices-api-credentials.json`. Esse arquivo JSON contém as cinco chaves necessárias para criar suas credenciais de conexão. Copie cada valor do arquivo JSON para o campo de conexão correspondente.

Seu valor de chave privada vem de um segundo arquivo chamado `private.key`.

Você também pode obter os valores do Adobe Developer Console conforme descrito acima.

## Como [!DNL Adobe Acrobat Services] usuários podem começar a trabalhar com o Microsoft Power Automate?

Para começar a trabalhar com o Power Automate, primeiro acesse <https://powerautomate.microsoft.com> e use o botão “Iniciar gratuitamente”. Se você não tem uma conta da Microsoft, é necessário criar uma. Após o logon, você verá o painel do Power Automate.

![Painel do PA, exibição inicial](assets/credentials_8.png)

Conforme descrito no início deste tutorial, crie um novo fluxo, adicione uma etapa e localize os Serviços do Adobe PDF. Selecione uma ação e você poderá ser avisado de que uma conta premium é necessária.

![Aviso de conta premium](assets/credentials_9.png)

Como mostra a captura de tela acima, você pode mudar para uma conta corporativa ou configurar uma nova conta de organização. Depois, você pode adicionar a ação Serviços do Adobe PDF.

Para obter uma visão mais detalhada sobre como criar seu primeiro fluxo do Microsoft Power Automate com o [!DNL Adobe Acrobat Services], consulte [Criar seu primeiro fluxo de trabalho no Microsoft Power Automate](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/create-workflow-power-automate.html).

## Recursos adicionais

Para ajudá-lo mais, veja uma lista de recursos adicionais:

* Primeiro, estão os documentos do Adobe PDF Services Power Automate: <https://docs.microsoft.com/en-us/connectors/adobepdftools/>. Esses recursos complementam o que você aprendeu aqui.
* Precisa de exemplos? Você pode encontrar vários [modelos do Power Automate](https://powerautomate.microsoft.com/en-us/connectors/details/shared_adobepdftools/adobe-pdf-services/) demonstrando os Serviços PDF.
* Nosso conteúdo de vídeo ao vivo, [Clipes de papel](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF), também contém vídeos demonstrando o uso do Power Automate.
* O [Blog Tecnológico do Adobe](https://medium.com/adobetech/tagged/microsoft-power-automate) possui muitos artigos sobre como trabalhar com o Power Automate.
* Por fim, consulte também a documentação [Serviços PDF](https://developer.adobe.com/document-services/docs/overview/) do núcleo.
