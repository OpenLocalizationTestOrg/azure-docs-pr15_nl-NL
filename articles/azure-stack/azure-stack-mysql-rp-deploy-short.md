<properties
    pageTitle="MySQL-databases gebruiken als PaaS op Azure Stack | Microsoft Azure"
    description="Meer informatie over de snelle stappen voor het implementeren van de provider van de resource MySQL en MySQL als een service op Azure stapel opgeeft."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>MySQL-database gebruiken als PaaS op Azure-Stack

> [AZURE.NOTE] De volgende informatie is alleen van toepassing op Azure stapel TP1 implementaties.

U kunt een provider van de resource MySQL op Azure Stack implementeren. Nadat u de resource-provider implementeert, kunt u MySQL-servers en databases via Azure resourcemanager implementatie sjablonen maken en MySQL-databases als een service opgeeft. MySQL-databases, die algemene op websites zijn, ondersteuning voor veel website platforms. Nadat u de resource-provider implementeert, kunt u WordPress websites maken van de Azure Web Apps-platform als een invoegtoepassing voor de service (PaaS) voor Azure-Stack.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>Snelle stappen om te implementeren van de resource-provider
Gebruik deze stappen als u al bekend met Azure stapel bent. Als u meer informatie wilt, volgt u de koppelingen in elke sectie of Ga rechtstreeks naar [de MySQL Database Resource Provider-Adapter op Azure stapel Haalbaarheidstest Deploy](azure-stack-mysql-rp-deploy-long.md).

1.  Zorg ervoor dat u hebt [voltooid alle stappen](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) voordat u de resource-provider implementeert:

    - .NET 3.5 framework is al in de Windows Server afbeelding ingesteld. (Als u de stapel Azure-bits gedownload na februari 23, 2016, kunt u deze stap overslaan.)
    - [Een versie van Azure PowerShell die compatibel is met Azure stapel is geïnstalleerd](http://aka.ms/azStackPsh).
    - In Internet Explorer beveiligingsinstellingen op de ClientVM, [Internet Explorer Verbeterde beveiliging is uitgeschakeld en cookies zijn ingeschakeld](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies).

2. [Het bestand MySQL resource-provider binaire bestanden downloaden](http://aka.ms/masmysqlrp) en deze op de ClientVM in uw bewijs van haalbaarheidstest Azure-stapel extraheren.

3. [Bootstrap.cmd en de scripts worden uitgevoerd](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Een reeks scripts die is gegroepeerd op de twee belangrijkste tabbladen wordt geopend in het filter geïntegreerde uitvoeren van scripts omgeving (wissen). Alle geladen scripts uitvoeren in de volgorde van links naar rechts op elk tabblad.

    1. Een script uitvoeren op het tabblad **voorbereiden** van links naar rechts naar:

        - Een certificaat jokertekens als u wilt beveiligen van de communicatie tussen de provider van de resource en Azure resourcemanager maken.
        - Accepteer de servicevoorwaarden MySQL-overeenkomst en de MySQL-binaire bestanden downloaden.
        - De certificaten en alle andere onderdelen uploaden naar een Azure stapel opslag-account.
        - Galerie-pakketten publiceren zodat u MySQL-bronnen via de galerie implementeren kunt.

        > [AZURE.IMPORTANT] Als een van de scripts om geen zichtbare reden dan ook vastloopt na het versturen van de Azure Active Directory-tenant, is het mogelijk dat uw beveiligingsinstellingen voor een DLL die nodig is voor de implementatie om uit te voeren blokkeert. U lost dit probleem, zoekt u de Microsoft.AzureStack.Deployment.Telemetry.Dll in de map provider resource, met de rechtermuisknop op het, klikt u op **Eigenschappen**en klik vervolgens in het tabblad **Algemeen** inchecken **deblokkeren** .

    2. Een script uitvoeren op het tabblad **Deploy** van links naar rechts naar:

        - [Deploy een virtuele machine (VM)](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) die als host fungeert zowel uw provider van de resource, MySQL-servers en databases die u, wordt een exemplaar. Dit script verwijst naar een bestand van de parameter JSON, die u bijwerken met enkele waarden wilt voordat u het script uitvoeren.
        - [Registreren van een lokale DNS-record](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) die wordt toegewezen aan uw provider resource VM.
        - [Provider van de resource hebt geregistreerd](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) met de lokale Azure Resource Manager.

        > [AZURE.IMPORTANT] Alle scripts wordt ervan uitgegaan dat de afbeelding basis-besturingssysteem voldoet aan de vereisten (.NET 3.5 geïnstalleerd, JavaScript en cookies ingeschakeld op de ClientVM en de nieuwste versie van Azure PowerShell geïnstalleerd). Als u fouten krijgt wanneer het uitvoeren van scripts, controleert u dat u de voorwaarden hebt voldaan.

5. [Test uw nieuwe MySQL resource-provider](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment), implementeren een MySQL-database in de portal Azure stapel. Klik op **maken** &gt; **aangepaste** &gt; **MySQL-Server en Database**.

Dit moet uw provider van de resource MySQL omhoog en worden uitgevoerd in ongeveer 25 minuten krijgen.
