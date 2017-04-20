<properties
 pageTitle="Problemen met de cloud-implementatie serviceproblemen | Microsoft Azure"
 description="Zijn er enkele veelvoorkomende problemen met die u optreden kan wanneer u een cloudservice implementatie in Azure. In dit artikel vindt enkele van deze oplossingen."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Cloud service implementatie problemen oplossen

Wanneer u een toepassing van cloud servicepakket Azure implementeert, kunt u informatie over het installeren vanuit het deelvenster **Eigenschappen** in de portal van Azure. U kunt de details in dit deelvenster gebruiken om u te helpen bij het oplossen van problemen met de cloudservice en u kunt deze gegevens bieden ter ondersteuning van Azure bij het openen van een verzoek voor ondersteuning van de nieuwe.

U kunt het deelvenster **Eigenschappen** als volgt vinden:

* Klik in de portal Azure klikt u op de implementatie van uw cloudservice, klikt u op **alle instellingen**en klik vervolgens op **Eigenschappen**.
* Klik in de klassieke Azure portal de implementatie van uw cloudservice en klik op **DASHBOARD**, vinden op de rechter benedenhoek van de pagina (onder **snel aan te duiden**). Let erop dat er geen label 'Eigenschappen' beschikbaar op dit deelvenster is.

> [AZURE.NOTE] U kunt de inhoud van het deelvenster **Eigenschappen** naar het Klembord kopiëren door te klikken op het pictogram in de rechterbovenhoek van het deelvenster.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Probleem: ik kan geen toegang tot mijn website, maar mijn implementatie wordt gestart en alle exemplaren van de rol gereed zijn

De website-URL-koppeling wordt weergegeven in de portal is niet inbegrepen voor de poort. De standaardpoort voor websites is 80. Als uw toepassing is geconfigureerd om uit te voeren in een andere poort, moet u het juiste poortnummer toevoegen aan de URL bij het openen van de website.

1. Klik in de Azure-portal op de implementatie van uw cloudservice.
2. Schakel in **het eigenschappenvenster van de Azure-portal,** de poorten voor de rol-exemplaren (onder **Invoer eindpunten**).
3. Als de poort 80 is, moet u de juiste poortwaarde toevoegen aan de URL als u de toepassing. Als u wilt een niet-standaardpoort opgeven, typt u de URL, gevolgd door een dubbele punt (:), gevolgd door het poortnummer, zonder spaties.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Probleem: Mijn exemplaren rol gerecycled zonder me alles wat te doen

Service retoucheren wordt automatisch weergegeven wanneer Azure worden gedetecteerd probleem knooppunten en kunnen daarom rol exemplaren drukt, naar de nieuwe knooppunten verplaatst. Als dit gebeurt, ziet u mogelijk de exemplaren van uw rol automatisch hergebruik. Om na te gaan als service retoucheren is opgetreden:

1. Klik in de Azure-portal op de implementatie van uw cloudservice.
2. Controleer de gegevens in het deelvenster **Eigenschappen** van de Azure-portal en bepalen of op het moment dat u de rollen hergebruik waargenomen service retoucheren opgetreden.

Rollen wordt ook ongeveer Prullenbak eenmaal per maand tijdens host-OS en Gast-OS updates.  
Zie het blogbericht [Rol exemplaar opnieuw is opgestart moeten Upgrades voor het besturingssysteem](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx) voor meer informatie

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Probleem: ik kan geen een omwisselen VIP- en wordt een foutbericht weergegeven

Een omwisselen VIP is niet toegestaan als een implementatie-update uitgevoerd wordt. Implementatie-updates automatisch kunnen worden uitgevoerd wanneer:

* Een nieuw Gast-besturingssysteem beschikbaar is en u zijn geconfigureerd voor automatische updates.
* Service retoucheren plaatsvindt.

Om na te gaan als een automatische update wordt geblokkeerd door een omwisselen VIP doen:

1. Klik in de Azure-portal op de implementatie van uw cloudservice.
2. In het deelvenster **Eigenschappen** van de Azure-portal, kijkt u naar de waarde van **Status**. Als dit **klaar**is, controleert u of de **laatste bewerking** om te zien als een onlangs is er gebeurd die mogelijk niet meer het omwisselen VIP.
3. Herhaal stap 1 en 2 voor de productie-implementatie.
4. Als u een automatische update wordt uitgevoerd, wacht u totdat deze om te voltooien voordat u probeert het omwisselen VIP doen.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Probleem: Er is een exemplaar van de rol herhalen tussen gestart, Bezig met initialiseren, bezet en gestopt

Deze voorwaarde kan duiden op een probleem met uw toepassing code, pakket of configuratie-bestand. In dat geval u zou moeten kunnen de status wijzigen om de paar minuten bekijken en de portal van Azure kan ik me iets op zoals **Recycling**, **bezet**of **Bezig met initialiseren**. Hiermee wordt aangeduid dat er iets mis met de toepassing waardoor het exemplaar van de rol wordt uitgevoerd.

Zie het blogbericht [Azure PaaS berekenen naar diagnostische gegevens](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) en [veelvoorkomende problemen die leiden rollen tot naar de Prullenbak](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)voor meer informatie over het voor dit probleem kunt oplossen.

## <a name="problem-my-application-stopped-working"></a>Probleem: Mijn toepassing is gestopt

1. Klik op het exemplaar van de rol in de portal Azure.
2. In het deelvenster **Eigenschappen** van de Azure-portal, kunt u overwegen de volgende voorwaarden uw probleem op te lossen:
   * Als het exemplaar van de functie voor het laatst is gestopt (u kunt de waarde van het **aantal afbrekingen**controleren), de implementatie kan worden bijgewerkt. Wacht om te zien als het exemplaar van de rol werkt op een eigen hervat.
   * Als het exemplaar van de rol **bezet is**, controleert u uw toepassingscode om te zien als de gebeurtenis [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) wordt verwerkt. Mogelijk moet u toevoegen of corrigeren van de code die deze gebeurtenis verwerkt.
   * Ga via de diagnostische gegevens en het oplossen van problemen scenario's in het blogbericht [Azure PaaS berekenen naar diagnostische gegevens](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

>[AZURE.WARNING] Als u uw cloudservice Prullenbak, u opnieuw instellen de eigenschappen voor de implementatie, de gegevens voor het oorspronkelijke probleem effectief wissen.

## <a name="next-steps"></a>Volgende stappen

Meer [artikelen probleemoplossing](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloudservices weergeven

Als u wilt weten hoe u problemen met de rol serviceproblemen cloud met behulp van Azure PaaS computer naar diagnostische gegevens, raadpleegt u [de reeks blogartikelen van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
