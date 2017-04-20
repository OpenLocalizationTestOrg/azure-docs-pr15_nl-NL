<properties 
    pageTitle="Wat is Azure RemoteApp? | Microsoft Azure" 
    description="Informatie over het delen van apps en resources op elk apparaat via Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Wat is Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp kunt u de functionaliteit van de on-premises implementatie Microsoft RemoteApp-programma ondersteund door Remote Desktop Services, naar Azure. Azure RemoteApp helpt u veilige, externe toegang bieden tot toepassingen vanaf veel verschillende apparaten. Azure RemoteApp host met niet-permanente Terminal Server-sessies in de cloud en u toegang hebt tot deze gebruiken en te delen met uw gebruikers.

U kunt met Azure RemoteApp-apps en resources delen met gebruikers op vrijwel elk apparaat. We hosten uw apps in de cloud, wat betekent dat we handelingen kunt verrichten van de hardware en schaal om te voldoen aan vereisten van gebruikers. Enige u hoeft te doen is uploaden van de apps die u wilt delen en klik vervolgens zorgen dat uw gebruikers u deze apps kunt gebruiken. [Gebruikers toegang tot hun eigen apparaten blijven](remoteapp-clients.md), terwijl u alles tot en met de Azure-portal beheren. Zelfs er de optie van het gebruik van uw bedrijfsreferenties, zodat u de beveiliging van apps en gegevens zorgen.

Lees verder voor meer informatie over Azure RemoteApp, of, als we u, [Probeer dit nu](https://azure.microsoft.com/services/remoteapp/)al hebt overtuigd.

Hebt u vragen over Azure RemoteApp? Bekijk onze [Veelgestelde vragen](remoteapp-faq.md).

Azure RemoteApp maakt deel uit van de [Microsoft Virtual Desktop Infrastructure](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Nieuw!** Wilt u meer informatie over Azure RemoteApp? Of bent u er klaar voor het valideren van Azure RemoteApp bij het op schaal? Deelnemen aan onze wekelijks [overleggen met de experts webinar](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp-verzamelingen
Er zijn twee soorten [Azure RemoteApp siteverzamelingen](remoteapp-collections.md):


- Een **cloud-siteverzameling** wordt gehost en gegevens voor programma's worden opgeslagen in de cloud. Gebruikers hebben toegang tot apps door aanmelden met hun Microsoft-account of zakelijke referenties gesynchroniseerd of federatieve met Azure Active Directory.

    Kies een verzameling cloud wanneer de toepassing die u wilt delen niet een verbinding met een resource hoeft privé netwerk van uw bedrijf (bijvoorbeeld via een VPN-apparaat). Als de toepassing resources op Internet, OneDrive of Azure gebruikt, wordt een verzameling cloud werken voor u. Het is ook de snelste manier om te maken.

- Een **hybride siteverzameling** wordt gehost en worden gegevens opgeslagen in de cloud Azure, maar ook kunt gebruikers access-gegevens en resources die zijn opgeslagen op uw lokale netwerk. Gebruikers hebben toegang tot apps door aanmelden met hun bedrijfsreferenties gesynchroniseerd of federatieve met Azure Active Directory.

    Kies een verzameling hybride als u een verbinding om bronnen op particuliere netwerk van uw bedrijf vereisen. Als de toepassing moet toegang tot een van de volgende handelingen uit:

    - Bestandsservers bevinden op uw intranet
    - Quicken
    - Databases achter een firewall

    Dit is gewoonlijk gebruiksvriendelijker voor grote bedrijven met een groot aantal bronnen op hun particuliere netwerken die niet worden verplaatst naar de cloud.

De verschillende siteverzamelingen hebt verschillende opties, waaronder netwerken, zodat u uitzoeken [welke siteverzameling](remoteapp-collections.md) meest geschikt is voor u. 


### <a name="updating-your-collection"></a>Bijwerken van uw siteverzameling
Een van de belangrijkste verschillen tussen de hybride en cloud-verzamelingen is hoe software-updates worden afgehandeld. Met een cloud-siteverzameling waarin de vooraf geïnstalleerde Office 365 ProPlus of Office 2013-afbeelding, er geen zorgen over eventuele updates te maken. De service zelf onderhoudt en uitgevouwen updates doorlopend, bij zowel apps als het besturingssysteem.

U bent verantwoordelijk voor het onderhouden van een afbeelding van de en apps voor hybride verzamelingen, evenals cloud-siteverzamelingen die de afbeelding van een aangepaste sjabloon gebruiken. Voor een domein behoren afbeeldingen, kunt u updates beheren met hulpprogramma's zoals Windows Update, Groepsbeleid of System Center.

Nadat u de afbeelding van uw aangepaste sjabloon bijwerkt, moet u de nieuwe afbeelding uploaden naar de cloud Azure en past u de verzameling als de nieuwe afbeelding wilt gebruiken. (U kunt dit doen vanuit de pagina Azure RemoteApp **Snel starten** of het Dashboard.)

Zie [de verzameling bijwerken](remoteapp-update.md) voor meer informatie.

## <a name="supported-remoteapp-clients"></a>Ondersteunde RemoteApp-clients
Azure RemoteApp wordt ondersteund op de RemoteApp-client-apps voor Windows en Windows RT, evenals de Microsoft Remote Desktop-apps voor Mac, iOS en Android. Uw gebruikers kunnen deze apps gebruiken op hun mobiele of berekenen van apparaten voor toegang tot de nieuwe Azure RemoteApp-programma's.

Zie [toegang krijgen tot uw apps in Azure RemoteApp](remoteapp-clients.md) voor meer informatie over de clients.

## <a name="next-steps"></a>Volgende stappen
Ga! Probeer het zelf! Deze artikelen helpen u aan de slag met Azure RemoteApp:

- [Welk soort siteverzameling hebt u nodig voor Azure RemoteApp?](remoteapp-collections.md)
- [Een Azure RemoteApp-afbeelding maken](remoteapp-imageoptions.md)
- [Het maken van een wolk verzameling Azure RemoteApp](remoteapp-create-cloud-deployment.md)
- [Het maken van een hybride verzameling Azure RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Hoe licensing werkt in Azure RemoteApp?](remoteapp-licensing.md)
- [Aanbevolen procedures voor het gebruik van Azure RemoteApp](remoteapp-bestpractices.md)
- [Azure RemoteApp-Veelgestelde vragen](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Help ons te helpen u 
Wist u naast de beoordeling van dit artikel en het toevoegen van opmerkingen omlaag onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreken? Iets mis? Ik iets dat is alleen verwarrend schrijven? Omhoog schuiven en klikt u op **bewerken op GitHub** of **bewerken** als u wijzigingen wilt - die wordt verzonden naar ons nakijken en vervolgens zodra we zich op deze afmelden, ziet u uw wijzigingen en verbeteringen hier.