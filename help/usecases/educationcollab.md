---
title: Colaboração Estudante-Professor
description: Saiba como criar uma plataforma de aprendizado online que permite que professores e estudantes compartilhem recursos facilmente no PDF
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8091
thumbnail: KT-8091.jpg
exl-id: 570a635c-e539-4afc-a475-ecf576415217
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1385'
ht-degree: 0%

---

# Colaboração entre aluno e professor

![Banner do herói do caso de uso](assets/UseCaseStudentHero.jpg)

Instituições de ensino usam documentos do PDF para compartilhar material de aprendizado com estudantes. Os PDF fornecem um formato de documento intercambiável para professores.

A integração da [API de Serviços do Adobe PDF](https://developer.adobe.com/document-services/apis/pdf-services) e da [API de Incorporação do Adobe PDF](https://developer.adobe.com/document-services/apis/pdf-embed) em um aplicativo fornece aos professores e estudantes uma plataforma única para ensinar e aprender. Por exemplo, seu aplicativo pode permitir que os alunos façam perguntas em suas tarefas e boletins de ocorrência e colaborem em tarefas em grupo.

Há um SDK oficial para aplicativos Node.js acessarem a API de serviços de PDF. Isso permite que você converta documentos como Microsoft Word ou Microsoft Excel em
PDF. Além disso, é possível executar operações mais avançadas, como combinar vários relatórios, reorganizar as páginas e proteger PDF. Para obter mais detalhes, consulte a [documentação do produto](https://developer.adobe.com/document-services/homepage/).

## O que você pode aprender

Neste tutorial prático, aprenda a criar uma plataforma de aprendizado online que [permita que professores e estudantes compartilhem recursos facilmente](https://developer.adobe.com/document-services/use-cases/collaboration/student-teacher-collaboration) no PDF. Este tutorial usa um [portal de aprendizado](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers) criado com o tempo de execução de JavaScript Node.js (Node.js) e os Serviços PDF.

O portal de aprendizado tem os seguintes recursos:

* Permite que professores façam upload de recursos

* Permite que os alunos selecionem vários documentos para converter em PDF

* Permite a conversão de documentos em PDF

* Fornece uma visualização de PDF para os alunos em um navegador da Web e permite que eles façam anotações nos documentos sem software adicional

* Permite que os alunos deixem comentários e façam download deles em seus computadores

Saiba como o [!DNL Adobe Acrobat Services] fornece uma experiência completa para seus alunos com PDF. As APIs do [!DNL Acrobat Services] se integram perfeitamente aos seus aplicativos existentes para que os alunos possam carregar, converter, visualizar arquivos e fazer e salvar comentários, tudo na configuração atual.

## APIs e recursos relevantes

* [API de inserção de PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API de Serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Código do projeto](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers)

## Carregando recursos para o portal de aprendizado

Na seção dos professores do portal de aprendizado, os professores podem fazer upload de documentos, como tarefas e testes. Os documentos podem estar em qualquer formato, como Microsoft Word, Microsoft Excel, HTML, vários formatos de imagem e assim por diante.

![Captura de tela da seção de professores do portal de aprendizado](assets/edu_1.png)

Os documentos carregados são armazenados e apresentados aos alunos quando eles abrem sua página da Web.

Para saber como o aplicativo carrega os arquivos, consulte o [código do projeto](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers).

## Conversão de documentos em PDF

Os estudantes podem converter um ou vários documentos de qualquer tipo em PDF, como Microsoft Word, Excel e PowerPoint, bem como outros tipos populares de arquivos de texto e imagem. O portal de aprendizado usa os Serviços PDF para executar a conversão de arquivos em PDF.

Para criar seu próprio portal de aprendizado, primeiro você deve criar suas próprias credenciais. [Inscreva-se](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) para
use a API de serviços PDF gratuitamente por seis meses e até 1.000 transações de documentos. Depois disso, [pague conforme usa](https://developer.adobe.com/document-services/pricing/main) a apenas \$0,05 por transação de documento à medida que a classe aumenta suas atribuições.

Quando um aluno seleciona um documento no painel, ele vê o seguinte:

![Captura de tela da seção de alunos do portal de aprendizado](assets/edu_2.png)

O aluno simplesmente seleciona os documentos para conversão e clica em **Obter relatório**.

O portal de aprendizado converte os documentos em PDF e exibe uma página de relatório, juntamente com uma visualização do arquivo de PDF.

Veja o código de exemplo dessa etapa:

```
async function createPdf(rawFile, outputPdf) {
    try {
            // configurations
            const credentials =  adobe.Credentials
            .serviceAccountCredentialsBuilder()
            .fromFile("./src/pdftools-api-credentials.json")
            .build();
 
            // Capture the credential from app and show create the context
            const executionContext = adobe.ExecutionContext.create(credentials),
            operation = adobe.CreatePDF.Operation.createNew();
 
            // Pass the content as input (stream)
            const input = adobe.FileRef.createFromLocalFile(rawFile);
            operation.setInput(input);
 
            // Async create the PDF
            let result = await operation.execute(executionContext);
            await result.saveAsFile(outputPdf);
    } catch (err) {
            console.log('Exception encountered while executing operation', err);
    }
}
```

O código de exemplo chama o método `createPdf` dentro do manipulador de rota Express para gerar o PDF.

Para saber como esse método é chamado, consulte [o código do projeto](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers/blob/master/src/helpers/pdf.js).

## Visualização dos recursos de aprendizado

A interface de usuário do usa a API incorporada do PDF para renderizar PDF em um navegador da Web. Esta API está disponível para uso gratuito.

A API incorporada PDF usa uma credencial diferente da API de Serviços PDF, portanto, você deve [criar uma credencial](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
antes de poder usá-lo. Depois, você pode usar a opção Incorporar PDF completamente gratuita.

Insira o URL correto do site no token. Caso contrário, talvez você não consiga renderizar os PDF com o token.

A interface de usuário usa a linguagem de modelos [Handlebars](https://handlebarsjs.com/). O PDF será exibido em um navegador da Web.

Aqui está o código para esta etapa:

```
<div id="adobe-dc-view" style="height: 750px; width: 700px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function () {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-credentials-here>", divId: "adobe-dc-view" });
        adobeDCView.previewFile(
            {
                content: {
                    location: { url: "<file-url>" }
                },
                    metaData: { fileName: "<file-name>" }
            },
           );
    });
</script>
 
<p>Material has been generated, <a href="/students/download/{{filename}}" target="_blank">click here</a> to download it.
</p>
```

Este código exibe a saída de PDF e o link para baixar o relatório de PDF, conforme mostrado na captura de tela abaixo:

![Captura de tela de visualização de PDF de estudante](assets/edu_3.png)

Os alunos devem ser capazes de baixar o relatório ou trabalhar no material aqui.

## Anotar documentos do PDF

Uma plataforma de aprendizado deve oferecer suporte a anotações, comentários e discussões básicas em PDF. A API de incorporação de PDF fornece todos esses recursos. Isso ativa o suporte a anotações usando o `showAnnotationTools`, permitindo que professores e estudantes comentem nos documentos e arquivem comentários como parte do PDF.

Para habilitar anotações em documentos PDF, basta passar o argumento `showAnnotationTools` : true para o método `previewFile`. Isso exibe a ferramenta de anotações no visualizador de PDF. Acesse essa ferramenta a partir do menu de três pontos no canto superior direito da visualização.

![Captura de tela das ferramentas de comentários no PDF](assets/edu_4.png)

Nos documentos carregados pelos professores, os alunos podem realçar textos, adicionar comentários e assim por diante.

![Captura de tela da adição de comentários no PDF](assets/edu_5.png)

Na captura de tela acima, o usuário é rotulado como “Convidado”, mas você pode configurar perfis para usuários, como estudantes e professores.

Quando um estudante aplica uma anotação, a API de Incorporação do PDF exibe um botão **Salvar** no banner superior. Salvar adiciona as anotações ao arquivo. Tente clicar em **Salvar** para ver como o arquivo é salvo com a anotação incorporada no relatório.

Os alunos podem usar anotações para fazer perguntas ou compartilhar seus comentários sobre o material de aprendizado.

## Rastrear o uso do documento

É importante que professores e escolas vejam como os estudantes estão usando as plataformas online. Isso ajuda os professores a apoiar seus alunos com recursos que os ajudam a ter um melhor desempenho em suas tarefas. A API incorporada do PDF integra-se à análise que você pode usar para medir todos os eventos que ocorrem, como quando os usuários estão abrindo, lendo e fechando documentos. Com a API dos Serviços de PDF, os professores também podem desativar a modificação de impressão, download e arquivos para ajudar a manter a integridade acadêmica.

Se você tiver uma licença do [Adobe Analytics](https://developer.adobe.com/analytics-apis/docs/2.0/), poderá usar sua [integração imediata](https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfembed/controlpdfexperience#adobe-analytics). Caso contrário, use retornos de chamada para integrar seus Serviços PDF com outros provedores de análise, como o [Google](https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfembed/controlpdfexperience#google-analytics).

Para habilitar a medição de eventos de documento, anexe os manipuladores de eventos usando o método `registerCallback` com a instância de Exibição de DC de Adobe. Você pode exibir métricas básicas, como abrir um documento ou ler uma página, no console. Você também pode salvar as métricas em um log ou publicá-las em outros armazenamentos de análise.

Aqui está o código de exemplo para anexar os manipuladores de eventos:

```
adobeDCView.registerCallback(
    AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
    function(event) {
           console.log(event);
    },
    {
           enablePDFAnalytics: true
    }
);
```

Os professores podem ver quantos estudantes viram a tarefa, quantos passaram por todas as páginas de suas anotações e outros detalhes valiosos.

Aqui está uma captura de tela do console do navegador da Web:

![Captura de tela do console do navegador da Web](assets/edu_6.png)

Essa captura de tela mostra que o aluno abriu o arquivo de atribuição e leu a primeira página, que não rolava para páginas adicionais ou o documento tinha apenas uma página, e então baixou o arquivo. Você pode coletar essas métricas para realizar análises e estudar o comportamento de seus alunos.

Além disso, o [Adobe Analytics](https://business.adobe.com/products/adobe-analytics.html) está integrado com a API incorporada do PDF, portanto, se você tiver uma assinatura do Adobe Analytics suite, poderá publicar suas métricas na sua assinatura. Para publicar as métricas no Adobe Analytics, você só precisa passar a ID do conjunto para o construtor de API incorporado do PDF. (Observe que você deve usar suas credenciais de API de Incorporação de PDF, não suas credenciais de API de Serviços de PDF).

Aqui está um exemplo de código que mostra como passar a ID do conjunto para o construtor de API incorporado PDF:

```
var adobeDCView = new AdobeDC.View({
    clientId: "<your-adobe-dc-credential>",
    divId: "<#element>"
    reportSuiteId: <your-id-here>,
}); 
```

## Próximas etapas

Este tutorial prático analisou como usar a API de Serviços de PDF e a API Incorporada de PDF para criar um portal de aprendizado, facilitando a [colaboração eficaz entre estudantes e professores](https://developer.adobe.com/document-services/use-cases/collaboration/student-teacher-collaboration). Com esse portal, os professores podem fazer upload do material de aprendizado em qualquer formato e convertê-lo em PDF usando a API de serviços do PDF. Os alunos podem visualizar esses PDF usando a API incorporada do PDF.

Agora que você sabe como anotar relatórios de PDF, arquivar as anotações e controlar o uso de relatórios de PDF, pode começar a implementar essas soluções em seus próprios projetos.

Você pode usar as APIs do [!DNL Adobe Acrobat Services] para criar experiências de PDF interativas e de fácil utilização em seu site. Aproveite o uso gratuito da API de Serviços do Adobe PDF por seis meses e, a seguir, apenas [pague conforme usa](https://developer.adobe.com/document-services/pricing/main) (por meio do AWS ou de um contrato direto) por apenas \$0,05 por transação de documento. Use o Adobe PDF Embed gratuitamente e sem limite de tempo. Crie uma conta gratuita para [começar](https://www.adobe.com/go/dcsdks_credentials) hoje.
