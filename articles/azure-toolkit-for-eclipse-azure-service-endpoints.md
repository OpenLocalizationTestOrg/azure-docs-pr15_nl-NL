<properties
    pageTitle="Azure-Service eindpunten"
    description="Beschrijving van de Service-eindpunt Azure-instellingen in de Toolkit Azure voor Eclips."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Azure-Service eindpunten #

Azure-service eindpunten Hiermee bepaalt u of uw toepassing is geïmplementeerd in en beheerd door de globale Azure platform, Azure dat wordt beheerd door 21Vianet in China, of een privé Azure platform. Het dialoogvenster **Service eindpunten** kunt u opgeven welke service-eindpunten die u wilt gebruiken. U opent het dialoogvenster **Service eindpunten** binnen Eclips, klik op **venster**, op **Voorkeuren**, **Azure**uitvouwen en klik vervolgens op **Service-eindpunten**. Het veld **Actieve instellen** bepaalt u welke eindpunten Azure-service wordt gebruikt voor de Azure projecten in uw huidige werkruimte.

Hieronder ziet u het dialoogvenster **Service-eindpunten** .

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>De service-eindpunten instellen ##

Klik in het dialoogvenster **Service eindpunten** gaat u als een van de volgende bewerkingen uit:

* Als u wilt gebruiken van de globale Azure platform, in de vervolgkeuzelijst **Actieve instellen** **windowsazure.com** selecteren en klik op **OK**.
* Als u wilt gebruiken dat wordt beheerd door 21Vianet in China, in de vervolgkeuzelijst **Actieve instellen** Azure Selecteer **windowsazure.cn (China)** en klik op **OK**.
* Als u een privé Azure platform gebruiken wilt:
    1. Klik op **bewerken**.
    2. Een dialoogvenster wordt geopend, waarin wordt gemeld dat het dialoogvenster **Service eindpunten** wordt gesloten, en de voorkeur sets-bestand wordt geopend. Klik op **OK**.
    3. Maak een nieuw in het bestand preferencesets.xml `preferenceset` element. Voor deze nieuwe element maken `name`, `blob`, `management`, `portalURL` en `publishsettings` kenmerken en waarden voor deze die overeenkomen toevoegen aan uw persoonlijke Azure platform. U kunt de waarden die zijn opgegeven voor de bestaande `preferenceset` elementen als sjablonen. **Opmerking**: de waarde die wordt gebruikt voor de `blob` kenmerk moet de tekst 'blob' in de URL bevatten.
    4. Opslaan en sluiten preferencesets.xml.
    5. Open opnieuw het dialoogvenster **Service-eindpunten** .
    6. In de vervolgkeuzelijst **Actieve instellen** , selecteert u de actieve set die u hebt gemaakt en klik op **OK**.
    7. Nadat u uw persoonlijke Azure platform hebt gemaakt `preferenceset` element, kunt u de waarden die zijn toegewezen aan deze door te klikken op de knop **bewerken** in het dialoogvenster **Services eindpunt** wijzigen. U kunt ook meerdere privé Azure platform maken `preferenceset` elementen, als u om wat.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Installatie van de Azure Toolkit voor Eclips][] 

[Maken van een toepassing van de wereld Hallo voor Azure in Eclips][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png
