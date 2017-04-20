
<properties
    pageTitle="Azure-consistente opslag: verschillen en overwegingen | Microsoft Azure"
    description="Het verschil van Azure opslagcapaciteit en de andere Azure-consistente aandachtspunten voor de opslag-implementatie."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Azure-consistente opslag: verschillen en overwegingen

Azure-consistente opslag is de set van opslag cloudservices in Microsoft Azure stapel. Azure-consistente opslagruimte biedt blob, tabel, wachtrij en functionaliteit voor het beheer van account met Azure-consistente semantiek. In dit artikel bevat een overzicht van de bekende Azure-consistente opslag verschillen met Azure opslagmedia. Ook kunt u zien andere relevante informatie in gedachten moet houden wanneer u de momenteel openbaar preview-versie van Microsoft Azure stapel implementeert.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Bekende verschillen

Deze Technical Preview-versie van Azure-consistente opslag heeft geen 100% functie overeenkomst met Azure-opslag voor de API-versies die worden ondersteund. Bekende Functieverschillen onder andere het volgende:

-   Bepaalde accounttypen opslag nog beschikbaar zijn, bijvoorbeeld standaard\_RAGRS en standaard\_GRS.

-   Premium\_LRS opslag accounts kunnen worden ingericht. Er zijn echter momenteel geen prestatielimieten of garanties.

-   Azure bestanden functionaliteit is nog niet beschikbaar.

-   De API krijgen pagina bereiken biedt geen ondersteuning voor het ophalen van pagina's die tussen momentopnamen van BLOB van pagina's verschillen.

-   De API krijgen pagina bereiken geeft als resultaat van de pagina's die 4 KB specifieker hebben.

-   Partitiesleutel en rijsleutel in de Azure-consistente opslag tabel implementatie zijn elk beperkt tot 400 tekens (dat wil zeggen 800 bytes) grootte.

-   BLOB namen in de Azure-consistente opslag Blob service-implementatie is beperkt tot 880 tekens (dat wil zeggen 1760 bytes) grootte.

-   De Azure-consistente opslag-implementatie van gegevens voor gebruik van tenant opslag rapportage biedt opslag gebruik meters die gelijk is aan die in Azure wordt aangegeven, met dezelfde semantiek en eenheden. Op dit moment echter IaaS transacties is niet inbegrepen bij de opslag transacties gebruik meter en de gebruik gegevens overbrengen meter geen onderscheid maken het bandbreedtegebruik door interne en externe netwerkverkeer naar een gebied Azure stapel.

-   Bepaalde verschillen bestaat niet in het bereik van de functionaliteit voor opslag beheerbaarheid. Bijvoorbeeld niet kunt u het accounttype wijzigen of aangepaste domeinen hebt. Bovendien alleen API-niveau consistentie wordt aangeboden voor de Premium\_LRS opslag accounttype.

## <a name="deployment-considerations"></a>Aandachtspunten voor de implementatie

-   **Alleen testen.** Implementeer niet Azure-consistente opslagruimte in productieomgevingen die de huidige versie van de technische bètaversie van Microsoft Azure stapel gebruiken. Deze versie is bedoeld alleen het kader van evaluatie in een testomgeving.

-   **Prestaties**. De Microsoft Azure stapel Technical Preview-versie van Azure-consistente opslagruimte is niet volledig prestaties geoptimaliseerd.

-   **Schaalbaarheid.** De Microsoft Azure stapel Technical Preview-versie van Azure-consistente opslagruimte is niet geoptimaliseerd voor schaalbaarheid.

## <a name="version-support-considerations"></a>Aandachtspunten voor de versie ondersteuning

De volgende versies worden ondersteund met deze preview-versie van Azure-consistente opslag:

> Azure opslag Client-bibliotheek: [Microsoft Azure opslag 6.x .NET SDK](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Azure opslag van gegevens: [2015-04-05 REST API versie](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure opslag management-services: [2015-05-01-preview](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015-06-15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Volgende stappen

-   [Inleiding tot Azure-consistente opslag](azure-stack-storage-overview.md)
