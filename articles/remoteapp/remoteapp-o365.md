
<properties
    pageTitle="Gebruik van Office met Azure RemoteApp | Microsoft Azure" 
    description="Leer hoe Office en Azure RemoteApp samenwerken"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-office-with-azure-remoteapp"></a>Gebruik van Office met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Hebt u twee opties voor het hosten van Office-toepassingen in Azure RemoteApp: Office 365 ProPlus of Office 2013 Professional Plus proefabonnement.

**Hoi wist u dat er een nieuw, artikel die wordt binnenkort vervangen door dit beter? Bekijk [hoe u uw Office 365-abonnement met Azure RemoteApp gebruikt](remoteapp-officesubscription.md). Deze bedekt alle informatie die u nodig hebt voor het gebruik van Office 365 + Azure RemoteApp.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
U kunt een RemoteApp-siteverzameling met de Office 365 ProPlus Sjabloonafbeelding maken. Deze optie kunt u uw Office 365-service aan RemoteApp uitbreiden. U moet een bestaand abonnement hebt en uw gebruikers moeten worden licentie voor de Office 365 ProPlus service, een zelfstandig product of service via de Office 365-abonnementen.

RemoteApp ondersteunt gedeelde computeractivering van Office 365. Wanneer u gedeelde computeractivering inschakelt en de [Office-implementatieprogramma](http://www.microsoft.com/download/details.aspx?id=36778) voor installatie gebruiken, wordt Office 365 ProPlus geïnstalleerd niet worden geactiveerd. Wanneer een gebruiker positief of negatief in een verzameling met Office 365, Office gecontroleerd als de gebruiker voor Office 365 ProPlus is ingericht. Als dat zo is, Office tijdelijk activeert Office 365 ProPlus - dit activering zich blijft voordoen tot die gebruikers positief of negatief afmelden bij de service.

Als u wilt gebruiken gedeelde computeractivering van Office 365, moet u een [aangepaste sjabloon](remoteapp-create-custom-image.md) maken en te installeren, Office 365 ProPlus na [deze aanwijzingen](https://technet.microsoft.com/library/dn782858.aspx).

U kunt uw gebruikers Office 365-licenties aan de [Office 365-beheerportal](https://portal.office365.com/)beheren. Lees meer informatie over het [service-abonnementen voor Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx).  


## <a name="office-2013-professional-plus-trial"></a>Evaluatieversie van Office 2013 Professional Plus
Tijdens een proefabonnement van 30 dagen van RemoteApp, kunt u de afbeelding van de Office 2013 Professional Plus (evaluatieversie) sjabloon een RemoteApp-verzameling maken. U kunt gebruikers toewijzen aan deze proefabonnement verzameling met hun Azure Active Directory-accounts voor werk of een Microsoft-accounts. Geen extra abonnement is vereist.

Dit is een handige optie Activeer de banden en een goede gevoel voor Office in RemoteApp kunt ophalen. Deze optie is echter bestemd voor evaluatie en testen alleen. RemoteApp-siteverzamelingen die zijn gemaakt met behulp van de afbeelding van de Office 2013 Professional Plus (evaluatieversie) sjabloon kunnen niet worden overgebracht naar versie en wordt uitgeschakeld aan het einde van de proefperiode.

## <a name="switching-from-trial-to-production"></a>Overstappen van een proefabonnement op productie
Wanneer u uw gratis proefabonnement van 30 dagen begint, kunt een notitie in het gedeelte RemoteApp van de portal u nagaan hoe lang u in de evaluatieversie hebt verlaten voordat u de overgang naar een betaalde account moet. U kunt uw account te activeren en overschakelen naar productiemodus via de koppeling in deze notitie.

Wanneer u uw account hebt geactiveerd, heeft dit invloed op alle RemoteApp-collecties in uw account.

- Siteverzamelingen die worden uitgevoerd met de Windows Server 2012 R2 of de Office 365 ProPlus sjabloonafbeeldingen wordt naadloos overstappen naar productie. Alle gebruikersgegevens en de instellingen, inclusief lopende-sessies blijven behouden.
- Als u de aangepaste sjabloonafbeeldingen hebt geüpload, wordt met deze afbeeldingen verzamelingen worden ook naadloos overstappen.
- De afbeelding van de Office 2013 Professional Plus (evaluatieversie) sjabloon is bedoeld voor evaluatie alleen. Siteverzamelingen met de afbeelding van deze sjabloon kunnen niet worden overgebracht naar productie. Ze geplaatst in "uitgeschakeld" staat.


Als u niet naar versie op de vervaldatum van uw proefabonnement overstappen kan, worden uw RemoteApp-verzamelingen uitgeschakeld. Maak u geen zorgen: uw instellingen en gegevens van gebruikers worden opgeslagen voor een ander 90 dagen, zodat u kunt nog steeds uw service activeren en naar productiemodus zonder dat er gegevens verloren overschakelen.
