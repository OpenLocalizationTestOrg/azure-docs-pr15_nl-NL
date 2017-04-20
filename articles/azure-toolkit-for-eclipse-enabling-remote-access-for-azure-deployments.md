<properties
    pageTitle="Externe toegang inschakelen voor Azure implementaties in Eclips"
    description="Leer hoe u externe toegang voor Azure implementaties met behulp van de Azure-Toolkit voor Eclips inschakelen."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Externe toegang inschakelen voor Azure implementaties in Eclips

U kunt om u te helpen oplossen van uw implementaties, inschakelen en RAS met verbinding maken met de virtuele machine hostingprovider van uw implementatie. De functionaliteit voor externe toegang is gebaseerd op de Remote Desktop Protocol (RDP). Nadat u deze op Azure hebt gepubliceerd, of als u Eclips met een Windows-besturingssysteem gebruikt, u RAS configureren kunt voordat u naar Azure publiceren, kunt u externe toegang voor uw implementatie. Houd er rekening mee dat u moet een extern-bureaubladclient die compatibel is met uw besturingssysteem de verbinding met de implementatie van virtuele machine in Azure wordt aangegeven.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Hoe u externe toegang inschakelen voordat u bij Azure implementeert

> [AZURE.NOTE] Als u wilt externe toegang inschakelen voordat u de toepassing Azure implementeert, moet u Eclips worden uitgevoerd op Windows.

De volgende afbeelding ziet u de **RAS** -eigenschappen dialoogvenster kon u externe toegang inschakelen.

![][ic719494]

Er zijn twee manieren om het dialoogvenster **RAS** -eigenschappen weer te geven:

* Klik op de koppeling **Geavanceerd** in het gedeelte **RAS** van het dialoogvenster **publiceren op Azure** .
* Open het dialoogvenster **Eigenschappen** van uw Azure project.

Wanneer u een nieuw project van Azure-implementatie maakt, is het project hebben geen externe toegang standaard ingeschakeld. U kunt echter gemakkelijk externe toegang inschakelen door het opgeven van de gebruikersnaam en wachtwoord in het dialoogvenster **publiceren op Azure** . Het wachtwoord voor externe toegang is versleuteld met x.509-certificaten. Als u geen gebruik maakt vindt u in uw eigen certificaat, de versleuteling is afhankelijk van een zelfondertekend certificaat dat is verzonden met de Azure-invoegtoepassing voor Eclips. Dit zelfondertekend certificaat is in de map **certificaat** van uw Azure project, die zijn opgeslagen als een openbare certificaatbestand (SampleRemoteAccessPublic.cer) en als het bestand in een persoonlijke informatie Exchange (PFX)-certificaat (SampleRemoteAccessPrivate.pfx). Deze laatste bevat de persoonlijke sleutel voor het certificaat en er een standaardwachtwoord, **Wachtwoord1**. Echter, aangezien dit wachtwoord algemeen bekend is, kan de standaardcertificaat moet worden gebruikt alleen voor leermateriaal, niet voor een productie-implementatie. Dus anders dan leermateriaal, wanneer u wilt externe sessies voor uw implementaties ingeschakeld u moet klikt u op de koppeling **Geavanceerd** in het dialoogvenster **publiceren naar Azure** om op te geven van uw eigen certificaat. Houd er rekening mee dat u moet de PFX-versie van het certificaat uploaden naar uw gehoste service binnen de beheerportal Azure, zodat deze Azure wachtwoord van de gebruiker kunt ontsleutelen.

De rest van de zelfstudie ziet u hoe u externe toegang inschakelen voor een Azure-implementatie-project dat oorspronkelijk is gemaakt met externe toegang is uitgeschakeld. Voor toepassing van deze zelfstudie, gaan we een nieuwe zelfondertekend certificaat maken en de .pfx-bestand heeft een wachtwoord van uw keuze. U hebt ook de optie van het gebruik van een certificaat dat is uitgegeven door een certificeringsinstantie.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Hoe u externe toegang inschakelen nadat u hebt geïmplementeerd in Azure

Om externe toegang inschakelen nadat u hebt geïmplementeerd in Azure, gebruikt u de volgende stappen:

1. Meld u aan bij de Azure beheerportal via uw Azure-account
1. In de lijst met **Cloudservices**, selecteert u de cloudservice gedistribueerde
1. Klik op de koppeling **configureren** op de webpagina, voor de cloud-service
1. Vanaf de onderkant van de configuratiepagina, klikt u op de **externe** koppeling
1. Wanneer het pop-dialoogvenster wordt weergegeven:
    * Geef de rol waarvoor u wilt externe toegang inschakelen
    * Schakel het selectievakje **Extern bureaublad inschakelen**
    * Geef een gebruikersnaam en wachtwoord die u wilt gebruiken voor externe toegang
    * Selecteer het certificaat gebruiken
1. Klik op **OK** 

Hier ziet u een bericht weergegeven dat uw configuratiewijziging wordt uitgevoerd, dit kan een paar minuten duren. Nadat het wijzigen van de configuratie is voltooid, volgt u de stappen in de sectie **extern aanmelden** verderop in dit artikel.
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Hoe u externe toegang inschakelen in het pakket

