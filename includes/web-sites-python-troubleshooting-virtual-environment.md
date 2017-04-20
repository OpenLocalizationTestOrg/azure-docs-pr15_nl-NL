De implementatiescript slaat het maken van de virtuele omgeving op Azure als wordt vastgesteld dat een compatibele omgeving die is virtual al bestaat.  Dit kunt aanzienlijk implementatie versnellen.  Pakketten die al zijn geïnstalleerd worden overgeslagen door pip.

In sommige gevallen wilt u mogelijk afdwingen die virtuele omgeving verwijderen.  Wilt u dit doen als u een virtuele omgeving opnemen als onderdeel van uw bibliotheek.  U kunt ook hiervoor als u moet bepaalde pakketten weghalen of wijzigingen in requirements.txt te testen.

Er zijn een paar opties voor het beheren van de bestaande virtuele omgeving op Azure:

### <a name="option-1-use-ftp"></a>Optie 1: FTP gebruiken

Verbinding maken met de server met een FTP-client, en u kunt wel de envelop-map verwijderen.  Houd er rekening mee dat sommige FTP-clients (zoals webbrowsers) mogelijk alleen-lezen en dat het niet mogelijk te verwijderen voor mappen, zodat u zorgen moet voor het gebruik van een FTP-client met die mogelijkheid.  De FTP-hostnaam en de gebruiker worden weergegeven in uw web-app blade op de [Azure-Portal](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Optie 2: De wisselknop runtime

Hier ziet u een alternatief die van het feit gebruikmaakt dat de implementatiescript de envelop-map wordt verwijderd nadat deze niet overeenkomen met de gewenste versie van Python.  Hiermee wordt effectief verwijderen van de bestaande omgeving, en een nieuwe record maken.

1. Overschakelen naar een andere versie van Python (via runtime.txt of het blad **Toepassingsinstellingen** in de Portal Azure)
1. cijfer push sommige wijzigingen (negeren eventuele fouten bij de installatie van de pip indien van toepassing)
1. Ga terug naar de eerste versie van Python
1. cijfer push sommige wijzigingen opnieuw

### <a name="option-3-customize-deployment-script"></a>Optie 3: Implementatiescript aanpassen

Als u de implementatiescript hebt aangepast, kunt u de code in deploy.cmd te dwingen te verwijderen van de envelop-map wijzigen.
