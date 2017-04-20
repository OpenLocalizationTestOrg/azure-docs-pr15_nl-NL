<properties
    pageTitle="Verbinding maken met Azure stapel met CLI | Microsoft Azure"
    description="Informatie over het gebruik van de opdrachtregel platforms (CLI) om te beheren en implementeren van resources op Azure-Stack"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Installeren en configureren van Azure stapel CLI

In dit document helpen we u bij het proces van het gebruik van Azure-Interface met opdrachtregel (CLI) voor het beheren van Azure stapel resources op Linux- en Mac-client-platforms.  

## <a name="install-azure-stack-cli"></a>Azure stapel CLI installeren

Als u zich op de Mac of Linux, kunt u de CLI verkrijgen met behulp van de volgende opdracht uit:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Verbinding maken met Azure stapel
In de volgende stappen uit configureert u Azure CLI verbinding maken met Azure stapel. Klik u zich aanmeldt en informatie over abonnement ophalen.

1.  De waarde voor actief-directory-resource-id ophalen door het uitvoeren van deze PowerShell:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  Gebruik van de volgende opdracht uit CLI om toe te voegen van de stapel Azure-omgeving, ervoor te zorgen dat bijwerken *--actief-directory-resource-id* met de gegevens die de URL die zijn opgehaald in de vorige stap:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Aanmelden met behulp van de volgende opdracht uit (vervangen *gebruikersnaam* met uw gebruikersnaam in te voeren):

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Als u validatieproblemen certificaat krijgt, certificaatverificatie uitschakelen door de opdracht uit te voeren `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Stel de van Azure-configuratiemodus naar Azure resourcemanager met behulp van de volgende opdracht uit:

        azure config mode arm

5.  Een lijst met abonnementen ophalen.

        azure account list     

## <a name="next-steps"></a>Volgende stappen

[Sjablonen met Azure CLI implementeren](azure-stack-deploy-template-command-line.md)

[Verbinding maken met PowerShell](azure-stack-connect-powershell.md)

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
