<properties
    pageTitle="Het genereren en toetsen HSM-beveiliging doorverbinden voor Azure-toets kluis | Microsoft Azure"
    description="Lees dit artikel om te helpen u plannen voor, genereren en brengt u uw eigen sleutels HSM-beveiliging voor gebruik met Azure-toets kluis."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Het genereren en overbrengen HSM beveiligde sleutels voor Azure-toets kluis

##<a name="introduction"></a>Inleiding

Voor extra assurance, wanneer u Azure-toets kluis, gebruikt kunt u importeren of toetsen in hardware beveiligingsmodules (HSM's) die nooit de grens HSM laat genereren. Dit scenario wordt vaak *doen om uw eigen code*of BYOK genoemd. De HSM's zijn FIPS 140-2 niveau 2 gevalideerd. Azure-toets kluis wordt Thales nShield reeks HSM's gebruikt om te beveiligen van uw sleutels.

Gebruik de informatie in dit onderwerp om te helpen u plannen voor, genereren en brengt u uw eigen sleutels HSM-beveiliging voor gebruik met Azure-toets kluis.

Deze functie is niet beschikbaar voor Azure China. 

>[AZURE.NOTE] Zie voor meer informatie over Azure-toets kluis, [Wat Azure-toets kluis is?](key-vault-whatis.md)  
>
>Zie voor een ophalen gestart zelfstudie, inclusief het maken van een belangrijke kluis voor sleutels HSM is beveiligd, [aan de slag met Azure toets kluis](key-vault-get-started.md).

Meer informatie over het genereren en overbrengen van een sleutel HSM beveiligd via Internet:

- U de toets vanaf een offline werkstation, waardoor aanvallen genereren.

- De sleutel is versleuteld met een toets Exchange sleutel (KEK), dat versleutelde blijft voordat deze worden overgebracht naar de HSM Azure-toets kluis's. Alleen de versleutelde versie van uw sleutel verlaat het oorspronkelijke werkstation.

- De werkset stelt eigenschappen van uw tenant-sleutel waaraan uw sleutel is gebonden werelds beveiliging Azure-toets kluis. Dus nadat de HSM Azure-toets kluis's ontvangen en uw sleutel decoderen, kunnen alleen deze HSM's gebruiken. Uw sleutel kan niet worden geëxporteerd. Deze binding wordt afgedwongen door de Thales HSM's.

- De toets Exchange sleutel (KEK) die wordt gebruikt voor het coderen van uw sleutel wordt gegenereerd binnen de HSM Azure-toets kluis's en kan niet worden geëxporteerd. De HSM's afdwingen dat er geen wissen versie van de KEK buiten de HSM's kunnen zijn. Daarnaast bevat de werkset verklaring van Thales dat de KEK kan niet worden geëxporteerd en binnen een legitieme HSM die is gemaakt door Thales is gegenereerd.

- De werkset bevat verklaring van Thales dat de wereld van Azure-toets kluis beveiliging ook op een legitieme HSM geproduceerd door Thales is gegenereerd. Deze verklaring blijkt dat u dat Microsoft genuine hardware gebruikt.

- Microsoft afzonderlijk KEKs gebruikt en scheidt u beveiliging wereld in elke geografische regio. Deze scheiding zorgt ervoor dat uw sleutel kan alleen worden gebruikt in datacenters in de regio waarin u deze gecodeerd. Een sleutel van een Europees klant niet kan bijvoorbeeld worden gebruikt in datacenters in Noord-Amerikaanse of Azië.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Meer informatie over Thales HSM's en Microsoft-services

Thales e-beveiliging is een toonaangevende algemene provider van coderen en cyber beveiligingsoplossingen voor de financiële services, hoog technologie manufacturing, government en technologie sectoren. Met een 40 jaar bogen beschermen bedrijfs en de overheid informatie, worden Thales oplossingen met vier van de vijf grootste energie en ruimtevaart bedrijven gebruikt. Bijbehorende oplossingen ook worden gebruikt door 22 NAVO landen en beveiligen van meer dan 80% van de overal ter wereld betalingstransacties.

Microsoft heeft met Thales kunnen de status van de illustratie voor HSM's verbeteren samengewerkt. Deze verbeteringen kunnen u de normale voordelen van gehoste services zonder prijsgeeft controle over uw sleutels. Specifiek, laat deze verbeteringen Microsoft de HSM's beheren, zodat u niet te hoeft. Als een cloudservice schalen Azure-toets kluis op korte termijn moeten voldoen van uw organisatie gebruik pieken omhoog. Tegelijkertijd, uw sleutel is beveiligd in Microsoft HSM's: U Houd controle over de levenscyclus van de belangrijkste omdat u de toets genereren en naar Microsoft HSM's overbrengen.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Uw eigen code (BYOK) implementeren zorgen van Azure-toets kluis

Gebruik van de volgende informatie en procedures, als u uw eigen sleutel HSM beveiligde genereren en vervolgens naar Azure-toets kluis overbrengen — de voren uw situatie sleutel (BYOK).


##<a name="prerequisites-for-byok"></a>Vereisten voor BYOK

Zie de volgende tabel voor een lijst met vereisten voor uw eigen code (BYOK) zorgen van Azure-toets kluis.

|Vereiste|Meer informatie|
|---|---|
|Een abonnement op Azure|Als u wilt een Azure-toets kluis hebt gemaakt, moet u een Azure-abonnement: [registreren voor gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)|
|De laag Azure-toets kluis Premium service ter ondersteuning van toetsen HSM-beveiliging|Zie de website van [Azure toets kluis prijzen](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie over de Servicelagen en mogelijkheden voor Azure-toets kluis.|
|Thales HSM, smartcards en voor ondersteuningssoftware|U moet toegang hebben tot een Thales Hardware Security Module en operationele basiskennis van Thales HSM's. Zie [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) voor de lijst met compatibele modellen, of een HSM kopen als u geen hebt.|
|De volgende hardware en software:<ol><li>Een offline x64 workstation met een minimale Windows-besturingssysteem van Windows 7 en Thales nShield-software die is ten minste versie 11.50.<br/><br/>Als dit werkstation wordt uitgevoerd van Windows 7, moet u [Microsoft .NET Framework 4.5 installeren](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Een werkstation dat is verbonden met Internet en heeft een minimale Windows-besturingssysteem van Windows 7.</li><li>Een USB-station of andere draagbare opslagapparaat met ten minste 16 MB ruimte vrij te geven.</li></ol>|Om beveiligingsredenen wordt aangeraden het eerste werkstation is niet verbonden met een netwerk. Deze aanbeveling is echter niet via programmacode afgedwongen.<br/><br/>Houd er rekening mee dat in de instructies die u volgt, dit werkstation het verbroken werkstation heet.</p></blockquote><br/>Als uw tenant-code voor een productienetwerk is, is het bovendien raadzaam te waarmee u een tweede, afzonderlijk workstation kunt downloaden van de set hulpmiddelen en uploadt u de tenant-toets. Maar voor testdoeleinden, kunt u hetzelfde werkstation gebruiken als de eerste fase.<br/><br/>Houd er rekening mee dat in de instructies die u volgt, dit tweede werkstation het werkstation internetverbinding heet.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Genereren en uw sleutel overbrengen naar Azure-toets kluis HSM

U kunt de volgende vijf stappen genereren en uw sleutel overbrengen naar een Azure-toets kluis HSM:

- [Stap 1: Uw internetverbinding werkstation voorbereiden](#step-1-prepare-your-internet-connected-workstation)
- [Stap 2: Uw verbroken werkstation voorbereiden](#step-2-prepare-your-disconnected-workstation)
- [Stap 3: Uw code genereren](#step-3-generate-your-key)
- [Stap 4: Uw sleutel voor bestandsoverdracht voorbereiden](#step-4-prepare-your-key-for-transfer)
- [Stap 5: Uw sleutel overbrengen naar Azure-toets kluis](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Stap 1: Uw internetverbinding werkstation voorbereiden
Voor deze eerste stap, doet u de volgende procedures uit op uw werkstation dat is verbonden met Internet.


###<a name="step-11-install-azure-powershell"></a>Stap 1.1: Azure PowerShell installeren

Vanuit het werkstation internetverbinding downloaden en installeren van de Azure PowerShell-module met de cmdlets uit om het beheren van Azure-toets kluis. U moet hiervoor een minimale versie van 0.8.13.

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor installatie-instructies.

###<a name="step-12-get-your-azure-subscription-id"></a>Stap 1.2: Uw Azure abonnements-ID ophalen

Een Azure PowerShell-sessie starten en meld u aan bij uw Azure-account met behulp van de volgende opdracht uit:

        Add-AzureAccount
Voer uw gebruikersnaam in te voeren Azure-account en wachtwoord in het pop-browservenster. Gebruik vervolgens de opdracht [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Zoek vanaf de uitvoer, de ID voor het abonnement dat u voor Azure-toets kluis gebruiken wilt. Moet u deze abonnements-ID later.

Sluit het venster Azure PowerShell niet.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Stap 1.3: De werkset BYOK voor Azure-toets kluis downloaden

Ga naar het Microsoft Download Center en [de werkset Azure toets kluis BYOK downloaden](http://www.microsoft.com/download/details.aspx?id=45345) voor uw geografische regio of exemplaar van Azure. Raadpleeg de volgende informatie om aan te geven van de pakketnaam van het wilt downloaden en de bijbehorende SHA-256 pakket hash:

---

**Noord-Amerika:**

KeyVault-BYOK-hulpmiddelen-Verenigd States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Europa:**

KeyVault-BYOK-hulpmiddelen-Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Azië:**

KeyVault-BYOK-hulpmiddelen-AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Latijns-Amerika:**

KeyVault-BYOK-hulpmiddelen-LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Japan:**

KeyVault-BYOK-hulpmiddelen-Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Australië:**

KeyVault-BYOK-hulpmiddelen-Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure overheid:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-hulpmiddelen-USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Canada:**

KeyVault-BYOK-hulpmiddelen-Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Duitsland:**

KeyVault-BYOK-hulpmiddelen-Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**India:**

KeyVault-BYOK-hulpmiddelen-India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Valideren van de integriteit van uw BYOK set hulpmiddelen gedownloade, vanuit uw Azure PowerShell-sessie, gebruikt u de cmdlet [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) .

    Get-FileHash KeyVault-BYOK-Tools-*.zip

De werkset omvat het volgende:

- Een toets Exchange sleutel (KEK)-pakket waarvan de naam begint met **BYOK-KEK - pak-.**
- Een pakket beveiliging wereld waarvan de naam begint met **BYOK-SecurityWorld - pak-.**
- Een benoemde v python-script**erifykeypackage.py.**
- Een opdrachtregel uitvoerbaar bestand benoemde **KeyTransferRemote.exe** en bijbehorende DLL-bestanden.
- Een visuele C++ opnieuw te distribueren pakket, met de naam **vcredist_x64.exe.**

Het pakket kopiëren naar een USB-station of andere draagbare opslag.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Stap 2: Uw verbroken werkstation voorbereiden

Voor deze tweede stap, doet u de volgende procedures uit op het werkstation dat niet met een netwerk (Internet of het interne netwerk verbonden is).


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Stap 2.1: Het verbroken werkstation met Thales HSM voorbereiden

De nCipher (Thales) ondersteuning voor software installeren op een Windows-computer, en voeg vervolgens een HSM Thales op deze computer.

Zorg ervoor dat de Thales hulpmiddelen in het pad (**%nfast_home%\bin** en **%nfast_home%\python\bin**). Typ bijvoorbeeld het volgende:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Zie de gebruikershandleiding wordt geleverd bij de HSM Thales voor meer informatie.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Stap 2.2: Installeer de werkset BYOK op het verbroken werkstation

Het pakket van de set hulpmiddelen BYOK van de USB-station of andere draagbare opslag kopiëren en vervolgens als volgt te werk:

1. Pak de bestanden uit het gedownloade pakket naar een map.
2. Voer vcredist_x64.exe uit die map.
3. Volg de instructies voor de installatie de visuele C++ runtime-onderdelen voor Visual Studio-2013.

##<a name="step-3-generate-your-key"></a>Stap 3: Uw code genereren

Voor deze derde stap, doet u de volgende procedures uit op het verbroken werkstation.

###<a name="step-31-create-a-security-world"></a>Stap 3.1: Maak een wereld beveiliging

Start een opdrachtprompt en start het programma van de nieuwe wereld Thales.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Dit programma maakt een bestand **Beveiliging wereld** op % NFAST_KMDATA%\local\world, dat met de map C:\ProgramData\nCipher\Key Management Settings\User overeenkomt. U kunt verschillende waarden gebruiken voor het quorum, maar in ons voorbeeld, wordt u gevraagd om in te voeren drie lege kaarten en pincodes voor elke record. Elke twee kaarten geven vervolgens volledige toegang tot de wereld beveiliging. Deze kaarten worden de **Beheerder kaart ingesteld** voor de nieuwe beveiliging wereld.

Doe het volgende:

- Back-up van de wereld-bestand. Secure en beveiligen van de wereld-bestand, de beheerder kaarten en hun pin en zorg ervoor dat één niemand toegang tot meer dan één kaart heeft.

###<a name="step-32-validate-the-downloaded-package"></a>Stap 3,2: De gedownloade pakket valideren

Deze stap is optioneel, maar aanbevolen zodat u het volgende valideren kunt:

- De Exchange Key die is opgenomen in de werkset is gegenereerd uit een legitieme Thales HSM.
- De hash van de wereld beveiliging die is opgenomen in de werkset is gegenereerd in een legitieme Thales HSM.
- De Exchange Key kan worden niet geëxporteerd.

>[AZURE.NOTE]Als u wilt de gedownloade pakket valideren, de HSM mag niet worden verbonden, ingeschakeld, en moet een wereld beveiliging op is geïnstalleerd (zoals het account dat u zojuist hebt gemaakt).

Voor het valideren van het gedownloade pakket:

1.  De verifykeypackage.py-script uitvoeren door te koppelen van een van de volgende opties, afhankelijk van uw geografische regio of exemplaar van Azure:
    - Voor Noord-Amerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Voor Europe:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Voor Azië:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Voor Latijns-Amerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Voor Japan:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Voor Australië:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - Voor [De overheid Azure](https://azure.microsoft.com/features/gov/), waarin het Amerikaanse overheid-exemplaar van Azure gebruikt:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Voor Canada:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Voor Duitsland:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Voor India:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]De software Thales bevat python bij %NFAST_HOME%\python\bin

2.  Bevestig dat u ziet het volgende voorbeeld, waarin wordt aangegeven validatie is geslaagd: **resultaat: SUCCES**

Dit script is gevalideerd met de keten ondertekenaar snel aan de sleutel van de hoofdmap Thales. De hash van deze toets hoofdsite wordt ingesloten in het script en de waarde moet **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. U kunt deze waarde ook afzonderlijk door een bezoek de [website van Thales](http://www.thalesesec.com/)te controleren.

U bent nu klaar om te maken van een nieuwe sleutel.

###<a name="step-33-create-a-new-key"></a>Stap 3.3: Maak een nieuwe sleutel

Genereer een sleutel met behulp van de Thales **generatekey** -programma.

Voer de volgende opdracht om de sleutel te genereren:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Wanneer u deze opdracht uitvoert, gebruikt u deze instructies:

- De parameter *beveiligen* moeten zijn ingesteld op de waarde- **module**, zoals wordt weergegeven. Hiermee maakt u een sleutel module is beveiligd. De werkset BYOK biedt geen ondersteuning voor toetsen OCS is beveiligd.

- Vervang de waarde van *contosokey* voor de **ident** en **plainname** door een tekenreekswaarde. Als u wilt minimaliseren bijkomende administratieve kosten en de kans op pesterijen fouten te verkleinen, is het raadzaam dat u dezelfde waarde voor beide gebruiken. De waarde **ident** moet bevatten, alleen getallen, streepjes en kleine letters.

- De pubexp resteert leeg (standaard) in dit voorbeeld, maar kunt u specifieke waarden. Zie de documentatie Thales voor meer informatie.

Deze opdracht maakt een toets ge? exeerd-bestand in uw map %NFAST_KMDATA%\local met een naam die beginnen met **key_simple_**, gevolgd door de **ident** die is opgegeven in de opdracht. Bijvoorbeeld: **key_simple_contosokey**. Dit bestand bevat een versleutelde sleutel.

Back-up van deze toets ge? exeerd bestand in een veilige locatie.

>[AZURE.IMPORTANT] Wanneer u uw sleutel later naar Azure-toets kluis overbrengt, kan geen Microsoft deze toets terug naar u exporteren, zodat deze verandert in uiterst belangrijke back-up van de wereld-toets en beveiliging veilig. Contact opnemen met Thales voor instructies en aanbevolen procedures voor het back-ups van uw code.

U bent nu klaar wilt overbrengen van uw sleutel naar Azure-toets kluis.

##<a name="step-4-prepare-your-key-for-transfer"></a>Stap 4: Uw sleutel voor bestandsoverdracht voorbereiden

Voor deze vierde stap, doet u de volgende procedures uit op het verbroken werkstation.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Stap 4.1: Een kopie van uw sleutel met beperkte machtigingen maken

Als u wilt de machtigingen voor uw sleutel van een opdrachtprompt verlagen, voert u een van de volgende opties, afhankelijk van uw geografische regio of exemplaar van Azure:

- Voor Noord-Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Voor Europe:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Voor Azië:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Voor Latijns-Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Voor Japan:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Voor Australië:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- Voor [De overheid Azure](https://azure.microsoft.com/features/gov/), waarin het Amerikaanse overheid-exemplaar van Azure gebruikt:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Voor Canada:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Voor Duitsland:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Voor India:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Wanneer u deze opdracht uitvoert, vervangt u *contosokey* met dezelfde waarde die u hebt opgegeven in **stap 3.3: Maak een nieuwe sleutel** van de stap [genereren uw sleutel](#step-3-generate-your-key) .

Sluit uw beveiliging wereld beheerder kaarten wordt u gevraagd.

Wanneer de opdracht is voltooid, ziet u **resultaat: SUCCES** en de kopie van uw sleutel met beperkte machtigingen zijn in het bestand met de naam key_xferacId_<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Stap 4.2: De nieuwe kopie van de sleutel controleren

(Optioneel) het Thales-hulpprogramma's om te bevestigen de minimale machtigingen voor de nieuwe sleutel uitvoeren:

- aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Wanneer u deze opdrachten uitvoeren, vervangt u contosokey met dezelfde waarde die u hebt opgegeven in **stap 3.3: Maak een nieuwe sleutel** van de stap [genereren uw sleutel](#step-3-generate-your-key) .

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Stap 4,3: Uw sleutel versleutelen met behulp van Microsoft sleutel Exchange sleutel

Voer een van de volgende opdrachten, afhankelijk van uw geografische regio of exemplaar van Azure:

- Voor Noord-Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Europe:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Azië:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Latijns-Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Japan:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Australië:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor [De overheid Azure](https://azure.microsoft.com/features/gov/), waarin het Amerikaanse overheid-exemplaar van Azure gebruikt:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Canada:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Duitsland:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor India:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Wanneer u deze opdracht uitvoert, gebruikt u deze instructies:

- *Contosokey* vervangen door de id die u gebruikt voor het genereren van de sleutel in **stap 3.3: Maak een nieuwe sleutel** van de stap [genereren uw sleutel](#step-3-generate-your-key) .

- *SubscriptionID* vervangen door de ID van de Azure-abonnement met de belangrijkste kluis. U hebt deze waarde opgehaald eerder, in **stap 1.2: uw Azure abonnements-ID krijgen** van de stap [voorbereiden uw werkstation internetverbinding](#step-1-prepare-your-internet-connected-workstation) .

- *ContosoFirstHSMKey* vervangen door een label die wordt gebruikt voor de naam van het uitvoerbestand.

Wanneer dit voltooid is, wordt **resultaat: SUCCES** en er wordt een nieuw bestand in de huidige map met de volgende naam: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Stap 4.4: Uw pakket belangrijke doorverbinden naar het werkstation internetverbinding kopiëren

Gebruik een USB-station of andere draagbare opslag het uitvoerbestand kopiëren van de vorige stap (KeyTransferPackage-ContosoFirstHSMkey.byok) naar uw werkstation internetverbinding.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Stap 5: Uw sleutel overbrengen naar Azure-toets kluis

Gebruik de cmdlet [Toevoegen-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) voor het uploaden van de belangrijkste doorverbinden-pakket die u hebt gekopieerd vanaf de verbroken werkstation naar de Azure-toets kluis HSM voor deze laatste stap, op het werkstation internetverbinding:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Als de upload geslaagd is, ziet u de eigenschappen van de sleutel die u zojuist hebt toegevoegd weergegeven.


##<a name="next-steps"></a>Volgende stappen

U kunt nu deze toets HSM-beveiliging in uw belangrijkste kluis. Zie voor meer informatie de sectie **Als u wilt gebruiken van een waardepapier HSM (hardwarebeveiligingsmodule)** in deze zelfstudie [aan de slag met Azure toets kluis](key-vault-get-started.md) .
