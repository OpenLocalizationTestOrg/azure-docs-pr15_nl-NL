<properties
    pageTitle="Azure RemoteApp licensing | Microsoft Azure"
    description="Lees hoe in Azure RemoteApp licensing werkt."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Hoe licensing werkt in Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Zodat u uw Azure RemoteApp-service, gemaakt van de sjablonen, hebt ingesteld en publiceren van apps aan uw gebruikers. Maar er nog steeds tot slot is moet duidelijk: licenties. Alleen hoe werkt licentieverlening voor RemoteApp en voor de apps die u via RemoteApp deelt?

RemoteApp hoeft niet alle Windows licenties of licenties voor clienttoegang voor externe bureaublad. Uw abonnement zorgt van de flank RemoteApp zelf. (Raadpleegt u de details van de [prijzen van abonnementen](https://azure.microsoft.com/pricing/details/remoteapp).)

Als u een van de afbeeldingen die deel uitmaakt van uw abonnement gebruikt, kunt u een van de apps die op deze afbeelding worden geïnstalleerd zonder een aparte licentie kunt delen. Als u de afbeelding van de sjabloon Windows Server 2012 R2 gebruiken om te maken van uw siteverzameling, kunt u bijvoorbeeld systeem Center Endpoint Protection delen met uw gebruikers. De enige uitzonderingen op deze regel zijn Office 365 ProPlus, die vereist is voor een afzonderlijk abonnement, en Office 2013, die in een verzameling productie kan niet worden gedeeld.

Als u de afbeelding van het Office 365-sjabloon die wordt geleverd met RemoteApp gebruiken wilt, moet u een *bestaande* Office 365 ProPlus-abonnement hebben. Dit geldt voor alle Office 365-Apps die u publiceren met behulp van een aangepaste sjabloon. Moet u de apps gebruiken met uw eigen abonnement te activeren. Dit geldt voor abonnementen voor zowel proefabonnement en betaald. Als u wilt de afbeelding van de Office 365-sjabloon gebruiken tijdens het proefabonnement, *en u geen al een abonnement hebt*, gaat u naar de pagina Office 365 te [registreren](https://go.microsoft.com/fwlink/p/?LinkID=403802) voor een proefabonnement. Zie [hoe RemoteApp- en Office werken samen?](remoteapp-o365.md) voor meer informatie.

Als u tijdens de proefperiode u niet wilt verkrijgen van een Office 365-proefabonnement, gebruikt u de Office 2013 Professional Plus sjabloon afbeelding die wordt geleverd met RemoteApp. Afbeelding van deze sjabloon kan alleen worden gebruikt voor 30 dagen en kan niet worden geconverteerd naar een betaald verzameling.

Voor andere apps moet u zorgen dat u de licentie voor het delen van de app hebben.

Die relevant is, rechts? Een app die u zijn officieel gemachtigd zijn om te delen, kunt u publiceren. En u nodig hebt om ervoor te zorgen dat u echt hebt recht op uw programma's delen.

Houd er rekening mee dat u een ROEP of volumelicentie overeenkomst niet in een verzameling cloud gebruiken. U *kunt* een volumelicentie overeenkomst gebruiken om te activeren toepassingen in uw siteverzameling hybride (met uitzondering van Office). Alleen moet u deze installeren op uw Sjabloonafbeelding van de media volumelicentie. Volg de gegevens van de fabrikant van de toepassing voor het installeren van licenties in een omgeving met extern bureaublad.
