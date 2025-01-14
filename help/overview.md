---
title: Tutorials da API [!DNL Adobe Acrobat Services]
description: Página de visão geral de  [!DNL Adobe Acrobat Services]
feature: Acrobat Sign API, PDF Services API, PDF Embed API, Document Generation API, PDF Electronic Seal API, PDF Extract API, PDF Accessibility Auto-Tag API
role: Developer
level: Beginner, Intermediate, Experienced
jira: KT-7463
type: Tutorial
thumbnail: KT-7463.jpg
exl-id: c73feb77-4057-42fd-831c-a5004c7637c1
source-git-commit: 2e23ffef7e4438a9287c909d2fdb13968195c31f
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 2%

---

# Tutoriais de API do [!DNL Adobe Acrobat Services]

[!DNL Adobe Acrobat Services] tem seis APIs principais:

* [!DNL Adobe PDF Services API]
* [!DNL Adobe PDF Embed API]
* [!DNL Adobe Document Generation API]
* [!DNL Adobe PDF Electronic Seal API]
* [!DNL Adobe PDF Extract API]
* [!DNL Adobe PDF Accessibility Auto-Tag API]

As duas últimas APIs e seus SDKs estão incluídos no [!DNL Adobe PDF Services API] como parte de uma oferta paga. [!DNL PDF Embed API] é uma oferta gratuita. Essas APIs automatizam a geração, a manipulação e a transformação do conteúdo do documento por meio de um conjunto de serviços da Web modernos baseados em nuvem. Elas ajudam a fornecer experiências mais simples, rápidas e de marca para que você controle a interação do usuário com seus documentos, simplifique os fluxos de trabalho de PDF e promova o uso e a retenção. Estes tutoriais ajudam você a ficar atualizado, fornecendo experiências de marca mais simples e rápidas com as APIs do [!DNL Adobe Acrobat Services].

<!-- Comment -->
<!-- CARDS

* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices
  {target = _self}
  {title = PDF Services API}
  {description = PDF APIs with SDKs for node.js, .Net, and Java to create, convert, OCR PDFs, and more}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1a7b3ae4fc2b8c33c920f81a3eee05dc358108a74.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/overview-docgen
  {target = _self}
  {title = Document Generation API}
  {description = Generate PDF and Word documents from Word templates and JSON data}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1e4df708e549cf00ce0fb7fa1782957786ad4886b.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility
  {target = _self}
  {title = Adobe PDF Accessibility Auto-Tag API tutorials}
  {description = This AI-powered API automatically tags documents making it easy to scale PDF accessibiity}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract
  {target = _self}
  {title = PDF Extract API}
  {description = Unlock the structure and content elements of any PDF with a web service powered by Adobe Sensi's machine learning}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal
  {target = _self}
  {title = PDF Electronic Seal API}
  {description = Learn how to apply a tamper-evident electronic seal to PDFs at scale}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1144424efd29c8faaf22aceff3f73d80fb7fb3ac1.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed
  {target = _self}
  {title = PDF Embed API}
  {description = Free Javascript API to embed high-fidelity PDFs, enable collaboration, and see analytics}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_100c64db6899f092f8a30e0d153091398242f8abc.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign
  {target = _self}
  {title = Acrobat Sign API}
  {description = Integrate e-signatures into your platform or application}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1f579a346e7d3a7647236d7b81daf49d45dafde35.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/usecases/overview-usecases
  {target = _self}
  {title = Adobe Acrobat Services API use cases}
  {description = A wide variety of Acrobat Services API use cases}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_13219295abc6d94806a7cccf552effa9b54f25ecf.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}

-->
<!-- End Comment -->

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Services API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices" title="API de serviços PDF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1a7b3ae4fc2b8c33c920f81a3eee05dc358108a74.png?width=400&format=webply&optimize=medium" alt="API de serviços PDF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices" target="_self" rel="referrer" title="API de serviços PDF">API de Serviços PDF</a>
                    </p>
                    <p class="is-size-6">APIs PDF com SDKs para node.js, .Net e Java para criar, converter, OCR PDF e muito mais</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Procurar tutoriais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Document Generation API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/overview-docgen" title="API de geração de documento" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1e4df708e549cf00ce0fb7fa1782957786ad4886b.png?width=400&format=webply&optimize=medium" alt="API de geração de documento"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/overview-docgen" target="_self" rel="referrer" title="API de geração de documento">API de geração de documento</a>
                    </p>
                    <p class="is-size-6">Gerar documentos PDF e Word a partir de modelos do Word e dados JSON</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/overview-docgen" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Procurar tutoriais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe PDF Accessibility Auto-Tag API tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility" title="Tutoriais da API de tag automática de acessibilidade do Adobe PDF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium" alt="Tutoriais da API de tag automática de acessibilidade do Adobe PDF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility" target="_self" rel="referrer" title="Tutoriais da API de tag automática de acessibilidade do Adobe PDF">Tutoriais da API de marca automática de acessibilidade do Adobe PDF</a>
                    </p>
                    <p class="is-size-6">Essa API viabilizada por IA marca automaticamente os documentos, facilitando o dimensionamento da acessibilidade do PDF</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Procurar tutoriais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Extract API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract" title="API do PDF Extract" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium" alt="API do PDF Extract"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract" target="_self" rel="referrer" title="API do PDF Extract">API de Extração de PDF</a>
                    </p>
                    <p class="is-size-6">Desbloqueie a estrutura e os elementos de conteúdo de qualquer PDF com um serviço Web viabilizado pelo aprendizado de máquina do Adobe Sensi</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Procurar tutoriais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Electronic Seal API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal" title="API de selo eletrônico do PDF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1144424efd29c8faaf22aceff3f73d80fb7fb3ac1.png?width=400&format=webply&optimize=medium" alt="API de selo eletrônico do PDF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal" target="_self" rel="referrer" title="API de selo eletrônico do PDF">API de selo eletrônico do PDF</a>
                    </p>
                    <p class="is-size-6">Saiba como aplicar um selo eletrônico inviolável a PDF em escala</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Procurar tutoriais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Embed API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed" title="PDF Embed API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_100c64db6899f092f8a30e0d153091398242f8abc.png?width=400&format=webply&optimize=medium" alt="PDF Embed API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed" target="_self" rel="referrer" title="PDF Embed API">API de inserção de PDF</a>
                    </p>
                    <p class="is-size-6">API JavaScript gratuita para incorporar PDF de alta fidelidade, permitir colaboração e ver análises</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Procurar tutoriais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Acrobat Sign API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign" title="API do Acrobat Sign" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_1f579a346e7d3a7647236d7b81daf49d45dafde35.png?width=400&format=webply&optimize=medium" alt="API do Acrobat Sign"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign" target="_self" rel="referrer" title="API do Acrobat Sign">API DO Acrobat Sign</a>
                    </p>
                    <p class="is-size-6">Integrar assinaturas eletrônicas à sua plataforma ou aplicativo</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Procurar tutoriais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe Acrobat Services API use cases">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/usecases/overview-usecases" title="Casos de uso da API de Serviços do Adobe Acrobat" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/media_13219295abc6d94806a7cccf552effa9b54f25ecf.png?width=400&format=webply&optimize=medium" alt="Casos de uso da API de Serviços do Adobe Acrobat"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/usecases/overview-usecases" target="_self" rel="referrer" title="Casos de uso da API de Serviços do Adobe Acrobat">Casos de uso da API de Serviços do Adobe Acrobat</a>
                    </p>
                    <p class="is-size-6">Uma grande variedade de casos de uso da API de serviços da Acrobat</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/usecases/overview-usecases" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Procurar tutoriais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
