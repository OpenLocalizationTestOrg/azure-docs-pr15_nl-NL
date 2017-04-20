<properties 
   pageTitle="Azure-toepassing Wizard publiceren | Microsoft Azure"
   description="Azure-toepassing Wizard publiceren"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-azure-application-wizard"></a>Azure-toepassing Wizard publiceren

## <a name="overview"></a>Overzicht

Nadat u een webtoepassing in Visual Studio ontwikkelt, kunt u eenvoudiger naar een Azure cloudservice van die toepassing publiceren met behulp van de wizard **Publiceren Azure-toepassing** . De eerste sectie worden de stappen beschreven die u uitvoeren moet voordat u de wizard gebruiken en de overige secties wordt uitgelegd dat de functies van de wizard.

>[AZURE.NOTE] In dit onderwerp is over het implementeren van met cloudservices, niet op websites. Zie voor informatie over het implementeren naar websites, [het implementeren van een Azure-website](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="prerequisites"></a>Vereisten voor

Voordat u uw webtoepassing Azure publiceert kunt, moet u beschikken over een Microsoft-account en een Azure-abonnement en u moet uw webtoepassing koppelen aan een Azure-cloudservice. Als u deze taken al hebt uitgevoerd, kunt u naar de volgende sectie overslaan.

1. Krijg een Microsoft-account en een Azure-abonnement. U kunt een gratis één maand gratis Azure-abonnement [hier](https://azure.microsoft.com/pricing/free-trial/)

1. Maak een cloudservice en een account opslagruimte op Azure. U kunt dit doen vanuit Server Explorer in Visual Studio of met behulp van de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Uw webtoepassing voor Azure inschakelen. Als u wilt uw webtoepassing vanuit Visual Studio worden gepubliceerd op Azure inschakelt, moet u koppelen aan een project van de service Azure cloud in Visual Studio. De bijbehorende cloud service als project wilt maken, opent u het snelmenu voor het project voor uw webtoepassing en vervolgens converteren, **converteren naar Azure Cloud Service Project**te kiezen.

1. Nadat het project cloud-service is toegevoegd aan uw oplossing, open het dezelfde snelmenu opnieuw en kies **publiceren**. Zie voor meer informatie over het inschakelen van toepassingen voor Azure [hoe: migreren en publiceren van een webtoepassing naar een Cloudservice Azure van Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx).

>[AZURE.NOTE] Zorg ervoor dat u Visual Studio starten met beheerdersreferenties (uitvoeren als beheerder).

1. Wanneer u uw toepassing publiceren bent, opent u het snelmenu voor het project Azure cloud-service en kies vervolgens **publiceren**. De volgende stappen uit weergeven de wizard Publiceren Azure-toepassing.

## <a name="choosing-your-subscription"></a>Uw abonnement te kiezen

### <a name="to-choose-a-subscription"></a>Een abonnement kiezen

1. Voordat u de wizard voor het eerst gebruiken, moet u zich aanmelden. Kies de koppeling **Sign In** . Meld u aan bij de Azure portal wanneer u wordt gevraagd en geef uw Azure gebruikersnaam en wachtwoord. 

    ![Dit is een van de publishing wizard-schermen](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    De lijst met abonnementen wordt gevuld met het abonnement dat is gekoppeld aan uw account. Ziet u mogelijk ook abonnementen uit abonnementsbestanden die u eerder hebt geïmporteerd.

1. Kies in de lijst **Kies uw abonnement** het abonnement voor deze installatie wilt gebruiken.

   Als u **< beheren >**kiest, verschijnt het dialoogvenster **Abonnementen beheren** en kunt u het abonnement en gebruiker account dat u wilt gebruiken. Het tabblad **Accounts** ziet u al uw accounts en het tabblad **abonnementen** ziet u alle abonnementen die is gekoppeld aan de accounts. U kunt ook een gebied van waaruit u kunt gebruik Azure bronnen, evenals maken of importeren van certificaten voor uw abonnement van de Azure-portal. Als u abonnementen hebt geïmporteerd uit een abonnementsbestand, weergegeven de bijbehorende certificaten onder het tabblad **certificaten** . Wanneer u klaar bent, kiest u de knop **sluiten** .

    ![Abonnementen beheren](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] Een abonnementsbestand kan meer dan één abonnement bevatten.

1. Kies de knop **volgende** om door te gaan. 

    Als er niet een cloudservices van uw abonnement, moet u een cloudservice in Azure wordt aangegeven voor het hosten van uw project te maken. Het dialoogvenster **Cloudservice maken en opslag-Account** wordt weergegeven.

    Geef een nieuwe naam voor de cloudservice. De naam moet uniek zijn in Azure wordt aangegeven. Geef een regio of affiniteit groep waaraan u een datacenter dat zich dicht bij u of het grootste deel van uw klanten. Deze naam wordt ook gebruikt voor een nieuw account voor de opslag dat Azure wordt gemaakt voor uw cloudservice.

1. Wijzig de instellingen die u wilt gebruiken voor deze implementatie en vervolgens publiceren door te kiezen van de knop **publiceren** (het volgende gedeelte vindt u meer informatie over de verschillende instellingen). Als u wilt de instellingen voor de publicatie te controleren, kiest u de knop **volgende** .

    >[AZURE.NOTE] Als u ervoor hebt gekozen publiceren in deze stap, kunt u de status van deze implementatie in Visual Studio kunt controleren.

U kunt veelvoorkomende en geavanceerde instellingen voor een implementatie wijzigen met behulp van de wizard **Publiceren Azure-toepassing** . U kunt bijvoorbeeld een instelling voor uw toepassing naar een testomgeving implementeren voordat u deze uitgebracht. De volgende afbeelding ziet u het tabblad **Algemene instellingen** voor een Azure-implementatie.

![Algemene instellingen](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## <a name="configuring-your-publish-settings"></a>Configuratie van uw publicatie-instellingen

### <a name="to-configure-the-publish-settings"></a>De publicatie-instellingen configureren

1. Klik in de lijst **cloudservice** voert u een van de volgende set stappen:

   1. Kies een bestaande cloudservice in de vervolgkeuzelijst. De locatie voor het beheercentrum van gegevens voor de service wordt weergegeven. U moet deze locatie te noteren en zorg ervoor dat de opslaglocatie van het account in het dezelfde Datacenter.

    1. Kies **Nieuw** om te maken van een cloudservice die Azure hosts. Geef een naam voor de service en geef een regio of affiniteit groep naar de locatie van het datacenter dat u wilt deze cloudservice hosten opgeven in het dialoogvenster **Cloudservice maken** . De naam moet uniek zijn in Azure wordt aangegeven.

1. Klik in de lijst **omgeving** **productie** of **tijdelijke**te kiezen. Kies de testomgeving als u wilt uw toepassing naar een testomgeving implementeren. U kunt uw toepassing naar de productieomgeving verplaatsen.

1. Klik in de lijst **configuratie bouwen** **Foutopsporing** of **Release**te kiezen.

1. Kies **Cloud** of **lokale**in de lijst **Service te configureren** .

    Schakel het selectievakje **Extern bureaublad voor alle rollen inschakelen** als u wilt kunnen extern verbinding maken met de service. Deze optie hoofdzakelijk wordt gebruikt voor probleemoplossing. Wanneer u dit selectievakje selecteert, wordt het dialoogvenster **Configuratie voor extern bureaublad** wordt weergegeven. Kies de koppeling instellingen voor de configuratie wilt wijzigen.

    Schakel het selectievakje **Web implementeren inschakelen voor alle web rollen** om in te schakelen van de web-implementatie voor de service. U kunt Extern bureaublad voor deze functie moet inschakelen. Zie [[publiceren van een Cloudservice met de hulpmiddelen Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx)voor meer informatie. Zie [[een Cloudservice met de hulpmiddelen Azure publiceren](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx)voor meer informatie over het implementeren van Web.

1. Kies het tabblad **Geavanceerde instellingen** . In het veld **implementatie label** accepteer de standaardnaam of typ een naam van uw keuze. Als u wilt de datum toevoegt aan het label implementatie, laat u het selectievakje ingeschakeld.

    ![Derde scherm van de Wizard publiceren](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. Klik in de lijst **opslag-account** door het account opslagruimte wilt gebruiken voor deze installatie te kiezen. Vergelijk de locaties van de datacenters voor uw cloudservice en uw account opslag. In het ideale geval moet deze locaties hetzelfde.

    >[AZURE.NOTE] Het account Azure opslag slaat het pakket voor de toepassingsimplementatie. Nadat de toepassing wordt geïmplementeerd, wordt het pakket wordt verwijderd uit het opslag-account.

1. Schakel het selectievakje **implementatie bijwerken** als u wilt implementeren alleen bijgewerkte onderdelen. Dit soort implementatie kan zijn sneller dan een volledige implementatie. Kies de koppeling **Instellingen voor** de **implementatie-instellingen bijwerken** om dialoogvenster te openen, weergegeven in de volgende afbeelding. 

    ![Implementatie-instellingen](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    U kunt een van de twee opties voor bijwerken implementatie, incrementele of tegelijk. Een incrementele implementatie bijgewerkt geïmplementeerd eenmalig per keer, zodat uw toepassing online en beschikbaar voor gebruikers blijft. Een tegelijk implementatie overal geïmplementeerd in één keer worden bijgewerkt. Gelijktijdige update is sneller dan incrementele update, maar als u deze optie kiest, uw toepassing mogelijk niet beschikbaar tijdens het updateproces.

    Moet u het selectievakje voor als implementatie kan niet worden bijgewerkt, een volledige implementatie doen als u wilt dat de volledige implementatie automatisch worden uitgevoerd als een update-implementatie is mislukt. Een volledige implementatie Hiermee stelt u het virtuele IP-(VIP) adres voor de cloudservice opnieuw. Zie voor meer informatie [hoe: behouden een Constant virtuele IP-adres voor een Cloudservice](https://msdn.microsoft.com/library/azure/jj614593.aspx).


1. Schakel het selectievakje **IntelliTrace inschakelen** voor foutopsporing op uw service, of als u een configuratie **Foutopsporing** implementeert en wilt opsporen van uw cloudservice in Azure wordt aangegeven, selecteert u het selectievakje **Externe foutopsporing inschakelen voor alle rollen** voor het implementeren van de externe foutopsporing services.

2. Schakel het selectievakje **profielen inschakelen** om het profiel van de toepassing, en kies vervolgens de koppeling **Instellingen** om de profielbeleid opties. 


    >[AZURE.NOTE] Hebt u Visual Studio Ultimate IntelliTrace of laag interactie profielen (TIP) wilt inschakelen en u kunt niet beide tegelijkertijd inschakelen.

    Zie voor meer informatie [voor foutopsporing in een Cloudservice die zijn gepubliceerd met IntelliTrace en Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx) en [testen van de prestaties van een Cloudservice](https://msdn.microsoft.com/library/azure/hh369930.aspx).

1. Kies **volgende** om weer te geven van de overzichtspagina voor de toepassing.

## <a name="publishing-your-application"></a>Uw toepassing publiceren

1. U kunt een publicatie-profiel maken in de instellingen die u hebt gekozen. U kunt bijvoorbeeld één standaardprofiel voor een testomgeving en een andere voor productie maken. Kies het pictogram **Opslaan** om op te slaan dit profiel. De wizard profiel wordt gemaakt en opgeslagen in het project Visual Studio. U wijzigt de profielnaam, open de lijst **doelprofiel** en kies **< beheren >**.

    ![Scherm Samenvatting van de Wizard publiceren](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] De publicatie profiel wordt weergegeven in de Verkenner oplossing in Visual Studio en de profielinstellingen naar een bestand met de extensie .azurePubxml zijn geschreven. Instellingen worden opgeslagen als kenmerken van XML-codes.

1. Kies **publiceren** naar uw toepassing publiceren. U kunt de status van proces in het venster **uitvoer** in Visual Studio controleren.

## <a name="see-also"></a>Zie ook

[Hoe u: migreren en publiceren van een webtoepassing op een Azure Cloudservice vanuit Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[Een Cloudservice met de hulpmiddelen Azure publiceren](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[Voor foutopsporing in een gepubliceerde Cloudservice met IntelliTrace en Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[De prestaties van een Cloudservice testen](https://msdn.microsoft.com/library/azure/hh369930.aspx)

