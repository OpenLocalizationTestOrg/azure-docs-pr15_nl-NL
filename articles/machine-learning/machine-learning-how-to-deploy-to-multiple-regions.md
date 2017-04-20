<properties
    pageTitle="Hoe u een webservice implementeren naar meerdere gebieden | Microsoft Azure"
    description="Stappen om te implementeren (kopie) een nieuwe webservice naar andere regio's."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Het implementeren van een webservice aan meerdere landen

De nieuwe Azure-webservices kunt u eenvoudig een webservice implementeren naar meerdere gebieden zonder meerdere abonnementen of werkruimten. 

Prijzen is regio specifieke, dat dus moet u een factureringsbeheerder plan voor elke regio waarin u de webservice gaat implementeren definiëren.

## <a name="to-create-a-plan-in-another-region"></a>Maken van een abonnement in een andere regio

1. Meld u aan bij [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/).
2. Klik op de menuoptie **abonnementen** .
3. Klik op de abonnementen via de weergavepagina, klikt u op **Nieuw**.
4. Selecteer in de vervolgkeuzelijst **abonnement** het abonnement waarin het nieuwe abonnement zich bevinden.
5. Selecteer in de vervolgkeuzelijst **regio** , een gebied voor het nieuwe abonnement. De opties voor het geselecteerde gebied wordt weergegeven in de sectie **Opties voor het plannen** van de pagina.
6. Selecteer in de vervolgkeuzelijst **Resourcegroep** een resourcegroep voor het abonnement. Meer informatie over resourcegroepen, Zie [Azure beheren bronnen via de portal](../azure-portal/resource-group-portal.md).
7. Typ de naam van het abonnement in **De naam van abonnement** .
8. Klik onder **Opties voor abonnement**op het niveau van de facturering voor het nieuwe abonnement.
9. Klik op **maken**.


## <a name="deploying-the-web-service-to-another-region"></a>De webservice implementeren naar een andere regio

1. Klik op de menuoptie **Webservices** .
2. Selecteer de webservice die u naar een nieuwe gebied implementeren wilt.
3. Klik op **kopiëren**.
4. Typ een nieuwe naam voor de webservice in het vak **Naam van webservice**.
5. Typ een beschrijving voor de webservice in het vak **Beschrijving van de Web-service**.
6. Selecteer in de vervolgkeuzelijst **abonnement** het abonnement waarin de nieuwe webservice zich bevinden.
7. Selecteer in de vervolgkeuzelijst **Resourcegroep** een resourcegroep voor de webservice. Meer informatie over resourcegroepen, Zie [Azure beheren bronnen via de portal](../azure-portal/resource-group-portal.md).
8. Selecteer het gebied waarin de webservice implementeren in de vervolgkeuzelijst **regio** .
9. Selecteer een account opslag waarin de webservice opgeslagen in de vervolgkeuzelijst **opslag-account** .
10. Selecteer een abonnement in de regio die u hebt geselecteerd in stap 8 in de vervolgkeuzelijst **Prijs-abonnement** .
11. Klik op **kopiëren**.

