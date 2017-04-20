<properties
    pageTitle="Meld u Analytics-functies voor serviceproviders | Microsoft Azure"
    description="Log Analytics kunt beheerde serviceproviders (MSPs), grote ondernemingen, onafhankelijke softwareleveranciers (ISV's) en hostingproviders beheren en servers in van de klant on-premises implementatie of cloudinfrastructuur controleren."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Log Analytics-functies voor serviceproviders

Log Analytics kunt beheerde serviceproviders (MSPs), grote ondernemingen, onafhankelijke softwareleveranciers (ISV's) en hostingproviders beheren en servers in van de klant on-premises implementatie of cloudinfrastructuur controleren. 

Grote ondernemingen delen veel overeenkomsten met serviceproviders, vooral als er een centrale IT-afdeling die verantwoordelijk is voor het beheer van IT voor veel verschillende bedrijfseenheden. In dit document gebruikt de term- *serviceprovider* voor eenvoudig, maar dezelfde functionaliteit is ook beschikbaar voor ondernemingen en andere klanten.

## <a name="cloud-solution-provider"></a>Cloud oplossingsprovider

Voor partners en serviceproviders die deel van het programma [Cloud oplossing Provider (CSP uitmaken)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) , is Log Analytics een van de Azure services beschikbaar zijn op een abonnement CSP. 

Log Analytics, zijn de volgende mogelijkheden ingeschakeld in de *Cloud oplossingsprovider* abonnementen.

Als een *Provider van de Cloud-oplossing* kunt u het volgende doen:

+ Log Analytics werkruimten maken in een tenant (van klant)-abonnement.
+ Access-werkruimten die zijn gemaakt door tenants. 
+ Toevoegen en verwijderen van de gebruikerstoegang tot de werkruimte met Azure Gebruikersbeheer. Wanneer u in een tenant-werkruimte in de portal OMS de Gebruikersbeheer pagina onder instellingen is niet beschikbaar
  - Log Analytics biedt geen ondersteuning voor access op basis van rollen nog -geeft een gebruiker `reader` machtiging in de portal van Azure kan deze configuratiewijzigingen aanbrengen in de portal OMS

Als u wilt zich hebt aangemeld bij een tenant-abonnement, moet u de tenant-id opgeven. De tenant-id is vaak laatste deel van het e-mailadres gebruikt om aan te melden.

+ Klik in de portal OMS toevoegen `?tenant=contoso.com` in de URL voor de portal. Bijvoorbeeld:`mms.microsoft.com/?tenant=contoso.com`
+ Gebruik in PowerShell, de `-Tenant contoso.com` parameter bij gebruik van `Add-AzureRmAccount` cmdlet
+ De tenant-id wordt automatisch toegevoegd wanneer u de `OMS portal` koppeling vanuit de Azure-portal te openen en meld u aan bij de portal OMS voor de geselecteerde werkruimte

Als een *klant* van een Provider van de oplossing Cloud, kunt u het volgende doen:

+ Log analytics werkruimten maken in een CSP-abonnement
+ Access-werkruimten die zijn gemaakt door de CSP
  -  Gebruik de `OMS portal` koppeling vanuit de Azure-portal te openen en meld u aan bij de portal OMS voor de geselecteerde werkruimte
+ Bekijken en gebruiken van de beheerpagina onder instellingen in de OMS-portal

>[AZURE.NOTE] De back-up en herstellen van de Site-oplossingen voor Log Analytics kunnen geen verbinding maken met een kluis herstel Services en in een CSP-abonnement kunnen worden geconfigureerd.

## <a name="managing-multiple-customers-using-log-analytics"></a>Meerdere klanten met Log Analytics beheren 

Het wordt aanbevolen dat u een logboek Analytics-werkruimte maken voor elke klant die u beheert. Een werkruimte Log Analytics bevat:

+ Een geografische locatie voor gegevens op te slaan. 
+ Granulatie voor facturering 
+ Gegevens moeten worden geïsoleerd 
+ Unieke configuratie

U maakt een werkruimte per klant, bent u kunnen blijven de gegevens van elke klant gescheiden en het gebruik van elke klant ook bijhouden.

Meer informatie over wanneer en waarom meerdere werkruimten maken wordt beschreven in [beheren toegang uit om aan te melden analytics] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Maken en de configuratie van de klant-werkruimten kunnen worden geautomatiseerd via [PowerShell](log-analytics-powershell-workspace-configuration.md), [resourcemanager sjablonen](log-analytics-template-workspace-configuration.md), of de [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/)gebruiken.

Het gebruik van resourcemanager sjablonen voor de configuratie van de werkruimte kunt u beschikt over een basispagina configuratie die kan worden gebruikt om te maken en configureren van werkruimten. U kunt zijn er zeker van te zijn dat zoals werkruimten worden gemaakt voor klanten die ze automatisch worden geconfigureerd aan uw behoeften. Wanneer u uw vereisten voor bijwerkt, wordt de sjabloon wordt bijgewerkt en de bestaande werkruimten vervolgens opnieuw te toegepast. Dit proces zorgt ervoor dat zelfs bestaande werkruimten voldoen aan uw nieuwe standaarden.    

Bij het beheren van meerdere Log Analytics-werkruimten, wordt aangeraden elke werkruimte integreren met uw bestaande systeem voor kaartverkoop bewerkingen console aan met de functionaliteit voor [waarschuwingen](log-analytics-alerts.md) . Ondersteuningspersoneel kunt door te integreren met uw bestaande systemen, blijven hun vertrouwde processen volgen. Log Analytics regelmatig Hiermee wordt gecontroleerd elke werkruimte ten opzichte van de waarschuwing criteria die u opgeeft en genereert een waarschuwing als actie noodzakelijk is.

Voor leidinggevenden niveau rapporten die gegevens samenvatten over werkruimten kunt u de integratie tussen Log analyses en [PowerBI](log-analytics-powerbi.md). Als u integreren met een ander rapportage systeem wilt, kunt u de zoeken-API (via PowerShell of [REST](log-analytics-log-search-api.md)) query's uitvoeren en zoekresultaten exporteren.

## <a name="next-steps"></a>Volgende stappen

+ Automatiseren maken en de configuratie van werkruimten [resourcemanager sjablonen](log-analytics-template-workspace-configuration.md) gebruiken
+ Het maken van werkruimten via [PowerShell](log-analytics-powershell-workspace-configuration.md) automatiseren 
+ [Meldingen](log-analytics-alerts.md) gebruiken om te integreren met bestaande systemen
+ Maak overzichtsrapporten [PowerBI](log-analytics-powerbi.md) gebruiken