1. Vanuit de Eclips Projectverkenner deelvenster met de rechtermuisknop op uw Azure project en klik op **Eigenschappen**.

1. In het dialoogvenster **Eigenschappen van** **Azure** uitvouwen in het linkerdeelvenster en klikt u op **Externe toegang**.

1. Controleer of **dat alle rollen externe bureaublad-verbindingen met deze aanmeldingsreferenties accepteert inschakelen** is ingeschakeld in het dialoogvenster **Externe toegang** .

1. Geef een naam voor de verbinding met extern bureaublad.

1. Geef op en Bevestig het wachtwoord voor de gebruiker. De waarden gebruikersnaam en wachtwoord instellen in dit dialoogvenster wordt gebruikt wanneer u een verbinding met extern bureaublad. (Houd er rekening mee dat dit een afzonderlijk wachtwoord van uw wachtwoord PFX is.)

1. De verloopdatum voor de gebruikersaccount opgeven.

1. Klik op **Nieuw** om een nieuwe zelfondertekend certificaat maken. (U kunt ook kunt u een certificaat respectievelijk van uw systeem werkruimte of een bestand via de knoppen **werkruimte** of een **bestandssysteem** selecteren, maar voor toepassing van deze zelfstudie gaan we een nieuw certificaat maken.)

    * Geef in het dialoogvenster **Nieuw certificaat** en Bevestig het wachtwoord dat u voor uw PFX-bestand gebruikt.

    * Accepteer de waarde voor **CN (naam)**of een aangepaste naam gebruiken.

    * Geef het pad en de bestandsnaam waar het nieuwe certificaat, in het formulier voor .cer worden opgeslagen. Voor deze stap en de volgende stap, kunt u de map **certificaat** van uw Azure project, maar u kunt een andere locatie kiezen. Voor toepassing van deze zelfstudie gebruiken we **c:\mycert\mycert.cer**. (De map **c:\mycert** voordat u verder gaat maken, of gebruik een bestaande map desgewenst.)

    * Geef het pad en de bestandsnaam waar het nieuwe certificaat en de persoonlijke sleutel, in het formulier voor .pfx worden opgeslagen. Voor toepassing van deze zelfstudie gebruiken we **c:\mycert\mycert.pfx**. Het dialoogvenster **Nieuw certificaat** moet lijken op de volgende (de mappaden bijwerken als u geen **c:\mycert**hebt gebruikt):

        ![][ic712275]

    * Klik op **OK** om het dialoogvenster **Nieuw certificaat** te sluiten.

1. Uw **RAS** -dialoogvenster ziet er ongeveer als volgt uit:</p>

    ![][ic719495]

1. Klik op **OK** om het dialoogvenster **Remote Access** te sluiten.
    
Bouw die tabellen opnieuw uw-toepassing, met het bouwen instellen voor implementatie naar cloud.

## <a name="to-log-in-remotely"></a>Extern aanmelden

Nadat u uw exemplaar van de rol klaar is, kunt u extern aanmelden bij de virtuele machine die als voor uw toepassing host fungeert.

* Als Eclips gebruikt in Windows en u de optie **Extern bureaublad Start op implementeren** tijdens uw implementatie van Azure geselecteerd, wordt weergegeven met een verbinding met extern bureaublad-aanmeldingsscherm wanneer uw implementatie wordt gestart. Wanneer u wordt gevraagd om de gebruikersnaam en wachtwoord, voer de waarden die u hebt opgegeven voor de externe gebruiker en kunnen aan te melden.

* Een andere manier aan te melden op afstand is via de <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure Management Portal</a>:

    * In de **Cloud Services** -weergave van de Azure-beheerportal, klikt u op uw cloudservice op **exemplaren**, klikt u op een specifiek exemplaar, en klik vervolgens op de knop **verbinding maken** . De knop **verbinding maken** wordt weergegeven als de volgende handelingen uit in de opdrachtenbalk:

        ![][ic659273]

    * Wanneer u op de knop **verbinden** , wordt u gevraagd een RDP-bestand te openen. Open het bestand en volg de aanwijzingen. (U kunt dit bestand ook opslaan naar uw lokale computer, en voer het bestand door erop te dubbelklikken aan externe Meld u aan op uw virtuele computer zonder dat moet worden gaat eerst de beheerportal).

    * Wanneer u wordt gevraagd om de gebruikersnaam en wachtwoord, voer de waarden die u hebt opgegeven voor de externe gebruiker en kunnen aan te melden.

> [AZURE.NOTE] Als u op een niet-Windows-besturingssysteem, moet u een extern bureaublad die compatibel is met uw besturingssysteem gebruiken en volg de stappen voor het configureren van die client met de instellingen in het RDP-bestand dat u hebt gedownload.

## <a name="see-also"></a>Zie ook

[Azure Toolkit voor Eclips][]

[Maken van een toepassing van de wereld Hallo voor Azure in Eclips][]

[Installatie van de Azure Toolkit voor Eclips][] 

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png
