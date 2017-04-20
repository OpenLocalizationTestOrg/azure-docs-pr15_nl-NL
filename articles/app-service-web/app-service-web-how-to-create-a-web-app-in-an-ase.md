<properties
    pageTitle="Een WebApp in een omgeving van de Service-App maken"
    description="Informatie over het maken van WebApps en app-service-abonnementen in een App-Service-omgeving"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Een WebApp in een omgeving van de Service-App maken

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt getoond hoe u WebApps en App-Service-abonnementen maakt in een [App-omgeving](app-service-app-service-environment-intro.md) (ASE). 

> [AZURE.NOTE] Als u weten hoe wilt u een web-app maakt maar hoeft te doen in een omgeving met App-Service, raadpleegt u [een .NET-web-app maken](web-sites-dotnet-get-started.md) of een van de gerelateerde zelfstudies voor andere talen en kaders.

## <a name="prerequisites"></a>Vereisten voor

Deze zelfstudie wordt ervan uitgegaan dat u een App-Service-omgeving hebt gemaakt. Als u dat nog niet hebt gedaan, Zie [maken een App-Service-omgeving](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Een WebApp maken

1. Klik in de [Portal van Azure](https://portal.azure.com/)op **Nieuw > Web + Mobile > Web App**. 

    ![][1]

2. Selecteer uw abonnement.  

    Als er meerdere abonnementen Houd er rekening mee dat als u wilt een app in uw omgeving van de Service-App maken, u gebruiken hetzelfde abonnement die u hebt gebruikt moet bij het maken van de omgeving. 

3. Selecteer of maak een resourcegroep.

    *Resource-groepen* kunt u verwante Azure bronnen beheren als een eenheid en zijn handig bij het maken van regels voor *Rolgebaseerd toegangsbeheer* (RBAC) voor uw apps. Zie voor meer informatie [om uw Azure resources te beheren][ResourceGroups]. 

4. Selecteer of maak een App-serviceplan.

    *App-Service-abonnementen* worden beheerd sets van WebApps.  Normaal gesproken wanneer u de prijzen selecteert, is de prijs toegepast op met de App-operator in plaats van de afzonderlijke apps. In een ASE die u voor de exemplaren berekeningscluster is toegewezen aan de ASE betalen in plaats van wat u hebt opgenomen met de ASP.  Als u wilt schalen van het aantal exemplaren van een web-app dat u de schaal van de exemplaren van uw App-Service plannen en deze heeft invloed op alle van de WebApps in dat plan.  Bij sommige functies zoals site sleuven of VNET integratie zijn ook hoeveelheidsbeperkingen binnen het abonnement.  Zie [overzicht van de Azure App-Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) voor meer informatie

    U kunt de App Service-abonnementen in uw ASE bepalen door te kijken de locatie waar staat vermeld onder de naam van het plan.  

    ![][5]

    Als u gebruiken van een App serviceplan die al in uw omgeving van de Service-App wilt, selecteert u dat plan. Als u maken van een nieuwe App-serviceplan wilt, raadpleegt u het volgende gedeelte van deze zelfstudie [maken een App Service-abonnement in een App-Service-omgeving](#createplan).

5. Voer een naam voor uw web-app en klik vervolgens op **maken**. 

    Als uw ASE gebruikmaakt van een externe VIP de URL van een app in een ASE is: [*sitenaam*]. [*naam van uw App Service-omgeving*]. p.azurewebsites.net in plaats van [*sitenaam*]. azurewebsites.net
    
    Als uw ASE gebruikmaakt van een interne VIP en vervolgens de URL van een app in dat ASE is: [*sitenaam*]. [*subdomein opgegeven tijdens het maken van de ASE*]   
    Nadat u uw ASP tijdens het maken van ASE selecteren ziet u het subdomein onder **naam** bijwerken

## <a name="createplan"></a>Een App Service maken

Wanneer u een abonnement dat is App in een omgeving van de Service-App maakt, zijn uw keuzen werknemer verschillende als er geen gedeelde werknemers, in een ASE worden.  De werknemers die u moet gebruiken, zijn de velden die zijn toegewezen aan de ASE door de beheerder.  Dit betekent dat als u wilt maken een nieuw abonnement, moet u beschikken over meer werknemers die zijn toegewezen aan uw groep ASE werknemer dan het totale aantal exemplaren op al uw plannen al beschikbaar in die groep werknemer.  Als u niet voldoende werknemers in uw ASE werknemer-groep maken van uw abonnement hebt, moet u werken met uw beheerder ASE naar deze toegevoegd.

Een ander verschil met de App-Service-abonnementen die worden gehost door een App-Service-omgeving is het ontbreken van prijzen van selectie.  Wanneer u een App-Service-omgeving hebt u betaalt voor berekeningscluster-resources die worden gebruikt door het systeem en geen extra kosten voor de abonnementen in die omgeving.  Normaal gesproken wanneer u een abonnement dat is App maakt u een prijzen abonnement waarmee wordt bepaald uw facturen.  Een App-Service-omgeving is in principe een persoonlijke locatie waar u inhoud kunt maken.  U betaalt voor de omgeving en niet voor het hosten van uw inhoud.

De volgende instructies weergeven voor het maken van een App-serviceplan terwijl u een web-app, zoals beschreven in de vorige sectie van de zelfstudie maakt.

1. Klik op **Nieuw** in de selectie abonnement UI en geef een naam voor uw abonnement net zoals u gewend buiten een ASE bent.

2. Selecteer de ASE die u wilt gebruiken in de kiezer voor uw locatie.

    Omdat de omgeving van een App-Service in principe de locatie van een privé-implementatie is, ziet u onder locatie. 

    ![][2]

    Na de selectie van een ASE in de kiezer voor locatie, het maken van de App-Service-abonnement UI bijgewerkt.  De locatie ziet nu de naam van het systeem ASE en de regio is in het en de prijzen abonnement Kiezer wordt vervangen door een medewerker van toepassingen kiezer.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Een resourcegroep die werknemer selecteren

Normaal gesproken in Azure App-Service en buiten een App-Service-omgeving, zijn er 3 berekeningscluster puntgrootten die beschikbaar in de selectie van een speciale prijs-abonnement zijn.  Op soortgelijke wijze, voor een ASE kunt u maximaal 3 groepen werknemers definiëren en de grootte van de berekeningscluster die wordt gebruikt voor die groep werknemer aan te geven.  Wat dit betekent voor tenants van de ASE is dat in plaats daarvan aan het selecteren van een prijzen abonnement met berekeningscluster grootte voor uw App serviceplan, u selecteren wat een *werknemer van toepassingen*wordt genoemd.  

De selectie van de groep werknemer UI geeft berekeningscluster grootte die wordt gebruikt voor die groep werknemer onder de naam.  De beschikbare hoeveelheid verwijst naar het hoeveel exemplaren zijn beschikbaar voor gebruik in die groep berekenen.  De totale toepassingen meer exemplaren dan dit nummer instantie mogelijk, maar deze waarde verwijst naar een gewoon hoeveel niet gebruikt worden.  Als u nodig hebt om aan te passen raadpleegt u uw App-Service-omgeving om toe te voegen meer resources berekenen [configureren van uw App Service-omgeving](app-service-web-configure-an-app-service-environment.md).

![][4]

In dit voorbeeld ziet u slechts twee werknemer van toepassingen beschikbaar. Dat komt doordat de beheerder ASE toegewezen alleen hosts in deze twee werknemer pools.  De derde zou weergegeven wanneer er VMs toegewezen erin zijn.  

## <a name="after-web-app-creation"></a>Nadat de web-app is gemaakt

Er zijn een paar aandachtspunten voor het uitvoeren van WebApps en het beheren van App-Service-abonnementen in een ASE die moeten worden meegerekend.  

Zoals hierboven is, de eigenaar van de ASE is verantwoordelijk voor de grootte van het systeem en daardoor deze zijn ook verantwoordelijk voor om te garanderen dat er voldoende capaciteit voor het hosten van de gewenste App Service-abonnementen. Als er geen beschikbare werknemers, is het niet mogelijk om te maken van uw App serviceplan.  Dit is ook True schaalbaarheid van uw web-app.  Als u meer exemplaren nodig hebt u zou krijgen van de beheerder van de App-omgeving om meer werknemers toevoegen.

Na het maken van uw WebApp en de App-abonnement is een goed idee deze om uit te breiden.  In een ASE moet altijd u beschikken over minstens 2 exemplaren van uw App serviceplan voor fouttolerantie voor uw apps.  Schaalbaarheid van een abonnement dat is App in een ASE is hetzelfde als normale tot en met de App-serviceplan UI.  Voor meer informatie over schaling, [hoe u de schaal van een web-app in een App-Service-omgeving](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
