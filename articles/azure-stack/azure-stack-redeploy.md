<properties
    pageTitle="Implementeer deze opnieuw Azure stapel | Microsoft Azure"
    description="Implementeer deze opnieuw Azure stapel."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Implementeer deze opnieuw Azure stapel

Als u wilt implementeren Azure stapel, moet u beginnen helemaal zoals hieronder beschreven.

## <a name="steps-to-redeploy-azure-stack"></a>Stappen voor het implementeren van Azure-Stack

1. Start opnieuw op de host in het oorspronkelijke besturingssysteem (geïnstalleerd naar absoluut metalen). Dit is niet de standaardinstelling in het opstartmenu zodat u KVM of lokale console gebruiken moet om deze te selecteren de opgestart (tijdens de installatie u de 'opstarten uit VHD"OS naar 'AzureStack TP2' genoemd, zo kunt bepalen welke OS is).

    U hoeft niet te verwijderen van de bestaande opstarten invoer (de nieuwe ondersteuning script "PrepareBootFromVHD.ps1" voor die voor u zorgt.)

2. Als u geen KVM of kiest u het besturingssysteem opstarten voor opgestart naartoe wilt:
    
    1. Zoek het script.\BootMenuNoKVM.ps1. Dit bestand is beschikbaar met de andere ondersteuning-scripts die samen met deze opbouwen.
    
    2. Het script uitvoeren met verhoogde bevoegdheden. Selecteer de naam van de oorspronkelijke Host OS. Hiermee wordt de host start in de oorspronkelijke host OS zonder KVM-toegang.
    
    3. Wanneer het script voltooid is wordt u gevraagd om te bevestigen het opnieuw opstarten.

    - Als er andere gebruikers aangemeld, wordt deze opdracht, mislukt.

    - Voer de volgende opdracht uit alleen: Computer opnieuw opstarten-afdwingen 
 
3. Verwijder het CloudBuilder.vhdx-bestand dat is gebruikt als onderdeel van de vorige implementatie.

    U hoeft niet te verwijderen van de bestaande opslag-toepassingen uit de vorige TP2-implementatie. De implementatiescript wordt gedetecteerd en worden de bestaande en Hiermee maakt u nieuwe.

5. Implementeer deze opnieuw kopiëren van een exemplaar van de CloudBuilder.vhdx, opstarten in deze, enzovoort.

## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met Azure stapel](azure-stack-connect-azure-stack.md)
