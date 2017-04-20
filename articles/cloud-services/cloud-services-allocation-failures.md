<properties
    pageTitle="Fout bij het toewijzen van de Cloudservice probleemoplossing | Microsoft Azure"
    description="Fout bij het toewijzen oplossen wanneer u implementeert in de Cloud Services in Azure wordt aangegeven"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Fout bij het toewijzen oplossen wanneer u implementeert in de Cloud Services in Azure wordt aangegeven

## <a name="summary"></a>Overzicht
Wanneer u exemplaren naar een Cloudservice implementeren of nieuwe web of werknemer rol exemplaren toevoegen, wijst Microsoft Azure berekeningscluster resources. Soms foutbericht fouten bij het uitvoeren van deze bewerkingen zelfs voordat u de Azure abonnement limieten worden bereikt. In dit artikel wordt uitgelegd de oorzaken van enkele van de algemene fouten en stappen worden voorgesteld mogelijke remediation. De gegevens zijn mogelijk ook nuttig als u van plan de implementatie van uw services bent.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Achtergrond – de werking van de toewijzing
De servers in Azure datacenters worden ondergebracht in clusters. Een nieuwe cloud-toewijzing serviceaanvraag wordt geprobeerd in meerdere clusters. Wanneer het eerste exemplaar wordt geïmplementeerd in een cloudservice (in tijdelijke of productie), die service cloud wordt vastgemaakt aan een cluster. Alle verdere implementaties voor de cloudservice treedt in hetzelfde cluster. In dit artikel verwijzen we naar dit zoals 'vastgemaakt aan een cluster'. Diagram 1 hieronder ziet u het hoofdlettergebruik van een normale verdeling die wordt uitgevoerd in meerdere clusters; Diagram 2 ziet u het hoofdlettergebruik van een toewijzing die is vastgemaakt aan Cluster 2 omdat dit waar de bestaande Cloud Service CS_1 wordt gehost.

![Diagram van de toewijzing](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Waarom gebeurt van fout bij het toewijzen
Wanneer een aanvraag voor geheugentoewijzing wordt vastgemaakt aan een cluster, is er een hogere kans niet te zoeken naar gratis bronnen sinds de beschikbare resourcegroep beperkt tot een cluster is. Bovendien als uw verzoek voor de toewijzing wordt vastgemaakt aan een cluster, maar het type resource gezochte wordt niet ondersteund door dat cluster, uw aanvraag kunt invullen, mislukt zelfs als het cluster gratis resource heeft. Afbeelding 3 hieronder ziet u de hoofdletters/kleine letters waarin een vastgezette toewijzing mislukt omdat het cluster alleen candidate geen gratis resources. Diagram 4 ziet u de hoofdletters/kleine letters waarin een vastgezette toewijzing mislukt omdat het cluster alleen candidate biedt geen ondersteuning voor de gevraagde VM grootte, hoewel het cluster gratis resources heeft.

![Vastgezette toewijzing is mislukt](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Fout bij het oplossen van problemen toewijzen voor cloudservices
### <a name="error-message"></a>Foutbericht
Mogelijk ziet u het volgende foutbericht weergegeven:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Veelvoorkomende problemen
Hier vindt u de algemene toewijzing scenario's die ertoe leiden dat een aanvraag toegewezen, is vastgemaakt aan één cluster.

- Implementeert voor tijdelijke Slot - als een cloudservice een implementatie in beide slot heeft, wordt klikt u vervolgens de hele cloudservice vastgemaakt aan een specifiek cluster.  Dit betekent dat als een implementatie al in de slot productie bestaat, klikt u vervolgens een nieuwe tijdelijk opslaan implementatie kan alleen worden toegewezen in hetzelfde cluster als de productie slot. Als het cluster capaciteit nadert, wordt het verzoek kan mislukken.

- Schaalbaarheid - nieuwe exemplaren toevoegen aan een bestaande cloudservice moet in hetzelfde cluster toewijzen.  Klein schaalbaarheid aanvragen kan meestal worden toegewezen, maar niet altijd. Als het cluster capaciteit nadert, wordt het verzoek kan mislukken.

- Groep affiniteit - een nieuwe implementatie op een lege cloudservice kan worden toegewezen door de stof in een cluster in die regio, tenzij de cloudservice wordt vastgemaakt aan een groep affiniteit. Implementaties van dezelfde groep affiniteit wordt geprobeerd op hetzelfde cluster. Als het cluster capaciteit nadert, wordt het verzoek kan mislukken.

- Affiniteit groep vNet - oudere virtuele netwerken zijn gekoppeld aan affiniteit groepen in plaats van regio's en cloudservices in deze virtuele netwerken zou doen, is vastgemaakt aan de cluster affiniteit groep. Implementaties van dit type virtuele netwerk wordt geprobeerd op de vastgemaakte cluster. Als het cluster capaciteit nadert, wordt het verzoek kan mislukken.

## <a name="solutions"></a>Oplossingen

1. Implementeren naar een nieuwe cloudservice: deze oplossing is waarschijnlijk meest succesvolle als deze kan het platform kunt kiezen uit alle clusters in die regio.

    - De werklast implementeren naar een nieuwe cloudservice  

    - De CNAME- of een record om te laten verwijzen verkeer naar de nieuwe cloudservice bijwerken

    - Nadat de nul verkeer wordt ingebeld bij de oude site, kunt u de oude cloudservice verwijderen. Deze oplossing moet nul downtime in rekening gebracht.

2. Verwijderen van productie en tijdelijke sleuven: deze oplossing wordt de naam van uw bestaande DNS-behouden, maar veroorzaakt downtime in uw toepassing.

    - De productie en tijdelijke sleuven van een bestaande cloudservice zodat de cloudservice leeg is, verwijderen en kiest u vervolgens

    - Maak een nieuwe implementatie in de bestaande cloudservice. Dit probeert opnieuw te toewijzing op alle clusters in de regio. Controleer of dat de cloudservice is niet gekoppeld aan een groep affiniteit.

3. Gereserveerde IP - deze oplossing behouden blijft uw bestaande IP adres, maar veroorzaakt downtime in uw toepassing.  

    - Een optie voor uw bestaande implementatie via Powershell maken

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - Volg #2 boven, ervoor te zorgen dat de nieuwe optie opgeven in van de service CSCFG.

4. Verwijder affiniteit groep voor nieuwe implementaties - affiniteit groepen zijn niet langer aanbevolen. Volg de stappen voor het #1 hierboven om te implementeren van een nieuwe cloudservice. Zorg cloudservice is niet in een groep voor affiniteit.

5. Converteren naar een regionale virtueel netwerk - Zie [het migreren van affiniteit groepen met een regionale virtuele netwerk (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
