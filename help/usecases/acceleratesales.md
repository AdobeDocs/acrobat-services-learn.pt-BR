---
title: Acelere seu processo de vendas
description: Saiba como acelerar as vendas integrando experiências de documentos a [!DNL Adobe Acrobat Services]
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-10222.jpg
jira: KT-10222
exl-id: 9430748f-9e2a-405f-acac-94b08ad7a5e3
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1755'
ht-degree: 0%

---

# Acelere seu processo de vendas

![Use Case Hero Banner](assets/UseCaseAccelerateSalesHero.jpg)

De white papers a contratos e acordos, vários documentos são necessários durante toda a jornada de compra. Neste tutorial, saiba como [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/) podem integrar experiências documentais em toda essa jornada para ajudar a acelerar as vendas.

## Gerar contratos e ordens de venda a partir de dados

Os acordos de vendas, contratos e outros documentos podem variar muito com base em critérios específicos. Por exemplo, um contrato de venda pode incluir apenas determinados termos com base em critério exclusivo, como estar em um país ou estado específico ou incluir determinados produtos como parte do contrato. Criar manualmente esses documentos ou manter muitas variações de modelos diferentes pode aumentar significativamente os custos legais associados à revisão manual das alterações.

[API de geração de documento Adobe](https://developer.adobe.com/document-services/apis/doc-generation/) permite extrair dados do CRM ou de outro sistema de dados para gerar dinamicamente documentos de venda com base nesses dados.

## Obter credenciais

Comece registrando as credenciais gratuitas dos Serviços Adobe PDF:

1. Navegar [aqui](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) para registrar suas credenciais.
1. Faça logon usando sua Adobe ID.
1. Defina seu nome de credencial (por exemplo, Demonstração dos Acordos de Vendas).

   ![Captura de tela da configuração do nome da sua credencial](assets/accsales_1.png)

1. Escolha um idioma para baixar o código de amostra (por exemplo, Node.js).
1. Marque para concordar com **[!UICONTROL termos de desenvolvedor]**.
1. Selecionar **[!UICONTROL Criar credenciais]**.
Um arquivo é baixado no computador com um arquivo ZIP contendo os arquivos de exemplo pdfservices-api-credentials.json e private.key para autenticação.

   ![Captura de tela das credenciais](assets/accsales_2.png)

1. Selecionar **[!UICONTROL Baixe o suplemento do Microsoft Word]** ou vá para [AppSource](https://appsource.microsoft.com/en-cy/product/office/WA200002654) para instalar.

   >[!NOTE]
   >
   >A instalação do suplemento do Word requer que você tenha permissão para instalar suplementos no Microsoft 365. Se você não tiver permissão, entre em contato com o administrador do Microsoft 365.

## Seus dados

Se estiver extraindo dados de um sistema de dados específico, você deve gerar esses dados como dados JSON ou gerar seu próprio esquema. Esse cenário usa o seguinte conjunto de dados de amostra pré-criado:

```
{
    "salesOrder": {
        "comment": "Make sure to call 555-555-1234 when you arrive. The front door is broken."
    },
    "company": {
        "name":"Home Services Co.",
        "address": {
            "city": "Homestead",
            "state": "NY",
            "zip": "14623",
            "streetAddress": "123 Demohome Street"
        }
    },
    "customer": {
        "address": {
            "city": "Seattle",
            "state": "WA",
            "zip": "98052",
            "streetAddress": "20341 Whitworth Institute 405 N. Whitworth"
        },
        "email": "mailto:jane-doe@xyz.edu",
        "jobTitle": "Professor",
        "name": "Jane Doe",
        "telephone": "(425) 123-4567",
        "url": "http://www.janedoe.com"
    },
    "tax": {
        "state":"WA",
        "rate": 0.08
    },
    "referencesOrder": [
        {
            "description": "Carpet Cleaning Service - 3BR 2BA",
            "totalPaymentDue": {
                "price": 359.54
            },
            "orderedItem": {
                "description": "Carpet Cleaning Service"
            }
        },
        {
            "description": "Home Cleaning Service - 3BR 2BA",
            "totalPaymentDue": {
                "price": 299.99
            },
            "orderedItem": {
                "description": "House Cleaning Service"
            }
        }
    ]
}
```

## Adicionar tags básicas ao documento

Esse cenário usa um documento de Ordem de Venda, que pode ser baixado [aqui](https://github.com/benvanderberg/adobe-document-generation-samples/blob/main/SalesOrder/Exercise/SalesOrder_Base.docx?raw=true).

![Captura de tela do exemplo de documento de ordem de venda](assets/accsales_3.png)

1. Abra o *OrdemDeVenda.docx* documento de amostra no Microsoft Word.
1. Se o plug-in de geração de documento estiver instalado, selecione **[!UICONTROL Geração de documento]** na Faixa de Opções. Se você não vir Geração de documento na faixa de opções, siga estas instruções.
1. Selecionar **[!UICONTROL Introdução]**.
1. Copie os dados de amostra JSON gravados acima no *Dados JSON* campo.

   ![Captura de tela de cópia de dados JSON](assets/accsales_4.png)

Em seguida, navegue até o painel Marcador de geração de documento para inserir marcas no documento.

1. Selecione o texto que deseja substituir (por exemplo, *NOME DA EMPRESA*).
1. No menu *Marcador de Geração de Documento* , procure por &quot;nome&quot;.
1. Na lista de marcas, selecione o nome em empresa.
1. Selecionar **[!UICONTROL Inserir texto]**.

   ![Captura de tela da inserção da marca](assets/accsales_5.png)

   Esse processo coloca uma tag chamada {{company.name}} porque a tag está no caminho no JSON.

   ```
   {
   …
   "company": {
       "name":"Home Services Co.",
       …
   },
   …
   }
   ```

Repita essas ações para algumas das tags adicionais no documento, como ENDEREÇO, CIDADE, ESTADO, CEP etc.

## Visualizar o documento gerado

Diretamente no Microsoft Word, você pode visualizar o documento gerado com base nos dados JSON de amostra.

1. No menu *Marcador de Geração de Documento* , selecione **[!UICONTROL Gerar documento]**. A primeira vez que você for solicitado a fazer logon com sua Adobe ID. Selecionar **[!UICONTROL Fazer logon]** e conclua as solicitações para fazer logon com suas credenciais.

   ![Captura de tela de como visualizar o documento gerado](assets/accsales_6.png)

1. Selecionar **[!UICONTROL Exibir documento]**.

   ![Captura de tela do botão Exibir documento](assets/accsales_7.png)

1. Uma janela do navegador é aberta, permitindo visualizar os resultados do documento.

   ![Captura de tela do documento na janela do navegador](assets/accsales_8.png)

Você pode ver as marcas no documento que foram substituídas pelos dados dos dados de amostra originais.

![Captura de tela de tags substituídas por dados](assets/accsales_9.png)

## Adicionar uma tabela ao modelo

Nesse próximo cenário, adicione uma lista de produtos a uma tabela no documento.

1. Insira o cursor onde a tabela deve ser colocada.
1. No menu *Marcador de Geração de Documento* , selecione **[!UICONTROL Avançado]**.
1. Expandir **[!UICONTROL Tabelas e Listas]**.
1. No menu *Registros de tabela* , selecione *referencesOrder*, que é uma matriz que lista todos os itens do produto.
1. No campo Selecionar registros de coluna, digite para incluir *descrição* e *totalPaymentDue.price* campo.
1. Selecionar **[!UICONTROL Inserir tabela]**.

   ![Captura de tela da tabela de inserção](assets/accsales_10.png)

Edite a tabela para ajustar estilos, tamanhos e outros parâmetros como faria com qualquer outra tabela no Microsoft Word.

## Adicionar cálculo numérico

Cálculos numéricos permitem calcular somas e outros cálculos com base em uma coleção de dados, como uma matriz. Nesse cenário, adicione um campo para calcular o subtotal.

1. Selecione o *$ 0,00* ao lado do subtotal título.
1. No menu *[!UICONTROL Marcador de Geração de Documento]* painel, expandir **[!UICONTROL Cálculos numéricos]**.
1. Sob *[!UICONTROL Selecionar tipo de cálculo]*, escolha **[!UICONTROL Agregação]**.
1. Sob *[!UICONTROL Selecionar tipo]*, escolha **[!UICONTROL Soma]**.
1. Sob *[!UICONTROL Selecionar registros]*, escolha **[!UICONTROL OrdemDeReferências]**.
1. Sob *[!UICONTROL Selecionar item para executar agregação]**, escolha **[!UICONTROL totalPaymentsDue.price]**.
1. Selecionar **[!UICONTROL Inserir Cálculo]**.

Esse processo insere uma tag de cálculo que fornece a soma dos valores. Cálculos mais avançados podem ser feitos usando cálculos JSONata. Por exemplo:

* Subtotal: `${{expr($sum(referencesOrder.totalPaymentDue.price))}}`
Calcula a soma de referencesOrder.totalPaymentDue.price.

* Imposto: `${{expr($sum(referencesOrder.totalPaymentDue.price)*0.08)}}`
Calcula o preço e multiplica por 8% para calcular o imposto.

* Total devido: `${{expr($sum(referencesOrder.totalPaymentDue.price)*1.08)}}`
Calcula o preço e multiplica por 1,08 para calcular o subtotal + imposto.

## Adicionar termos condicionais

As seções condicionais permitem incluir apenas uma frase ou um parágrafo quando uma determinada condição for atendida. Nesse cenário, apenas uma seção é incluída se corresponder a um determinado estado.

1. No documento, localize a seção chamada *DECLARAÇÕES DE PRIVACIDADE DA CALIFÓRNIA*.
1. Selecione a seção com o cursor.

   ![Captura de tela da seleção](assets/accsales_11.png)

1. No menu *[!UICONTROL Marcador de Geração de Documento]*, selecione **[!UICONTROL Avançado]**.
1. Expandir **[!UICONTROL Conteúdo condicional]**.
1. No menu *[!UICONTROL Selecionar registros]* , pesquise e selecione **[!UICONTROL customer.address.state]**.
1. No menu *[!UICONTROL Selecionar operador]* , selecione **=**.
1. No menu *[!UICONTROL Campo Valor]*, tipo *CA*.
1. Selecionar **[!UICONTROL Inserir Condição]**.

A seção Califórnia só aparece no documento gerado se customer.address.state = CA.

Em seguida, selecione a seção para WASHINGTON PRIVACY STATEMENTS e repita as etapas acima, substituindo o valor CA por WA.

## Adicionar uma imagem dinâmica

A API de geração de documento permite inserir imagens dinamicamente a partir de dados. Isso é útil quando você tem submarcas diferentes e deseja alterar logotipos, imagens de retrato ou imagens para torná-las mais relevantes para um determinado setor.

As imagens podem ser transmitidas por um URL no conteúdo de dados ou base64. Este exemplo usa um URL.

1. Posicione o cursor no local em que deseja incluir uma imagem.
1. No menu *[!UICONTROL Marcador de Geração de Documento]* , selecione **[!UICONTROL Avançado]**.
1. Expandir **[!UICONTROL Imagens]**.
1. No menu *[!UICONTROL Selecionar marcas]* escolha **[!UICONTROL logotipo]**.
1. No menu *[!UICONTROL Texto alternativo opcional]* , forneça uma descrição (ou seja, logotipo). Esse processo insere um alocador de espaço de imagem com a seguinte aparência:

   ![Captura de tela da imagem de espaço reservado](assets/accsales_12.png)

No entanto, você deseja definir a imagem dinamicamente em uma imagem que já esteja no layout, o que pode ser feito da seguinte forma:

1. Clique com o botão direito na imagem de espaço reservado inserida.

   ![Captura de tela da imagem de espaço reservado](assets/accsales_13.png)

1. Selecionar **[!UICONTROL Editar texto alternativo]**.
1. No painel, copie o texto com a seguinte aparência:
   `{ "location-path": "logo", "image-props": { "alt-text": "Logo" }}`
1. Selecione uma imagem diferente no documento que deseja que seja dinâmica.

   ![Captura de tela da nova imagem no documento](assets/accsales_14.png)

1. Clique com o botão direito na imagem e selecione **[!UICONTROL Editar texto alternativo]**.
1. Cole o valor no painel.

Esse processo substitui a imagem por uma imagem que esteja na variável de logotipo nos dados.

## Adicionar tags para o Acrobat Sign

O Adobe Acrobat Sign permite que você capture assinaturas eletrônicas em seus documentos. O Acrobat Sign oferece uma maneira fácil de arrastar e soltar campos na interface da Web, mas você também pode controlar a assinatura e o posicionamento de outros campos usando uma Tag de texto. Com o Marcador de geração de documento do Adobe, você pode inserir facilmente esses campos de Marca de texto.

1. Navegue até onde uma assinatura é necessária no documento de amostra.
1. Insira o cursor onde a assinatura é necessária.
1. No menu *[!UICONTROL Marcador de Geração de Documento Adobe]* , selecione **[!UICONTROL Adobe Sign]**.
1. No menu *[!UICONTROL Especificar número de destinatários]* defina o número de destinatários (neste exemplo, é um).
1. No menu *[!UICONTROL Destinatários]* , selecione **[!UICONTROL Signer-1]**.
1. No menu *[!UICONTROL Campo]* tipo, selecione **[!UICONTROL Assinatura]**.
1. Selecionar **[!UICONTROL Inserir tag de texto do Adobe Sign]**.

Uma marca é inserida no documento.

![Captura de tela da marca de assinatura no documento](assets/accsales_15.png)

O Acrobat Sign fornece vários outros tipos de campos que você pode inserir, como campos de data.
1. No menu *Campo* tipo, selecione **[!UICONTROL Data]**.
1. Mova o cursor para cima do local Data no documento.
1. Selecionar **[!UICONTROL Inserir tag de texto do Adobe Sign]**.

![Captura de tela da marca de data no documento](assets/accsales_16.png)

## Gerar seu contrato

Agora você marcou seu documento e está pronto para começar. Esta próxima seção explica como gerar um documento usando as amostras da API de geração de documento para Node.js, no entanto, elas funcionarão em qualquer idioma.

Abra pdfservices-node-sdk-samples-master que foi baixado ao registrar suas credenciais. Os arquivos pdfservices-api-credentials.json e private.key devem ser incluídos nesses arquivos.

1. Abra um Terminal para instalar dependências usando a instalação do npm.
1. Copie o exemplo data.json na pasta resources.
1. Copie o modelo do Word na pasta de recursos.
1. Crie um novo arquivo no diretório raiz da pasta samples chamada generate-salesOrder.js.

```
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
const fs = require('fs');
const path = require('path');

var dataFileName = path.join('resources', '<INSERT JSON FILE');
var outputFileName = path.join('output', 'salesOrder_'+Date.now()+".pdf");
var inputFileName = path.join('resources', '<INSERT DOCX>');

//Loads credentials from the file that you created.
const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Setup input data for the document merge process
const jsonString = fs.readFileSync(dataFileName),
jsonDataForMerge = JSON.parse(jsonString);

// Create an ExecutionContext using credentials
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance
const documentMerge = PDFServicesSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile(inputFileName);
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile(outputFileName))
.catch(err => {
    if(err instanceof PDFServicesSdk.Error.ServiceApiError
        || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
        console.log('Exception encountered while executing operation', err);
    } else {
        console.log('Exception encountered while executing operation', err);
    }
});
```

1. Substituir `<INSERT JSON FILE>` com o nome do arquivo JSON em /resources.
1. Substituir `<INSERT DOCX>` com o nome do arquivo DOCX.
1. Para executar, use o Terminal para executar o nó generate-salesOrder.js.

O arquivo de saída deve estar na pasta /output com o documento gerado corretamente.

## Mais opções

Depois que o documento for gerado, você poderá realizar ações adicionais, como:

* Proteger documento com uma senha
* Compactar PDF se houver imagens grandes
* Capturar assinaturas eletrônicas no documento

Para saber mais sobre algumas das outras ações disponíveis, consulte os scripts na pasta /src nos arquivos de amostra. Você também pode saber mais revisando a documentação das diferentes ações.

## Casos de uso adicionais

[!DNL Adobe Acrobat Services] pode ajudar a simplificar várias partes de um ciclo de vendas com fluxos de trabalho de documentos digitais:

* Use a API incorporada do Adobe PDF para incorporar white papers e outros conteúdos a sites e, ao mesmo tempo, medir e reunir análises sobre o público-alvo
* Use o Acrobat Sign para coletar assinaturas eletrônicas em seus contratos gerados
* Extrair dados de contrato dos documentos do PDF usando a API de extração do Adobe PDF

## Aprendizagem adicional

Quer saber mais? Confira outras maneiras de usar o produto [!DNL Adobe Acrobat Services]:

* Saiba mais com [documentação](https://developer.adobe.com/document-services/docs/overview/)
* Veja mais tutoriais no Adobe Experience League
* Use os scripts de amostra na pasta /src para ver como você pode aproveitar o PDF
* Seguir [Blog da Adobe Tech](https://medium.com/adobetech/tagged/adobe-document-cloud) para obter as dicas e truques mais recentes
* Assine [Clipes de papel (a transmissão mensal em tempo real)](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) para saber mais sobre como automatizar usando [!DNL Adobe Acrobat Services]. =======
* Saiba mais com [documentação](https://developer.adobe.com/document-services/docs/overview/)
* Veja mais tutoriais no Adobe Experience League
* Use os scripts de amostra na pasta /src para ver como você pode aproveitar o PDF
* Seguir [Blog da Adobe Tech](https://medium.com/adobetech/tagged/adobe-document-cloud) para obter as dicas e truques mais recentes
* Assine [Clipes de papel (a transmissão mensal em tempo real)](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) para saber mais sobre como automatizar usando [!DNL Adobe Acrobat Services]
