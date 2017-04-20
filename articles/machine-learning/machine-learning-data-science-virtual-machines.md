<properties
    pageTitle="Gegevens wetenschappelijke virtuele machines in Azure | Microsoft Azure"
    description="Van een gegevens wetenschappelijke virtuele Machine instellen"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Gegevens wetenschappelijke virtuele machines in Azure wordt aangegeven

Instructies beschikbaar hier die wordt beschreven hoe u een VM Azure en een VM Azure met SQL-Service als IPython notitieblok servers wilt instellen. Het virtuele Windows-computer is geconfigureerd met ondersteunende hulpprogramma's zoals IPython notitieblok, Azure opslag Explorer en AzCopy, evenals andere hulpprogramma's die handig om gegevens wetenschappelijke projecten zijn. Azure opslag Explorer en AzCopy, bijvoorbeeld handige manieren om gegevens te uploaden naar Azure opslag van uw lokale computer of om deze te downloaden naar uw lokale computer uit opslag te bieden. 

In dit menukoppelingen naar onderwerpen over het instellen van de verschillende gegevens wetenschappelijke omgevingen die worden gebruikt door het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Diverse soorten Azure virtuele machines kunnen worden deze is ingericht en geconfigureerd voor gebruik als onderdeel van een cloudgebaseerde gegevens wetenschappelijke-omgeving. De beschikking over welke versie van virtuele machine gebruiken is afhankelijk van het type en de hoeveelheid gegevens kunnen worden vormgegeven met machine learning en de bestemming voor de gegevens in de cloud. 

* Voor hulp bij de vragen u rekening moet houden wanneer deze beslissing, raadpleegt u [Van plan bent uw Azure Machine Learning gegevens wetenschappelijke omgeving](machine-learning-data-science-plan-your-environment.md). 
* Zie voor een catalogus met enkele van de scenario's die optreden kunnen bij het uitvoeren van geavanceerde analyses, [scenario's voor het proces van geavanceerde analyses en technologie in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md)

Er zijn twee soorten instructies beschikbaar:

* [Instellen van een Azure virtuele machines als een server IPython notitieblok voor geavanceerde analyses](machine-learning-data-science-setup-virtual-machine.md) , ziet hoe u een Azure virtuele machines inrichten met IPython notitieblok en andere hulpmiddelen voor gegevens wetenschappelijke voor situaties waarin een formulier Azure opslag dan SQL kan worden gebruikt voor de opslag van de gegevens te doen.

* [Instellen van een Azure SQL Server-VM als een server IPython notitieblok voor geavanceerde analyses](machine-learning-data-science-setup-sql-server-virtual-machine.md) , ziet hoe u een Azure SQL Server-VM inrichten met IPython notitieblok en andere hulpmiddelen voor gegevens wetenschappelijke voor situaties waarin een SQL-database kan worden gebruikt voor de opslag van de gegevens te doen.

Zodra de ingerichte en geconfigureerd, zijn deze virtuele machines gereed voor gebruik als IPython notitieblok servers voor het verkennen en de verwerking van gegevens en voor andere taken die nodig zijn in combinatie met Azure Machine Learning en het Team gegevens wetenschap proces (TDSP). De volgende stappen in het proces van de wetenschappelijke gegevens zijn toegewezen in de [TDSP leerpad](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) en eventueel stappen die gegevens verplaatsen naar SQL Server of HDInsight, verwerken en probeer het voorbereiding op leren van de gegevens met Azure Machine Learning.


> [AZURE.NOTE] Azure virtuele Machines prijs als **betalen alleen voor wat u gebruikt**. Om ervoor te zorgen dat u bent niet die worden gefactureerd wanneer u uw VM niet gebruikt, hebt u er moet in de stand **gestopt (Deallocated)** in de [Klassieke Azure-Portal](http://manage.windowsazure.com/). Zie voor stapsgewijze instructies of hoe u de toewijzing VM [afsluiten en toewijzing VM wanneer u niet in gebruik](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 
