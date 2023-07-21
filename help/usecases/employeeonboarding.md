---
title: Modernização da integração de funcionários
description: Saiba como modernizar a integração de funcionários com [!DNL Adobe Acrobat Services] APIs
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-10203.jpg
jira: KT-10203
exl-id: 0186b3ee-4915-4edd-8c05-1cbf65648239
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1514'
ht-degree: 1%

---

# Modernização da integração de funcionários

![Use Case Hero Banner](assets/usecaseemployeeonboardinghero.jpg)

Em uma grande organização, a integração de funcionários pode ser um processo grande e lento. Normalmente, há uma mistura de documentação personalizada com material de chapa que deve ser apresentado e assinado por um novo funcionário. Essa mistura de material personalizado e boilerplate requer várias etapas, tirando um tempo valioso das pessoas envolvidas no processo. [!DNL Adobe Acrobat Services] e a Acrobat Sign pode modernizar e automatizar essa abordagem, liberando seu pessoal de RH para tarefas mais importantes. Vejamos como isso é alcançado.

## O que é [!DNL Adobe Acrobat Services]?

[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage) são um conjunto de APIs relacionadas ao trabalho com documentos (e não apenas PDF). Em linhas gerais, esse conjunto de serviços se enquadra em três categorias principais:

* Em primeiro lugar, [Serviços PDF](https://developer.adobe.com/document-services/apis/pdf-services/) conjunto de ferramentas. Esses são métodos &quot;utilitários&quot; para trabalhar com PDF e outros documentos. Os serviços incluem coisas como conversão de e para PDF, execução de OCR e otimização, fusão e divisão de PDF e assim por diante. É a caixa de ferramentas dos recursos de processamento de documentos.
* [API do PDF Extract](https://developer.adobe.com/document-services/apis/pdf-extract/) O usa técnicas avançadas de IA/aprendizado de máquina para analisar um PDF e retornar uma quantidade incrível de detalhes sobre o conteúdo. Isso inclui texto, estilo e informações de posição, além de poder retornar dados tabulares no formato CSV/XLS, bem como recuperar imagens.
* Por último, [API de geração de documento](https://developer.adobe.com/document-services/apis/doc-generation/) permite que os desenvolvedores usem o Microsoft Word como um &quot;modelo&quot;, mescle com seus dados (de qualquer origem) e gere documentos personalizados dinâmicos (PDF e Word).

Os desenvolvedores podem [inscreva-se](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) e experimente todos esses serviços com uma teste grátis. O [!DNL Acrobat Services] A plataforma usa uma API baseada em REST, mas também oferece suporte a SDKs para Node, Java, .NET e Python (somente Extract no momento).

Embora não seja uma API, os desenvolvedores também podem usar o [API de incorporação do PDF](https://developer.adobe.com/document-services/apis/pdf-embed/), que fornece uma experiência de exibição consistente e flexível de documentos com suas páginas da Web.

## O que é o Acrobat Sign?

[Acrobat Sign](https://www.adobe.com/br/sign.html) é líder mundial em serviços de assinatura eletrônica. Você pode enviar documentos para assinatura usando vários fluxos de trabalho diferentes, incluindo várias assinaturas. O Acrobat Sign também é compatível com fluxos de trabalho que exigem assinaturas e informações adicionais. Todos esses recursos são compatíveis com um painel de controle avançado com um sistema de criação flexível.

Como com [!DNL Acrobat Services], a Acrobat Sign tem um [teste grátis](https://www.adobe.com/sign.html#sign_free_trial) que permite aos desenvolvedores testar o processo de assinatura por meio do painel e com uma API baseada em REST fácil de usar.

## Um cenário de integração

Adobe Vamos considerar um cenário real que demonstre como os serviços de podem ajudar. Quando um novo funcionário entra em uma empresa, ele precisa de informações personalizadas e adaptadas à sua função. Além disso, também precisam de material para toda a empresa. Por fim, eles devem demonstrar a aceitação das políticas corporativas assinando os documentos. Vamos dividir isso em etapas concretas:

* Primeiro, é necessária uma carta de apresentação personalizada que cumprimente o novo funcionário pelo nome. A carta deve conter informações sobre o nome, a função, o salário e o local do funcionário.
* A carta personalizada deve ser combinada com uma PDF que contenha informações básicas de toda a empresa (pense em várias políticas de RH, benefícios etc.)
* Deve ser incluído um documento final que solicite a assinatura e a data do funcionário.
* Todos os itens acima devem ser apresentados como um documento enviado ao funcionário para assinatura.

Vamos entrar em detalhes sobre como fazer isso.

## Gerar documentos dinâmicos

Adobe [Geração de documento](https://developer.adobe.com/document-services/apis/doc-generation/) A API permite que os desenvolvedores criem documentos dinâmicos usando o Microsoft Word e uma linguagem de modelo simples, como base para gerar documentos PDF e Word. Aqui está um exemplo de como isso funciona.

Vamos começar com um documento do Word que tem valores codificados. O documento pode ter o estilo que você desejar, incluindo gráficos, tabelas e assim por diante. Aqui está o documento inicial.

![Captura de tela do documento inicial](assets/onboarding_1.png)

A Geração de documento funciona adicionando &quot;tokens&quot; a um documento do Word que é substituído por seus dados. Embora esses tokens possam ser inseridos manualmente, há uma [Suplemento do Microsoft Word](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin/) isso facilita a execução. Ao abri-lo, os autores podem definir tags ou conjuntos de dados que podem ser usados no documento.

![Captura de tela do marcador de documento](assets/onboarding_2.png)

Você pode carregar informações JSON de um arquivo local, copiar em texto JSON ou selecionar para continuar com os dados iniciais. Isso permite definir as tags de maneira ad hoc com base em suas necessidades específicas. Neste exemplo, somente uma tag para nome, função, salário e local é necessária. Isso é feito usando o **Criar Marca** botão:

![Captura de tela da definição de uma tag](assets/onboarding_3.png)

Depois de definir a primeira tag, você pode continuar a definir quantas forem necessárias:

![Captura de tela de tags definidas](assets/onboarding_4.png)

Com as marcas definidas, você seleciona o texto no documento e o substitui pelas marcas quando apropriado. Neste exemplo, as tags são adicionadas para nome, função e salário.

![Captura de tela das tags](assets/onboarding_5.png)

A Geração de documento não é compatível apenas com tags simples, mas também com expressões lógicas. O segundo parágrafo do documento tem texto que se aplica somente às pessoas da Louisiana. Você pode adicionar uma expressão condicional acessando a guia Avançado do Marcador de documento e definindo uma condição. Aqui está como você define uma condição de igualdade simples, mas observe que comparações numéricas e outros tipos de comparação também são suportados.

![Captura de tela da Condição](assets/onboarding_6.png)

Isso pode ser inserido e disposto ao redor do parágrafo:

![Captura de tela da Condição no documento](assets/onboarding_7.png)

Para testar como isso funciona, selecione **Gerar documento**. Na primeira vez que fizer isso, você deverá fazer logon com uma Adobe ID. Após fazer logon, é apresentado o JSON padrão, que pode ser editado manualmente.

![Captura de tela dos dados](assets/onboarding_8.png)

É gerado um PDF que pode ser visualizado ou baixado.

![Captura de tela de PDF gerado](assets/onboarding_9.png)

Embora o Marcador de documentos permita projetar e testar rapidamente, uma vez concluído e em produção, você pode usar um dos SDKs para automatizar esse processo. Embora o código real seja diferente com base em necessidades específicas, aqui está um exemplo de como esse código se parece no Node.js:

```js
 const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');

const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Data would be dynamic...
let data = {
    "name":"Raymond Camden",
    "role":"Lead Developer",
    "salary":9000,
    "location":"Louisiana"
}

// Create an ExecutionContext using credentials.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance.
const documentMerge = PDFServicesSdk.DocumentMerge,
    documentMergeOptions = documentMerge.options,
    options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance.
const documentMergeOperation = documentMerge.Operation.createNew(options);

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile('documentMergeTemplate.docx');
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
    .then(result => result.saveAsFile('documentOutput.pdf'))
    .catch(err => {
        if(err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

Resumindo, o código configura credenciais, cria um objeto de operação e define a entrada e as opções e, em seguida, chama a operação. Finalmente, ele salva o resultado como um PDF. (Os resultados também podem ser enviados como Word.)

A Geração de documentos é compatível com casos de uso muito mais complexos, incluindo a capacidade de ter tabelas e imagens totalmente dinâmicas. Consulte o [documentação](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) para obter mais detalhes.

## Executando operações PDF

O [API de serviços PDF](https://developer.adobe.com/document-services/apis/pdf-services/) fornece um grande conjunto de operações &quot;utilitárias&quot; para trabalhar com PDF. Essas operações incluem:

* Criando PDF a partir de documentos do Office
* Exportação de PDF para documentos do Office
* Combinação e divisão de PDF
* Aplicação de OCR a PDF
* Definindo, removendo e modificando a proteção para PDF
* Excluir, inserir, reordenar e girar páginas
* Otimização de PDF por compactação ou linearização
* Obtendo propriedades de PDF

Para esse cenário, o resultado da chamada de Geração de documento deve ser mesclado com um PDF padrão. Essa operação é bastante simples com os SDKs. Aqui está um exemplo de em Node.js:

```js
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
 
// Initial setup, create credentials instance.
const credentials = PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();
 
// Create an ExecutionContext using credentials and create a new operation instance.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials),
    combineFilesOperation = PDFServicesSdk.CombineFiles.Operation.createNew();
 
// Set operation input from a source file.
const combineSource1 = PDFServicesSdk.FileRef.createFromLocalFile('documentOutput.pdf'),
      combineSource2 = PDFServicesSdk.FileRef.createFromLocalFile('standardCorporate.pdf');

combineFilesOperation.addInput(combineSource1);
combineFilesOperation.addInput(combineSource2);
 
// Execute the operation and Save the result to the specified location.
combineFilesOperation.execute(executionContext)
    .then(result => result.saveAsFile('combineFilesOutput.pdf'))
    .catch(err => {
        if (err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

Esse código pega as duas PDF, as mescla e salva o resultado em uma nova PDF. Simples e fácil! Consulte o [docs](https://developer.adobe.com/document-services/docs/overview/pdf-services-api/) para obter exemplos do que pode ser feito.

## O processo de assinatura

Na etapa final do processo de integração, o funcionário deve assinar um contrato informando que leu e concorda com todas as políticas definidas na. [Acrobat Sign](https://www.adobe.com/br/sign.html) suporta muitos fluxos de trabalho e integrações diferentes, incluindo um automatizado por meio de um [API](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html). Em termos gerais, a parte final do cenário pode ser concluída da seguinte forma:

Primeiro, crie o documento que inclui o formulário que precisa ser assinado. Há várias maneiras de fazer isso, incluindo um visual criado no painel do usuário do Adobe Sign. Outra opção é usar o suplemento Geração de documento do Word para inserir as tags para você. Este exemplo solicita uma assinatura e uma data.

![Captura de tela do documento com tags Sign](assets/onboarding_10.png)

Este documento pode ser salvo como um PDF e, usando o mesmo método descrito acima, unido com todos os documentos. Esse processo cria um pacote coeso que contém uma saudação personalizada, documentação corporativa padrão e uma página final adequada para assinatura.

O modelo pode ser carregado no painel do Acrobat Sign e usado para novos contratos. Usando a API REST, esse documento pode ser enviado ao funcionário potencial para solicitar sua assinatura.

![Captura de tela do documento assinado](assets/onboarding_11.png)

## Experimente

Tudo descrito neste artigo pode ser testado agora mesmo. O [!DNL Adobe Acrobat Services] API [teste grátis](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) atualmente, você recebe 1.000 solicitações gratuitas em um período de seis meses. Acrobat Sign [teste grátis](https://www.adobe.com/sign.html#sign_free_trial) permite enviar contratos com marca-d&#39;água para fins de teste.

Dúvidas? O [fórum de suporte](https://community.adobe.com/t5/document-services-apis/ct-p/ct-Document-Cloud-SDK) é monitorado por desenvolvedores de Adobe e pessoal de suporte todos os dias. Finalmente, para obter mais inspiração, lembre-se de capturar o próximo [Clipes de papel](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) episódio. Há reuniões ao vivo regulares com notícias, demonstrações e palestras com clientes.
