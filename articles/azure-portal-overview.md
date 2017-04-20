<properties
    pageTitle="Overzicht van Microsoft Azure-portal"
    description="Informatie over het gebruik van de portal van Microsoft Azure."
    services=""
    documentationCenter=""
    authors="davidwrede"
    manager="dwrede"
    editor="jimbe"/>

<tags
    ms.service="na"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="12/16/2015"
    ms.author="dwrede"/>

# <a name="microsoft-azure-portal-overview"></a>Overzicht van Microsoft Azure-portal

De Microsoft Azure-portal is een centrale plaats waar u kunt inrichten en uw Azure bronnen beheren.  Deze zelfstudie wordt u vertrouwd met de portal en leert u hoe u een aantal deze key mogelijkheden gebruiken:
- Een **volledig marketplace** waarmee u bladeren door duizenden items van Microsoft en andere leveranciers die kunnen worden gekocht en/of deze is ingericht.
- Een **geïntegreerd en scalable bladeren ervaring** waarmee u gemakkelijk te vinden van de bronnen die u interesseren en verschillende management bewerkingen uitvoeren.
- **Consistente management pagina 's** (of bladen) waarmee u het beheren van Azure breed scala van services via een consistente manier dat instellingen, acties, factureringsbeheerder informatie, servicestatus bewaken en het gebruik gegevens en veel meer.
- Een **persoonlijke ervaring** waarmee u maken van een aangepaste startscherm met daarin de gegevens die u wilt zien wanneer u zich hebt aangemeld.  U kunt ook een van de bladen management met tegels aanpassen.

 ![Azure Portal UI afdrukstand][UIOrientation]

## <a name="before-you-get-started"></a>Voordat u begint

Moet u een geldig abonnement op Azure deze zelfstudie doorlopen.  Als u niet, klikt u vervolgens [registreren voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) vandaag hebt.  Nadat u een abonnement hebt, kunt u toegang tot de portal op [https://portal.azure.com].

## <a name="how-to-create-a-resource"></a>Het maken van een resource

Azure heeft een marketplace met duizenden items die u vanuit één plaats kunt maken.  Stel dat u wilt maken van een nieuwe Windows Server 2012 VM.  De + nieuwe hub is uw ingangspunt voor een curated set aanbevolen categorieën van marketplace.  Elke categorie is een kleine set aanbevolen items samen met een koppeling naar de volledige marktplaats waarop u alle categorieën en zoeken. Als u wilt dat nieuwe Windows Server 2012 VM hebt gemaakt, kunt u de volgende acties uitvoeren:  

1.  Windows Server 2012 is uitgerust, zodat u deze uit de categorie berekeningscluster selecteren kunt.  
2.  Enkele eenvoudige invoer in een formulier invullen.
3.  Klik op 'Maak' en uw VM voor het inrichten van onmiddellijk wordt gestart.

De hub meldingen verschijnt een waarschuwing wanneer de bron is gemaakt en een blade management wordt geopend (kunt u altijd naar bladeren resources later).

![Portal-categorieën][PortalCategories]


## <a name="how-to-find-your-resources"></a>Uw resources zoeken

U kunt veelgebruikte resources altijd vastmaken aan uw startboard, maar moet u mogelijk Blader naar een ander nummer dat u niet regelmatig gebruikt.  De bladeren hub hieronder wordt weergegeven, is uw manier om alle resources.  U kunt filteren op abonnement, kiest u/formaat van kolommen en navigeer naar de bladen management door te klikken op afzonderlijke items.

![Hub bladeren][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Het beheren en Gemachtigdentoegang tot een resource

Uit deze blade die u kunt verbinding maken met de virtuele machine met extern bureaublad, bewaken voornaamste prestatiestatistieken ophalen, toegang tot deze VM Rolgebaseerd access (RBAC) gebruiken, de VM configureren en andere belangrijke beheertaken.  Delegeren van access op basis van de rol is belangrijk dat het beheer van op schaal.  Klik [hier](./active-directory/role-based-access-control-configure.md) voor meer informatie hierover. Als u wilt delegeren toegang tot een bron, kunt u de volgende acties uitvoeren:

1.  Blader naar de resource.
2.  Klik op alle instellingen in de sectie Essentials.
3.  Klik op 'Users' in de instellingenlijst.
4.  Klik op 'Toevoegen' in de opdrachtenbalk.
5.  Kies een gebruiker en een rol.

![Een Resource beheren][ManageResource]

## <a name="how-to-customize-a-resource-blade"></a>Het aanpassen van een resource blade

Azure configureert vooraf de bladen voor uw resources, maar de tegels in deze bladen zijn vergeten om te bepalen.  U kunt eenvoudig aanpassen in de modus wilt toevoegen, verwijderen, het formaat wijzigen of opnieuw rangschikken van de tegels gaan. U kunt een blade aanpassen door de volgende acties uitvoeren:

1.  Blader naar de resource.
2.  Klik op de '...' aan de bovenkant van het blad dat u wilt aanpassen.
3.  Klik op "Toevoegen delen".
4.  Slepen en neerzetten delen starten.  

![Bladen aanpassen][CustomizeBlades]

## <a name="how-to-get-help"></a>Help opvragen

Als u ooit een probleem hebt, we u hier graag voor u.  De portal heeft een hulp en ondersteuning voor pagina die u verwijzen in de juiste richting hebben kunt.  Afhankelijk van uw voor [ondersteuning van abonnement](https://azure.microsoft.com/support/plans/), kunt u ook ondersteuningstickets rechtstreeks in de portal maken.  Na het maken van een ondersteuningsticket, kunt u de levensduur van de tickets van binnen de portal beheren. U gaat naar de help en ondersteuningspagina door te schuiven om te bladeren -> Help + ondersteuning.  

![Help en ondersteuning][HelpSupport]

## <a name="summary"></a>Overzicht

Laten we bekijken wat u hebt geleerd in deze zelfstudie:
- U hebt geleerd hoe u zich aanmeldt, een abonnement nemen en blader naar de portal
- U hebt u oriënteren in de portal UI en geleerd hoe u kunt maken en bladert u resources
- U hebt geleerd hoe u een resource maakt en bladert u resources
- U hebt geleerd over de structuur of management bladen en hoe u kunt verschillende soorten resources consistente beheren
- U hebt geleerd het aanpassen van de portal om de informatie die u belangrijk vindt aan de voorzijde en centreren
- U hebt geleerd hoe u toegang tot resources op basis van de rol access (RBAC) gebruiken
- U hebt geleerd hoe u help en ondersteuning

De portal van Microsoft Azure maakt veel eenvoudiger maken en beheren van uw toepassingen in de cloud.  Bekijk de [management blog](https://azure.microsoft.com/blog/topics/management/) voor het up-to-date houden terwijl we voortdurend [luisteren naar feedback](https://feedback.azure.com/forums/223579-azure-preview-portal/) en verbeteringen te maken.  [Blog van ScottGu](http://weblogs.asp.net/scottgu) is nog een prima plek om te zoeken naar alle Azure updates.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png
