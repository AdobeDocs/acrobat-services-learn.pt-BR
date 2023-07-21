---
title: Obtendo credenciais para o Microsoft Power Automate
description: Saiba como obter credenciais para começar a usar ou testar os Serviços Adobe PDF
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-10382.jpg
jira: KT-10382
exl-id: 68ec654f-74aa-41b7-9103-44df13402032
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 3%

---

# Obtendo credenciais para o Microsoft Power Automate

[Microsoft Power Automate](https://powerautomate.microsoft.com/pt-br/) fornece uma maneira poderosa para os desenvolvedores e desenvolvedores cidadãos criarem processos automatizados poderosos para melhorar seus negócios sem escrever código. [Serviços da Adobe PDF](https://us.flow.microsoft.com/pt-br/connectors/shared_adobepdftools/adobe-pdf-services/) conector, como parte de [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services), permite que os usuários executem qualquer uma das ações disponíveis na API de serviços da Adobe PDF no Microsoft Power Automate.

Neste tutorial, saiba como obter credenciais para começar a usar ou testar os Serviços da Adobe PDF. Dependendo se você é um usuário de avaliação ou um cliente existente, este tutorial mostra as etapas adequadas para obter as credenciais.

## Como os usuários do Microsoft Power Automate podem começar a usar o conector de serviços da Adobe PDF?

Os usuários existentes do Microsoft Power Automate podem [obter credenciais de avaliação](https://www.adobe.com/go/powerautomate_getstarted) para os Serviços da Adobe PDF. O link acima é um link de inscrição especial para ajudar nesse processo especificamente para usuários do Microsoft Power Automate.

![Logon de usuário do Adobe Developer](assets/credentials_1.png)


>[!IMPORTANT]
> Se estiver fazendo logon para uma versão de avaliação, você deve usar um Adobe ID, e não um Enterprise ID. Se você não for um assinante atual da API de serviços da Adobe PDF e tentar fazer logon com seu Enterprise ID, poderá receber um erro de permissão porque sua empresa não tem direito a usar a API de serviços da Adobe PDF. Por esse motivo, é recomendável usar um Adobe ID pessoal que é gratuito.
>

1. Depois de fazer logon, você será solicitado a selecionar um nome para suas novas credenciais. Insira seu *Nome da credencial*.
1. Marque a caixa de seleção para concordar com os termos de desenvolvedor.
1. Selecionar **[!UICONTROL Criar credenciais]**.

   ![Nomeando suas credenciais](assets/credentials_2.png)

Essas credenciais abrangem cinco valores diferentes:

* ID do cliente (chave de API)
* Segredo do cliente
* ID da organização
* ID da conta técnica
* Base64 (Chave Privada Codificada)

![Novas credenciais](assets/credentials_3.png)

Um arquivo JSON contendo todos esses valores também é baixado automaticamente para o sistema. Este arquivo é nomeado `pdfservices-api-pa-credentials.json` e se parece com:

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

1. No menu da barra lateral, abra o **[!UICONTROL Dados]** e selecione **Conexões**:

   ![Menu Conexões no site do Microsoft Power Automate](assets/credentials_4.png)

1. Selecionar **+ [!UICONTROL Nova Conexão]**.

1. A próxima tela mostra uma lista de possíveis tipos de conexão. No canto superior direito, digite &quot;adobe&quot; para filtrar as opções:

   ![Lista de conexões Adobe](assets/credentials_5.png)

1. Selecionar **[!UICONTROL Serviços do Adobe PDF (visualização)]**.
1. Na janela modal, informe todos os cinco valores gerados anteriormente. Selecionar **[!UICONTROL Criar]** quando terminar.

   ![Campos de formulário para inserir informações de credencial](assets/credentials_6.png)

Agora você está pronto para usar os serviços da Adobe PDF no Microsoft Power Automate.

### Acesso às credenciais após sua criação

Se você já tiver criado credenciais e tiver colocado as credenciais baixadas no local errado, poderá recuperá-las novamente em [Adobe Developer Console](https://developer.adobe.com/console).

1. Depois de fazer logon em [Adobe Developer Console](https://developer.adobe.com/console), primeiro encontre seu projeto e selecione-o.
1. No menu à esquerda, em *Credenciais*, selecione **Conta de serviço (JWT)**:

   ![Credenciais existentes](assets/credentials_7.png)

1. Observe os cinco valores que são apresentados aqui: *ID do cliente*, *Segredo do cliente*, *ID da conta técnica*, *Email da Conta Técnica* e *ID da Organização*.

Infelizmente, você não pode baixar a chave privada anterior, mas pode usar o botão &quot;Gerar um par de chaves pública/privada&quot; para criar uma nova.

## Uso das credenciais existentes dos Serviços Adobe PDF

Se você tiver credenciais existentes de API de serviços da Adobe PDF geradas de [!DNL Adobe Acrobat Services] site, você pode usá-los com o Microsoft Power Automate. Se você baixou um SDK ao se inscrever, suas credenciais existentes vieram na forma de um arquivo JSON com o nome mais provável `pdfservices-api-credentials.json`. Esse arquivo JSON contém as cinco chaves necessárias ao criar suas credenciais de conexão. Copie cada valor do arquivo JSON no campo de conexão correspondente.

O valor da chave privada é proveniente de um segundo arquivo chamado `private.key`.

Você também pode obter os valores do Adobe Developer Console conforme descrito acima.

## Como [!DNL Adobe Acrobat Services] os usuários começam a trabalhar com o Microsoft Power Automate?

Para começar a trabalhar com o Power Automate, primeiro vá para <https://powerautomate.microsoft.com> e use o botão &quot;Start free&quot;. Se você não tiver uma conta da Microsoft, precisará criar uma. Após fazer logon, você verá o painel do Power Automate.

![Painel do PA, exibição inicial](assets/credentials_8.png)

Conforme descrito no início deste tutorial, crie um novo fluxo, adicione uma etapa e localize os Serviços da Adobe PDF. Selecione uma ação e você poderá ser avisado de que uma conta premium é necessária.

![Aviso de conta premium](assets/credentials_9.png)

Como mostra a captura de tela acima, você pode alternar para uma conta corporativa ou configurar uma nova conta de organização. Depois de fazer isso, você poderá adicionar a ação Serviços da Adobe PDF.

Para saber mais sobre como criar seu primeiro fluxo do Microsoft Power Automate com [!DNL Adobe Acrobat Services], consulte [Crie seu primeiro fluxo de trabalho no Microsoft Power Automate](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/create-workflow-power-automate.html).

## Recursos adicionais

Para obter mais ajuda, veja uma lista de recursos adicionais:

* Os primeiros são os documentos do Power Automate para serviços Adobe PDF: <https://docs.microsoft.com/en-us/connectors/adobepdftools/>. Esses recursos complementam o que você aprendeu aqui.
* Precisa de exemplos? Você pode encontrar vários [Modelos do Power Automate](https://powerautomate.microsoft.com/en-us/connectors/details/shared_adobepdftools/adobe-pdf-services/) demonstrando os Serviços PDF.
* Nosso conteúdo de vídeo ao vivo, [Clipes de papel](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF), também contém vídeos demonstrando o uso do Power Automate.
* O [Blog da Adobe Tech](https://medium.com/adobetech/tagged/microsoft-power-automate) A tem muitos artigos sobre como trabalhar com o Power Automate.
* Por fim, consulte o núcleo [Serviços PDF](https://developer.adobe.com/document-services/docs/overview/) também.
