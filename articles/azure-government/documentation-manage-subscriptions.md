<properties
    pageTitle="Azure overheid Services | Microsoft Azure"
    description="Informatie over het beheren van uw abonnement in Azure overheid"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Beheren en verbinding maakt met uw abonnement in Azure overheid

Azure overheid heeft unieke URL's en eindpunten voor het beheren van uw omgeving. Het is belangrijk naar de juiste verbindingen gebruiken voor het beheren van uw omgeving naar de beheerportal of PowerShell. Nadat u bent verbonden met de overheid Azure-omgeving, werkt de normale bewerkingen voor het beheren van een service als het onderdeel is geïmplementeerd.

## <a name="connecting-via-the-portal"></a>Verbinding maken via de portal
De portal is de eenvoudigste manier die de meeste mensen verbinding met Azure overheid maken.  Als u wilt verbinden, blader naar de portal op [https://portal.azure.us](https://portal.azure.us).  De oudere versie van de Azure portal kan worden geraadpleegd via [https://manage.windowsazure.us](https://manage.windowsazure.us).

Abonnementen kunnen worden gemaakt voor uw account door te verbinden met [https://account.windowsazure.us](https://account.windowsazure.us).

## <a name="connecting-via-powershell"></a>Verbinding maken via PowerShell

Opgeven of u Azure PowerShell gebruiken voor het beheren van grote abonnement via script of access-functies die niet zijn momenteel beschikbaar in de Azure portal die u moet verbinding maken met Azure overheid in plaats van Azure openbare.  Als u PowerShell in openbare Azure hebt gebruikt, is het meestal hetzelfde.  De verschillen in Azure overheid zijn:

+ Verbinding maken van uw account
+ Regionamen

>[AZURE.NOTE] Als u nog niet PowerShell gebruikt hebt, raadpleegt u de [Inleiding tot Azure PowerShell](../powershell-install-configure.md).

Wanneer u PowerShell begint, moet u vertellen Azure PowerShell verbinding maken met Azure overheid door een parameter omgeving te geven.  De parameter zorgt ervoor dat PowerShell verbinding met de juiste eindpunten maakt.  De verzameling eindpunten wordt bepaald wanneer u verbinding aanmelden bij uw account maakt.  Verschillende API's vereisen verschillende versies van de schakeloptie omgeving:

Verbindingstype | Opdracht
---|----
Opdrachten voor het [Beheer van de service](https://msdn.microsoft.com/library/dn708504.aspx) | `Add-AzureAccount -Environment AzureUSGovernment`
Opdrachten voor het [Beheer van de resource](https://msdn.microsoft.com/library/mt125356.aspx) | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
[Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) -opdrachten | `Connect-MsolService -AzureEnvironment UsGovernment`
[Azure Active Directory-opdracht v2](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

U kunt ook de schakeloptie omgeving gebruiken wanneer de verbinding met een opslag-account nieuw AzureStorageContext met en AzureUSGovernment opgeven.

### <a name="determining-region"></a>Regio vaststellen

Nadat u verbonden bent, moet u er één extra verschil – de regio's die worden gebruikt om een service is.  Elke Azure cloud heeft verschillende regio's.  U kunt zien ze vermeld op de pagina van de beschikbaarheid van de service.  U normaal gesproken de regio in de parameter locatie gebruiken voor een opdracht.

Er is een variabel.  De Azure overheid regio's moeten anders dan hun algemene namen worden opgemaakt:

De naam van de algemene | Opdracht
---|----
Amerikaans beurs Virginia | USGov Virginia
Amerikaans beurs Iowa | USGov Iowa

>[AZURE.NOTE] Er is geen ruimte tussen de Verenigde Staten en beurs bij gebruik van de Parameter locatie.

Als u ooit voor het valideren van de beschikbare regio's in Azure overheid wilt, kunt u de volgende opdrachten uitvoeren en afdrukken van de huidige lijst:

    Get-AzureLocation

Als u meer wilt weten over de beschikbare omgevingen via Azure, die u kunt uitvoeren:

    Get-AzureEnvironment

## <a name="next-steps"></a>Volgende stappen

Als u voor meer informatie zoeken wilt, kunt u raadplegen:

+ [PowerShell-documenten op GitHub](https://github.com/Azure/azure-powershell)
+ [Stapsgewijze instructies voor het verbinden met bronbeheer](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure PowerShell-documenten op MSDN](https://msdn.microsoft.com/library/mt619274.aspx)

Voor aanvullende informatie en updates Abonneer u op de [Microsoft Azure overheid Blog] (https://blogs.msdn.microsoft.com/azuregov/)
