---
title: Criação e Edição de Relatórios
description: Saiba como gerar relatórios de PDF em seu site para os clientes
role: Developer
level: Intermediate
type: Tutorial
feature: Use Cases
thumbnail: KT-8093.jpg
jira: KT-8093
exl-id: 2f2bf1c2-1b33-4eee-9fd2-5d0b77e6b0a9
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1346'
ht-degree: 1%

---

# Criação e edição de relatórios

![Banner do herói do caso de uso](assets/UseCaseReportHero.jpg)

Finanças, educação, marketing e outros setores usam PDF para compartilhar dados com seus clientes e partes interessadas. Os PDF facilitam o compartilhamento de documentos avançados, com tabelas, gráficos e conteúdo interativo, em um formato que todos podem visualizar. [!DNL Adobe Acrobat Services] As APIs ajudam essas empresas a gerar relatórios de PDF compartilháveis do Microsoft Word, Microsoft Excel, gráficos e outros formatos de documentos diversos.

Diga você [executar uma empresa de rastreamento de redes sociais](https://www.adobe.io/apis/documentcloud/dcsdk/on-demand-report-creation.html). Seus clientes fazem logon em uma parte do site protegida por senha para exibir a análise da campanha. Muitas vezes, eles desejam compartilhar essas estatísticas com seus executivos, acionistas, doadores ou outras partes interessadas. Documentos de PDF para download são uma ótima maneira de seus clientes compartilharem números, gráficos e muito mais.

Ao incorporar [API de serviços PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) no seu site, você pode gerar relatórios de PDF para cada cliente em qualquer lugar. Você pode criar PDF e combiná-los em um único relatório prático para que seus clientes baixem e passem adiante às partes interessadas.

## O que você pode aprender

Neste tutorial prático, aprenda a usar o SDK de serviços de PDF em um ambiente Node.js e Express.js (com apenas alguns JavaScript, HTML e CSS) para adicionar rápida e facilmente funcionalidade orientada a PDF a um site existente. Este site tem uma página onde os administradores carregam relatórios, uma área onde os clientes exibem uma lista de relatórios disponíveis e selecionam documentos para converter em PDF e endpoints úteis para baixar PDF gerados pelo sistema.

## APIs e recursos relevantes

* [API de serviços PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

## Painel de relatórios do Campaign para clientes

>[!NOTE]
>
>Este tutorial não é sobre as práticas recomendadas do Node.js ou como proteger seus aplicativos Web. Algumas áreas do site são expostas para uso público e a nomeação de documentos pode não ser amigável para produção. Para discutir a melhor abordagem possível para projetar um sistema como este, consulte seus arquitetos e engenheiros.

Aqui, você tem um aplicativo web Express.js básico que tem uma área de relatórios de clientes e uma seção de administrador. Este aplicativo pode mostrar relatórios para campanhas de mídia social. Por exemplo, ele pode demonstrar o número de vezes que um anúncio é clicado.

![Captura de tela de como obter relatórios personalizados](assets/report_1.png)

Você pode baixar este projeto no [Repositório GitHub](https://github.com/afzaal-ahmad-zeeshan/express-adobe-pdf-tools).

Agora, vamos explorar como publicar os relatórios.

## Carregando relatórios

Para simplificar, use aqui somente o upload e o processamento baseados no sistema de arquivos. No Express.js, você pode usar o módulo fs para listar todos os arquivos disponíveis em um diretório.

Na mesma página, permita que o administrador faça upload de arquivos de relatório para o servidor para que os clientes vejam. Esses arquivos podem estar em muitos formatos diferentes, como Microsoft Word, Microsoft Excel, HTML e [outros formatos de dados]https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf) incluindo arquivos gráficos. A página do administrador é semelhante a esta:

![Captura de tela do recurso do administrador](assets/report_2.png)

>[!NOTE]
>
>Proteja seus URLs com senha ou use o pacote passport do npm para proteger seu aplicativo por trás da camada de autenticação e autorização.

Quando o administrador seleciona e faz upload de um arquivo, ele é movido para um repositório público onde outras pessoas podem acessá-lo. Use o mesmo repositório para publicar documentos da página de administrador e listar os relatórios de marketing disponíveis para os clientes. Este código é:

```
router.get('/', (req, res) => {
try {
let files = fs.readdirSync('./public/documents/raw') // read the files
res.status(200).render("reports", { page: 'reports', files: files });
} catch (error) {
res.status(500).render("crash", { error: error });
}
});
```

Esse código lista todos os arquivos e renderiza uma visualização da lista de arquivos.

## Selecionar relatórios

Do lado do usuário, você tem um formulário para os clientes selecionarem os documentos que desejam incluir em seus relatórios de campanha de redes sociais. Para simplificar, na página de exemplo, mostre apenas o nome do documento e uma caixa de seleção para selecionar o documento. Os clientes podem selecionar um único relatório ou vários relatórios para combinar em um único documento PDF.

Para uma interface de usuário mais avançada, você também pode mostrar uma visualização do relatório aqui.

![Captura de tela da capacidade do cliente](assets/report_3.png)

## Geração de um relatório de PDF

Use o SDK dos Serviços de PDF para criar os relatórios de PDF a partir de suas entradas de dados. Os dados (conforme mostrado nas capturas de tela acima) podem vir de vários formatos de dados, como Microsoft Word, Microsoft Excel, HTML, gráficos e muito mais. Comece instalando o pacote npm para PDF Services SDK.

```
$ npm install --save @adobe/documentservices-pdftools-node-sdk
```

Antes de iniciar, você deve ter credenciais de API, [sem Adobe](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred). Use seu [!DNL Acrobat Services] conta [grátis por seis meses e depois pague conforme usa](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) por apenas \$0,05 por transação de documento.

Baixe o arquivo e extraia o arquivo JSON para as credenciais e a chave privada. No projeto de amostra, coloque o arquivo no diretório src.

![Captura de tela do diretório src](assets/report_4.png)

Agora que você tem as credenciais configuradas, pode gravar a tarefa de conversão de PDF. Para esta demonstração, você tem duas operações que devem ser executadas no aplicativo:

* Converter documentos raw em arquivos PDF

* Combinar vários arquivos de PDF em um único relatório

O procedimento geral é semelhante para executar qualquer operação. A única diferença é o serviço que você usa. No código a seguir, você converte o documento raw em um arquivo PDF:

```
async function createPdf(rawFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
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

No código acima, você lê as credenciais e cria o contexto de execução. O SDK dos Serviços PDF requer o contexto de execução para autenticar suas solicitações.

Em seguida, execute a operação Criar PDF que converte os documentos originais para o formato PDF. Por fim, use o comando `outputPdf` parâmetro para copiar o relatório de PDF. Na amostra de código, você encontra esse código no arquivo src/helpers/pdf.js. Posteriormente neste tutorial, você importará o módulo PDF e chamará esse método.

Como demonstrado na seção anterior, seus clientes podem acessar a seguinte página para selecionar os relatórios que desejam converter em PDF:

![Captura de tela da capacidade do cliente](assets/report_3.png)

Quando um cliente seleciona um ou mais desses relatórios, você cria o arquivo de PDF.

Primeiro, vamos ver um único arquivo de PDF em ação. Quando o usuário seleciona um único relatório, você só precisa convertê-lo em PDF e fornecer o link de download.

```
try {
console.log(`[INFO] generating the report...`);
await pdf.createPdf(`./public/documents/raw/${reports}`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

Esse código cria um relatório e compartilha o URL de download com o cliente. Aqui está a página da Web de saída:

![Captura de tela da tela de download do cliente](assets/report_5.png)

E aqui está o PDF de saída:

![Captura de tela do relatório genérico](assets/report_6.png)

Os clientes podem selecionar vários arquivos para gerar um relatório combinado. Quando o cliente seleciona mais de um documento, você executa duas operações: a primeira cria um PDF parcial para cada documento e a segunda combina-as em um único relatório de PDF.

```
async function combinePdf(pdfs, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials),
operation = adobe.CombineFiles.Operation.createNew();
// Pass the PDF content as input (stream)
for (let pdf of pdfs) {
const source = adobe.FileRef.createFromLocalFile(pdf);
operation.addInput(source);
}
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Esse método está disponível no arquivo src/helpers/pdf.js e exposto como parte da exportação do módulo.

```
try {
console.log(`[INFO] creating a batch report...`);
// Create a batch report and send it back
let partials = [];
for (let index in reports) {
const name = `partial-${index}-${reports[index]}`;
await pdf.createPdf(`./public/documents/raw/${reports[index]}`, `./public/documents/processed/${name}`);
partials.push(`./public/documents/processed/${name.replace('docx', 'pdf').replace('xlsx', 'pdf')}`);
}
await pdf.combinePdf(partials, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the combined report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

Este código gera um relatório compilado para vários documentos de entrada. A única função adicionada é a `combinePdf` método que obtém uma lista de nomes de caminho de arquivo PDF e retorna um único PDF de saída.

Agora, seus clientes do painel de redes sociais podem selecionar relatórios relevantes de suas contas e baixá-los como um PDF útil. Esse painel permite que eles mostrem ao gerenciamento e a outras partes interessadas o sucesso de suas campanhas com dados, tabelas e gráficos em um formato universalmente fácil de abrir.

## Próximas etapas

Este tutorial prático explica como usar a API de serviços de PDF para ajudar os clientes a baixar relatórios relevantes como PDF fáceis de compartilhar. Você criou um aplicativo Node.js para mostrar o poder da API de serviços de PDF para os serviços de relatório e leitura de PDF. O aplicativo demonstrou como os clientes podem baixar um único documento de relatório ou combinar e mesclar vários documentos em um único relatório de PDF.

Esse aplicativo baseado em Adobe ajuda a [clientes do painel de redes sociais](https://www.adobe.io/apis/documentcloud/dcsdk/on-demand-report-creation.html) obtenha e compartilhe os relatórios de que precisa, sem se preocupar se todos os destinatários têm o Microsoft Office ou outro software instalado em seu dispositivo. Você pode usar as mesmas técnicas em seu próprio aplicativo para ajudar os usuários a exibir, combinar e baixar documentos. Ou consulte Adobe outras APIs para adicionar e rastrear assinaturas e muito mais.

Para começar, reivindique seu [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) depois crie experiências envolventes de emissão de relatórios para seus funcionários e clientes. Aproveite sua conta gratuitamente por seis meses depois [pré-pago](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) à medida que seus esforços de marketing se expandem, apenas \$0,05 por transação de documento.
