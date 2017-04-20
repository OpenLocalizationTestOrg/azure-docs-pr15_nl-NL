<properties 
    pageTitle="Logboekregistratie voor Machine Learning-webservices | Microsoft Azure" 
    description="Informatie over het inschakelen van logboekregistratie voor Machine Learning-webservices. Logboekregistratie biedt aanvullende informatie voor het oplossen van de API's." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Logboekregistratie inschakelen voor Machine Learning-webservices  

In dit document wordt aandacht besteed aan de mogelijkheid tot het vastleggen van klassieke webservices. Inschakelen logboekregistratie in webservices biedt aanvullende informatie, naast alleen een foutnummer en een bericht, kunt u problemen met uw oproepen naar de Machine Learning-API's.  

Inschakelen van logboekregistratie in Web Services in de klassieke Azure-portal:   

1.  Meld u aan bij [Azure klassieke portal](https://manage.windowsazure.com/)
2.  Klik in de kolom links functies op **MACHINE LEARNING**.
3.  Klik op uw werkruimte, klikt u vervolgens **WEBSERVICES**.
4.  Klik in de lijst Web services op de naam van de webservice.
5.  Klik op de naam van het eindpunt in de lijst eindpunten.
6.  Klik op **configureren**.
7.  **Diagnostisch hulpprogramma DOELCELLEN niveau** ingesteld op *fout* of *alle*en klik op **Opslaan**.

Om te schakelen logboekregistratie in de portal Azure Machine Learning-webservices.

1. Meld u aan bij de portal [Azure Machine Learning-webservices](https://services.azureml.net) .
2. Klik op klassieke webservices.
3.  Klik in de lijst Web services op de naam van de webservice.
4.  Klik op de naam van het eindpunt in de lijst eindpunten.
5.  Klik op **configureren**.
6.  Stelt u **vastleggen** in *fout* of *alle*en klik op **Opslaan**.

## <a name="the-effects-of-enabling-logging"></a>De effecten van het inschakelen van logboekregistratie

Als logboekregistratie is ingeschakeld, die alle diagnostisch hulpprogramma en fouten van het geselecteerde eindpunt worden geregistreerd in het Azure opslag-Account zijn gekoppeld aan de werkruimte van de gebruiker. Hier ziet u dit account opslag in de portal van Azure klassieke dashboardweergave (onder aan de sectie snelle bekijken) van de werkruimte.  

De logboeken kunnen worden bekeken met een van de verschillende hulpprogramma's voor het verkennen een Azure opslag-Account. De eenvoudigste mogelijk gewoon Navigeer naar de opslag-Account in de portal van Azure klassieke en klik vervolgens op **CONTAINERS**. Vervolgens ziet u een Container met de naam **ml-diagnostische gegevens**. Deze container bevat alle informatie diagnostische hulpprogramma's voor alle de eindpunten van een webservice voor alle werkruimten die is gekoppeld aan dit account opslag. 
 
## <a name="log-blob-detail-information"></a>Logboekgegevens blob

Elke blob in de container bevat de gegevens van de diagnostische hulpprogramma's voor één van de volgende opties:

-   De uitvoering van de Batch Execution-methode  
-   De uitvoering van de aanvraag en respons-methode  
-   Initialisatie van een container verzoek-antwoord
  
De naam van elke blob heeft een voorvoegsel van de volgende notatie: 

{Werkruimte Id}-{webservice Id}-{eindpunt-Id} / {Log type}  

Waar is Logboektype een van de volgende waarden:  

- batch  
- score/aanvragen  
- score/initialisatie  