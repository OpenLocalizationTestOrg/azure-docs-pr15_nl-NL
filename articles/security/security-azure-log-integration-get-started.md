<properties
   pageTitle="Aan de slag met Azure log integratie | Microsoft Azure"
   description="Informatie over het installeren van de Azure log integratie-service en logboeken van Azure opslag, Azure controlelogboeken bijhouden en waarschuwingen van Azure Beveiligingscentrum integreren."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="get-started-with-azure-log-integration-preview"></a>Aan de slag met Azure log integratie (Preview)

Integratie van Azure log kunt u onbewerkte logboeken van uw Azure resources integreren in uw on-premises implementatie-systemen voor beveiligingsinformatie en gebeurtenis Management (SIEM). Deze integratie biedt een geïntegreerd dashboard voor alle activa, on-premises implementatie of in de cloud, zodat u kunt aggregeren, relateren analyseren en Waarschuw voor beveiligingsgebeurtenissen die zijn gekoppeld aan uw toepassingen.

Deze zelfstudie begeleidt u bij het installeren van Azure log integratie en logboeken van Azure opslag, Azure controlelogboeken bijhouden en waarschuwingen van Azure Beveiligingscentrum integreren. Geschatte tijd in beslag deze zelfstudie is één uur.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende:

- Een machine (on-premises implementatie of in de cloud) voor het installeren van de Azure log integratie-service. Deze computer moet een 64-bits Windows-besturingssysteem worden uitgevoerd met .net 4.5.1 is geïnstalleerd. Deze computer is de **Azlog Integrator**genoemd.
- Azure-abonnement. Als u niet een hebt, kunt u zich kunt aanmelden voor een [gratis-account](https://azure.microsoft.com/free/).
- Azure diagnostische hulpprogramma's voor uw Azure virtuele machines (VMs) is ingeschakeld. Zie [Azure diagnostische hulpprogramma's in Azure Cloud Services inschakelen](../cloud-services/cloud-services-dotnet-diagnostics.md)voor meer informatie over het inschakelen van diagnostische hulpprogramma's voor Cloudservices. Zie voor meer informatie over het inschakelen van diagnostische hulpprogramma's voor een met Windows Azure-VM [PowerShell gebruiken om in te schakelen Azure diagnostische gegevens in een Windows VM uitgevoerd](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Connectiviteit uit de Integrator Azlog met Azure storage en voor de verificatie en machtiging aan Azure-abonnement.
- Azure VM logboekbestanden, moet de SIEM-agent (bijvoorbeeld Splunk universele doorstuurservers, HP ArcSight Windows gebeurtenis verzamelen agent of IBM QRadar WinCollect) op de Integrator Azlog zijn geïnstalleerd.

## <a name="deployment-considerations"></a>Aandachtspunten voor de implementatie

Als het volume van de gebeurtenis hoog is, kunt u meerdere exemplaren van de Integrator Azlog uitvoeren. Taakverdeling van Azure diagnostisch hulpprogramma opslag accounts voor Windows *(af)* en het aantal abonnementen op te geven in de exemplaren moet worden gebaseerd op de capaciteit van de.

Op een computer 8-processor (core), kan slechts één exemplaar van Azlog Integrator bijna 24 miljoen gebeurtenissen per dag (~1M/hour) verwerken.

Op een computer 4-processor (core), kan slechts één exemplaar van Azlog Integrator ongeveer 1,5 miljoen gebeurtenissen per dag (~62.5K/hour) verwerken.

## <a name="install-azure-log-integration"></a>Integratie van Azure log installeren

Download [Azure Meld integratie](https://www.microsoft.com/download/details.aspx?id=53324).

De service van de integratie van Azure log verzamelt telemetriegegevens van de computer waarop deze is geïnstalleerd.  Telemetriegegevens die worden verzameld luidt als volgt:

- Uitzonderingen die tijdens de uitvoering van Azure log-integratie optreden
- Aan de doelstellingen over het aantal query's en gebeurtenissen verwerkt
- Statistieken over welke Azlog.exe opdrachtregelopties worden gebruikt

> [AZURE.NOTE] U kunt de verzameling telemetriegegevens uitschakelen door deze optie uit te schakelen.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Integreren Azure VM Logboeken uit uw diagnostisch hulpprogramma Azure opslag-accounts

1. Controleer de vereisten om ervoor te zorgen dat uw account van de opslag af logboeken verzamelen voordat u verdergaat integratie met de Azure log bovenstaande. Voer de volgende stappen niet uit als uw af opslag-account is niet logboeken verzamelen.

2. Open de opdrachtprompt en de **cd** in **c:\Program Files\Microsoft Azure Log integratie**.

3. De opdracht uitvoeren

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      StorageAccountName vervangen door de naam van de Azure opslag-account dat is geconfigureerd voor het ontvangen van diagnostische gegevens gebeurtenissen van uw VM.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Als u de abonnements-id dat wilt moet verschijnen in het logboek XML, moet u de abonnements-ID toevoegen aan de beschrijvende naam:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Wacht 30-60 minuten (het duurt het zo lang als per uur) en vervolgens de gebeurtenissen die zijn opgehaald uit de opslag-account weergeven. Als u wilt bekijken, openen **Logboeken > Windows-Logboeken > doorgestuurd gebeurtenissen** op de Integrator Azlog.

5. Zorg ervoor dat de standaard SIEM verbindingslijn op de computer zijn geïnstalleerd kiest u gebeurtenissen uit de map **Doorgestuurd gebeurtenissen** en deze op uw exemplaar SIEM pipe is geconfigureerd. Bekijk de SIEM specifieke configuratie als u wilt configureren en raadpleegt u de logboeken integreren.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Wat gebeurt er als gegevens niet wordt weergegeven in de map doorgestuurd gebeurtenissen?

Als na een uur gegevens, niet wordt weergegeven in de map **Gebeurtenissen doorgestuurd** , klikt u vervolgens:

1. Controleer de computer op en Bevestig dat deze Azure toegang hebben tot. Connectiviteit testen probeert te openen van de [Azure portal](http://portal.azure.com) vanuit de browser.

2. Zorg ervoor dat de gebruiker account **azlog** schrijftoegang heeft op de map **users\azlog**.

3. Zorg ervoor dat het opslag-account hebt toegevoegd in de opdracht **azlog bron toevoegen** wordt weergegeven wanneer u de opdracht **azlog bronlijst**uitvoert.
4. Ga naar **Logboeken > Windows-Logboeken > toepassing** om te zien als er fouten zijn gerapporteerd vanuit het logboek met Azure-integratie.

Als u zich nog steeds niet de gebeurtenissen, klikt u vervolgens ziet:

1. [Microsoft Azure opslag Explorer](http://storageexplorer.com/)downloaden.

2. Verbinding maken met het opslag-account hebt toegevoegd in de opdracht **azlog bron toevoegen**.

3. Blader in Microsoft Azure opslag Explorer naar de tabel **WADWindowsEventLogsTable** om te zien of er momenteel geen gegevens is. Als dat niet zo is, klikt u vervolgens diagnostische gegevens in de VM is onjuist geconfigureerd.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integratie van Azure controlelogboeken bijhouden en waarschuwingen van Beveiligingscentrum

1. Open de opdrachtprompt en de **cd** in **c:\Program Files\Microsoft Azure Log integratie**.

2. De opdracht uitvoeren

        azlog createazureid

      Deze opdracht wordt u gevraagd uw Azure login. De opdracht maakt vervolgens een [Azure Active Directory Service Principal](../active-directory/active-directory-application-objects.md) in de Azure AD-Tenants die als host van de Azure abonnementen waarin de aangemelde gebruiker een beheerder of de beheerder van een collega een eigenaar is. De opdracht loopt vast als de gebruiker is aangemeld alleen een gastgebruiker in de Azure AD-Tenant is. Verificatie van Azure klaar is tot en met Azure Active Directory (AD).  Maken van een service principal voor Azlog integratie wordt gemaakt van de Azure AD-identiteit die toegang tot het lezen van Azure-abonnement krijgt.

3. De opdracht uitvoeren

        azlog authorize <SubscriptionID>

      Hiermee wordt toegewezen lezerstoegang op het abonnement op de hoofdsom service gemaakt in stap 2. Als u een SubscriptionID niet opgeeft, klikt u vervolgens probeert deze toe te wijzen de rol van service principal lezer alle abonnementen waarnaar u een toegang hebt.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] U ziet mogelijk waarschuwingen als u de opdracht **Autoriseer** direct na de opdracht **createazureid** uitvoert. Er is sommige latentie tussen wanneer het Azure AD-account is gemaakt en wanneer het account is beschikbaar voor gebruik. Als u ongeveer 10 seconden wacht na het uitvoeren van de opdracht **createazureid** de opdracht **Autoriseer** uitvoeren, moet u deze waarschuwingen niet weergegeven.

4. Controleer de volgende mappen om te bevestigen dat de controle-logboekbestanden JSON er zijn:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Controleer de volgende mappen om te bevestigen dat meldingen van Beveiligingscentrum bestaat niet in deze:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Wijs de standaard SIEM bestand doorstuurservers verbindingslijn naar de gewenste map naar de gegevens in het exemplaar van SIEM pipe. Mogelijk moet u enkele veldtoewijzingen op basis van de SIEM-product dat u gebruikt.

Stuur een e-mail naar als u vragen over Azure Log integratie hebt, [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe voor het installeren van Azure log integratie en logboeken van Azure opslag integreren. Zie de volgende onderwerpen voor meer informatie:

- [Integratie van Microsoft Azure Log Azure logboekbestanden (Preview)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center voor meer informatie, systeemvereisten en installatie-instructies over de integratie van Azure log.
- [Inleiding tot Azure log integratie](security-azure-log-integration-overview.md) – dit document maakt u kennis met Azure log integratie, de belangrijkste mogelijkheden en hoe dit werkt.
- [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dit blogbericht ziet u hoe u het logboek met Azure-integratie voor gebruik met partneroplossingen Splunk, HP ArcSight en IBM QRadar configureren.
- [Azure log integratie vaak gevraagd Veelgestelde vragen](security-azure-log-integration-faq.md) - deze Veelgestelde vragen over vindt u antwoorden op vragen over het logboek met Azure-integratie.
- [Integratie Beveiligingscentrum waarschuwingen met Azure Meld integratie](../security-center/security-center-integrating-alerts-with-log-integration.md) – dit document, ziet u hoe u het synchroniseren van meldingen van Beveiligingscentrum, samen met VM beveiligingsgebeurtenissen die zijn verzameld met Azure diagnostisch hulpprogramma en Azure controlelogboeken bijhouden, met uw log analytics of SIEM oplossing.
- [Nieuwe functies voor Azure diagnostisch hulpprogramma en Azure controlelogboeken bijhouden](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – dit blogbericht maakt u kennis met Azure controlelogboeken bijhouden en andere functies waarmee u inzicht in de bewerkingen van uw Azure bronnen krijgen.
