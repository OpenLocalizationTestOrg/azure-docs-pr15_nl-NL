<properties 
   pageTitle="StorSimple beveiliging | Microsoft Azure" 
   description="De beveiliging en privacy worden functies beschreven die beveiligen van uw service StorSimple, apparaat en gegevens on-premises en in de cloud." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="05/03/2016"
   ms.author="v-sharos"/>

# <a name="storsimple-security-and-data-protection"></a>StorSimple beveiliging en gegevensbescherming

## <a name="overview"></a>Overzicht

Beveiliging is een groot belang voor iedereen die is gesteld door een nieuwe technologie, met name wanneer de technologie wordt gebruikt met vertrouwelijke gegevens. Als u andere technologieën evalueren, moet u rekening houden met verbeterde risico's en -kosten voor gegevensbeveiliging. Microsoft Azure StorSimple biedt een beveiligings- en privacy-oplossing voor gegevensbescherming, helpen om ervoor te zorgen: 

- **Vertrouwelijkheid** – alleen geautoriseerde entiteiten kunnen uw gegevens. 
- **Integriteit** – alleen gemachtigde entiteiten kunt wijzigen of verwijderen van uw gegevens.

De Microsoft Azure StorSimple-oplossing bestaat uit vier hoofdonderdelen die met elkaar samenwerken:

- **StorSimple Manager-service die zijn ingesloten in een Microsoft Azure** – de management-service die u kunt configureren en het apparaat StorSimple inrichten.
- **StorSimple apparaat** – een fysiek apparaat is geïnstalleerd in uw datacenter. Alle hosts en clients die gegevens genereren verbinden met het apparaat StorSimple en het apparaat dat de gegevens worden beheerd en het bericht wordt verplaatst naar de cloud Azure uw besturingssysteem.
- **Clients/hosts verbonden met het apparaat** – de clients in de infrastructuur van uw die verbinding maken met het apparaat StorSimple en zorgt u voor gegevens die moeten worden beveiligd.
- **Cloudopslag** – de locatie in de Azure cloud waar gegevens worden opgeslagen.

De volgende secties worden de StorSimple beveiligingsfuncties die u helpen beschermen elk van deze onderdelen en de gegevens worden opgeslagen. Het bevat ook een lijst met vragen die u over Microsoft Azure StorSimple beveiliging en de bijbehorende antwoorden moet mogelijk.

## <a name="storsimple-manager-service-protection"></a>Bescherming tegen StorSimple Manager-service

De StorSimple Manager-service is een management-service ingesloten in een Microsoft Azure en gebruikt voor het beheren van alle StorSimple apparaten die uw organisatie heeft aangebracht. U kunt de StorSimple Manager-service openen met behulp van uw organisatie-referenties voor aanmelding bij de portal van Azure klassieke via een webbrowser. 

Toegang tot de service StorSimple Manager is vereist dat uw organisatie een Azure abonnement hebben dat bevat StorSimple. Uw abonnement bepaalt de functies die u in de portal van Azure klassieke openen kunt. Als uw organisatie beschikt niet over een Azure-abonnement en u wilt meer informatie over deze, raadpleegt u [zich aanmeldt voor Azure als een organisatie](../active-directory/sign-up-organization.md). 

Omdat de StorSimple Manager-service wordt gehost in Azure wordt aangegeven, is deze door de Azure beveiligingsfuncties beveiligd. Voor meer informatie over de beveiligingsfuncties van Microsoft Azure, gaat u naar het [Vertrouwenscentrum in Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>Bescherming tegen StorSimple-apparaat

De StorSimple apparaat is een on-premises implementatie hybride opslagruimte met effen staat stations (SSD) en vaste schijven (harde schijven), samen met redundante controllers en automatische failover mogelijkheden. De controllers beheren opslag trapsgewijs schakelen, het plaatsen van momenteel gebruikte (of warm) gegevens op lokale opslag (in de StorSimple apparaat of on-premises servers), terwijl minder veelgebruikte gegevens verplaatsen naar de cloud.

Alleen gemachtigde StorSimple apparaten zijn toegestaan deel te nemen aan de StorSimple Manager-service die u hebt gemaakt in uw Azure-abonnement. Als u akkoord gaat een apparaat, moet u deze met de service StorSimple Manager doordat de registersleutel voor de service registreren. De registersleutel voor de service is een 128-bit willekeurige sleutel in de portal van Azure klassieke gegenereerd. 

![Service registratiegegevens sleutel](./media/storsimple-security/ServiceRegistrationKey.png)

Voor meer informatie over hoe u een service registratie-sleutel, Ga naar [stap 2: de sleutel voor de registratie ophalen](storsimple-deployment-walkthrough.md#step-2-get-the-service-registration-key).

De registersleutel voor de service is een lange sleutel die 100 + tekens bevat. U kunt de toets kopiëren en opslaan in een tekstbestand op een veilige locatie, zodat u deze gebruiken kunt om te machtigen extra apparaten zo nodig. Als de sleutel voor de registratie verloren gaat nadat u uw eerste apparaat hebt geregistreerd, kunt u een nieuwe code genereren van de service StorSimple Manager. Dit is niet van invloed op de werking van bestaande apparaten. 

Nadat een apparaat is geregistreerd, wordt deze tokens gebruikt om te communiceren met Microsoft Azure. De registersleutel voor de service wordt niet gebruikt na apparaatregistratie.

> [AZURE.NOTE] Het is raadzaam dat u opnieuw de sleutel voor de registratie na elke gebruik genereren.

## <a name="protect-your-storsimple-solution-via-passwords"></a>Uw oplossing StorSimple via wachtwoorden beveiligen

Wachtwoorden zijn een belangrijk onderdeel van uw computerbeveiliging en grote schaal in de oplossing StorSimple worden gebruikt om ervoor te zorgen dat uw gegevens voor alleen geautoriseerde gebruikers toegankelijk is. StorSimple kunt u de volgende wachtwoorden configureren:

- Beheerderswachtwoord StorSimple-apparaat
- Uitdaging CHAP Handshake Authentication Protocol () begin- en doellijst wachtwoorden
- StorSimple momentopname Manager-wachtwoord

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShell voor StorSimple en het beheerderswachtwoord voor StorSimple-apparaat

Windows PowerShell voor StorSimple is een opdrachtregel-interface die u gebruiken kunt voor het beheren van het apparaat StorSimple. Windows PowerShell voor StorSimple bevat functies waarmee u kunnen u uw apparaat registreren, het netwerkinterface configureren op uw apparaat, bepaalde soorten updates installeren, problemen met uw apparaat via de ondersteuningssessie en de status van het apparaat te wijzigen. Verbinding maken met de seriële console op het apparaat of via Windows PowerShell externe, kunt u Windows PowerShell voor StorSimple openen.

De externe PowerShell kan plaatsvinden via HTTPS of HTTP. Als u extern beheer via HTTPS is ingeschakeld, moet u het certificaat extern beheer van het apparaat downloaden en installeren op de externe client. Ga naar de [verbinding maken met uw apparaat StorSimple](storsimple-remote-connect.md)voor meer informatie over externe PowerShell.

Nadat u Windows PowerShell voor StorSimple verbinding maken met het apparaat gebruikt, moet u het wachtwoord van de beheerder apparaat aan te melden bij het apparaat te geven.

![Beheerderswachtwoord apparaat](./media/storsimple-security/DeviceAdminPW.png)

Houd rekening met de volgende aanbevolen procedures:

- Extern beheer is standaard uitgeschakeld. U kunt de StorSimple Manager-service te kunnen gebruiken. Als veiligheidsoverwegingen, externe toegang moet zijn ingeschakeld alleen tijdens de periode dat het daadwerkelijk is vereist.
- Als u het wachtwoord wijzigt, moet u alle externe gebruikers melden zodat ze niet een verlies onverwachte connectivity ondervinden.
- Bestaande wachtwoorden kan niet worden opgehaald door de service StorSimple Manager: deze ze alleen kunt herstellen. Het is raadzaam dat u alle wachtwoorden opslaan op een veilige plaats, zodat er geen een wachtwoord opnieuw instellen als deze is vergeten. Als u een wachtwoord opnieuw instellen moet, moet u alle gebruikers een melding voordat u het opnieuw instellen. 

U kunt de Windows PowerShell-interface openen met behulp van een seriële verbinding met het apparaat. U kunt ook toegang tot deze op afstand via HTTP of HTTPS, die extra beveiliging bieden. HTTPS biedt een hoger niveau van beveiliging dan een serie- of HTTP-verbinding. Echter als wilt HTTPS gebruiken, moet u eerst een certificaat installeren op de clientcomputer die toegang het apparaat tot. U kunt het certificaat RAS downloaden van de pagina van de configuratie van apparaten in de service StorSimple Manager. Als het certificaat voor externe toegang verbroken is, moet u een nieuw certificaat downloaden en deze doorgeven aan alle clients die zijn geautoriseerd gebruik van extern beheer.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Uitdaging CHAP Handshake Authentication Protocol () begin- en doellijst wachtwoorden

CHAP is een schema voor verificatie van het apparaat StorSimple gebruikt om te valideren de identiteit van externe clients. De verificatie is gebaseerd op een gedeelde wachtwoord. CHAP kan zijn in één richting (één richting) of onderlinge (bidirectionele). Met één richting CHAP, wordt het doel (het apparaat StorSimple) geverifieerd een initiator (host). Onderlinge of omgekeerde CHAP vereist dat het doel verifiëren het beginpunt en vervolgens het beginpunt het doel te verifiëren. Uw StorSimple kan worden geconfigureerd voor gebruik van een methode.

Als u CHAP configureert, worden op de hoogte van de volgende handelingen uit:

- De naam van de gebruiker CHAP moet minder dan 233 tekens bevatten.
- Het wachtwoord CHAP moet liggen tussen 12 en 16 tekens bevatten. U probeert te gebruiken van een langere gebruikersnaam of wachtwoord zijn ingevoegd om een verificatie mislukt op de Windows-host.
- U kunt hetzelfde wachtwoord niet gebruiken voor zowel het beginpunt CHAP als het doel CHAP.
- Wanneer u het wachtwoord instelt, kunnen worden gewijzigd, maar kan niet worden opgehaald. Als het wachtwoord wordt gewijzigd, moet u alle externe gebruikers een melding dat ze kunnen wel verbinden met het apparaat StorSimple.

Ga naar [Configureren voor uw apparaat StorSimple CHAP](storsimple-configure-chap.md)voor meer informatie over CHAP en hoe u configureert u deze voor uw oplossing StorSimple.

### <a name="storsimple-snapshot-manager-password"></a>StorSimple momentopname Manager-wachtwoord

StorSimple momentopname Manager is een Microsoft Management Console (MMC)-module waarin volume-groepen en Windows Volume schaduw kopie voor het genereren van toepassing-consistente back-ups wordt gebruikt. Bovendien kunt u StorSimple momentopname Manager maken van back-planningen en klonen of volumes herstellen.

Als u een apparaat met StorSimple momentopname Manager configureert, moet u het wachtwoord StorSimple momentopname Manager te geven. Dit wachtwoord is eerst instellen in Windows PowerShell voor StorSimple tijdens de registratie. Het wachtwoord kan ook worden ingesteld en gewijzigd van de service StorSimple Manager. Dit wachtwoord wordt geverifieerd door het apparaat met StorSimple momentopname Manager.

![StorSimple momentopname Manager-wachtwoord](./media/storsimple-security/SnapshotMgrPassword.png)

Het wachtwoord StorSimple momentopname Manager moet 14 tot 15 tekens en moet 3 of meer van een combinatie van hoofdletters, kleine letters, cijfers en speciale tekens bevatten. Nadat u het wachtwoord StorSimple momentopname Manager instelt, kunnen worden gewijzigd, maar kan niet worden opgehaald. Als u het wachtwoord wijzigt, moet u alle externe gebruikers een melding.

Voor meer informatie over StorSimple momentopname Manager, gaat u naar [Wat is StorSimple momentopname Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Aanbevolen werkwijzen voor wachtwoorden

We raden u aan dat u de volgende richtlijnen gebruiken om ervoor te zorgen dat StorSimple wachtwoorden sterke en goed beschermde zijn:

- Uw wachtwoord wijzigen om de drie maanden. Wijzigen van de wachtwoorden wordt jaarlijks afgedwongen.
- Gebruik sterke wachtwoorden. Ga voor meer informatie naar [sterke wachtwoorden maken en ze te beveiligen](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
- Gebruik altijd verschillende wachtwoorden voor verschillende access mogelijkheden; elk van de wachtwoorden die u opgeeft moet uniek zijn.
- Wachtwoorden niet delen met iedereen die is niet geautoriseerd voor toegang tot het StorSimple-apparaat.
- Geen uitspreken over een wachtwoord vóór anderen en aanwijzing aan de opmaak van een wachtwoord.
- Als u vermoedt dat een account of wachtwoord kent, melden bij de uw beveiligingsafdeling.
- Alle wachtwoorden gevoelige, vertrouwelijke informatie behandeld. 

## <a name="storsimple-data-protection"></a>Gegevensbescherming StorSimple

Dit onderwerp vindt de beveiligingsfuncties StorSimple die gegevens tijdens overdracht en opgeslagen gegevens beveiligen.

Zoals is beschreven in andere secties, worden wachtwoorden toe te staan en gebruikers worden geverifieerd voordat ze toegang hebben tot uw oplossing StorSimple gebruikt. Een andere beveiliging is gegevens beveiligen tegen onbevoegde gebruikers terwijl deze wordt overgebracht tussen opslagsystemen en terwijl deze worden opgeslagen. De volgende secties worden de functies voor gegevensbeveiliging StorSimple voorzien.

> [AZURE.NOTE] Deduplication zorgt voor extra beveiliging voor gegevens die zijn opgeslagen op het apparaat StorSimple en in Microsoft Azure-opslag. Wanneer u gegevens is deduplicated, de gegevensobjecten afzonderlijk worden opgeslagen in de metagegevens die worden gebruikt om te wijzen en deze openen: Er is geen beschikbaar opslagniveau context de gegevens op basis van de volumestructuur, bestandssysteem of de bestandsnaam van het opnieuw.

## <a name="protect-data-flowing-through-the-service"></a>Gegevens die doorloopt tot en met de service beschermen

Het primaire doel van de StorSimple Manager-service is om te beheren en configureren van het apparaat StorSimple. De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure. Gebruikt u de portal van Azure klassieke apparaat configuratiegegevens in te voeren, en klikt u vervolgens de StorSimple Manager-service in Microsoft Azure wordt gebruikt de gegevens te verzenden naar het apparaat. Een systeem van paren van asymmetrische sleutels StorSimple gebruikt om ervoor te zorgen dat een inbreuk op de Azure-service niet in een inbreuk op opgeslagen gegevens resulteert. 

![Gegevenscodering in flight](./media/storsimple-security/DataEncryption.png)

Het asymmetrische belangrijke systeem helpt bij de bescherming van de gegevens die tot en met de service als volgt doorloopt:

1. Een versleutelingscertificaat gegevens met een asymmetrische openbare en persoonlijke sleutel koppelen op het apparaat wordt gegenereerd en worden gebruikt om de gegevens te beveiligen. De gebruikte toetsen worden gegenereerd wanneer het eerste apparaat is geregistreerd. 
2. De gegevens versleuteling certificaat toetsen worden geëxporteerd naar een Personal Information Exchange (.pfx)-bestand dat door de service gegevens versleuteling sleutel, dat wil zeggen een sterke 128-bit sleutel dat willekeurig wordt gegenereerd door het eerste apparaat tijdens de registratie is beveiligd.
3. De openbare sleutel van het certificaat voor het veilig beschikbaar voor de StorSimple Manager-service is gemaakt en de persoonlijke sleutel blijft bij het apparaat.
4. Gegevens invoeren van de service is versleuteld met de openbare sleutel en ontsleuteld met de persoonlijke sleutel die is opgeslagen op het apparaat, ervoor te zorgen dat de Azure-service de gegevens die doorloopt naar het apparaat niet ontsleutelen.

De service gegevens versleutelingssleutel wordt gegenereerd op alleen het eerste apparaat geregistreerd met de service. Alle volgende apparaten die zijn geregistreerd met de service moeten dezelfde service gegevens versleutelingssleutel gebruiken. 

> [AZURE.IMPORTANT] 
> 
> Het is belangrijk dat u een kopie van de sleutel gegevens versleuteling en sla deze op een veilige locatie. Een kopie van de sleutel gegevens versleuteling mogen zodanig dat deze kan worden geopend door een gemachtigde persoon en eenvoudig kan worden doorgegeven aan de apparaatbeheerder van het zijn opgeslagen.
>
> Als de sleutel gegevens service verbroken is, kunt een Microsoft-ondersteuningsmedewerker u om op te halen mits u ten minste één apparaat online-status hebt. Het is raadzaam dat u de service gegevens versleutelingssleutel wijzigen nadat deze zijn opgehaald. Ga naar [de service gegevens versleutelingssleutel wijzigen](storsimple-service-dashboard.md#change-the-service-data-encryption-key)voor instructies.

U kunt de sleutel service data versleuteling en de bijbehorende gegevens versleutelingscertificaat wijzigen door de optie **service gegevens versleutelingssleutel wijzigen** op het servicedashboard. Om ervoor te zorgen dat de beveiliging van gegevens niet is beschadigd, moet u een fysiek StorSimple-apparaat gebruiken om te wijzigen van de service gegevenssleutel. De toetsen versleuteling wijzigen, moet alle apparaten worden bijgewerkt met de nieuwe sleutel. Daarom, is het raadzaam dat u de sleutel wijzigen wanneer alle apparaten online bent. Als er apparaten zijn offline, kunnen hun toetsen op een ander tijdstip worden gewijzigd. De apparaten met verouderde toetsen nog steeds niet kan worden uitgevoerd back-ups, maar niet mogelijk om gegevens te herstellen totdat de sleutel wordt bijgewerkt. Ga naar [het servicedashboard van de StorSimple Manager gebruiken](storsimple-service-dashboard.md)voor meer informatie.

De sleutel service data versleuteling en het versleutelingscertificaat van de gegevens verlopen niet. Echter, is het raadzaam dat u de sleutel service data versleuteling jaarlijks om te voorkomen dat inbreuk op sleutel wijzigen.

## <a name="protect-data-at-rest"></a>Gegevens in rust beveiligen

Gegevens van het apparaat StorSimple worden beheerd door op te slaan in lagen lokaal en in de cloud, afhankelijk van de frequentie in gebruik. Gegevens verstuurd alle host machines die met het apparaat verbonden zijn naar het apparaat, dat wordt verplaatst van gegevens naar de cloud, zo nodig. Gegevens worden overgebracht van het apparaat in de cloud veilig via Internet. Elk apparaat heeft een iSCSI-doel waarmee alle gedeelde volumes voor dat apparaat. Alle gegevens worden gecodeerd voordat deze wordt verzonden naar cloud opslag. 

![Cloud opslag versleuteling-toets](./media/storsimple-security/CloudStorageEncryption.png)

Om te garanderen de veiligheid en de integriteit van gegevens die zijn verplaatst naar de cloud, kunt StorSimple u cloud opslag versleuteling toetsen als volgt definiëren:

- U kunt de cloud opslag versleuteling-sleutel opgeven wanneer u een container volume maakt. De toets kan niet worden gewijzigd of later toegevoegd. 
- Enkel volume van een container volume delen dezelfde versleutelingssleutel. Als u een ander formulier van versleuteling voor een specifieke volume wilt, wordt u aangeraden dat u een nieuwe volume container als host voor dat volume maken.
- Wanneer u de cloud opslag sleutel in de service StorSimple Manager invoert, wordt de sleutel is versleuteld met de openbare gedeelte van de sleutel gegevens versleuteling en vervolgens naar het apparaat is verzonden.
- De cloud opslag sleutel niet is opgeslagen, een willekeurige plaats in de service en is alleen bekend is bij het apparaat.
- Cloud opslag versleuteling sleutel is optioneel. U kunt gegevens die bij de host bij het apparaat is versleuteld verzenden.

### <a name="additional-security-best-practices"></a>Aanbevolen procedures voor extra beveiliging

- Verkeer splitsen: uw iSCSI SAN van gebruiker-verkeer is toegestaan in een zakelijke LAN isoleren door de implementatie van een netwerk met een volledig zijn gescheiden en het gebruik van VLAN's waar fysiek moeten worden geïsoleerd is niet een optie. Een speciale netwerk voor iSCSI-opslag wordt de veiligheid en prestaties van uw cruciale gegevens garanderen. Opslag en verkeer via een zakelijke LAN mengen kunt wordt niet aanbevolen en latentie vergroten en netwerkfouten veroorzaken.

- Voor de netwerkbeveiliging van de host aan de clientzijde, netwerkinterfaces die ondersteuning bieden voor TCP/IP Offload Engine (TOE) te gebruiken. CPU-belasting Hiermee reduceert u TOE door de verwerking van TCP op de netwerkadapter.

## <a name="protect-data-via-storage-accounts"></a>Beschermen van gegevens via opslag-accounts

Elke Microsoft Azure-abonnement kunt een of meer opslagruimte-accounts maken. (Een account opslagruimte biedt een unieke naamruimte voor het werken met gegevens die zijn opgeslagen in de cloud Azure.) Toegang tot een opslag-account wordt bepaald door de toets met het abonnement en toegang die is gekoppeld aan dat account opslag. 

Wanneer u een account opslag maakt, genereert Microsoft Azure twee 512 bits opslag-toegangstoetsen, één voor verificatie wordt gebruikt wanneer het apparaat StorSimple toegang heeft tot het opslag-account. Houd er rekening mee dat slechts één van deze toetsen gebruikt wordt. De andere toets houdt in reserveren, zodat u kunt de toetsen regelmatig draaien. Als u wilt draaien toetsen, kunt u de secundaire sleutel actief maken, en verwijder vervolgens de primaire sleutel. Vervolgens kunt u een nieuwe sleutel voor gebruik tijdens de volgende draaiing maken. (Vanwege de beveiliging vereisen veel datacenters belangrijke draaiing.) 

We raden u aan deze aanbevolen procedures voor belangrijke draaiing:

- U moet opslag account toetsen regelmatig om ervoor te zorgen dat uw account opslag niet wordt geopend door onbevoegde gebruikers draaien.
- De beheerder van uw Azure moet regelmatig, wijzigen of de primaire of secundaire sleutel opnieuw te genereren met behulp van de sectie opslag van de Azure klassieke portal direct toegang tot het opslag-account.


## <a name="protect-data-via-encryption"></a>Gegevens via versleuteling beschermen

StorSimple gebruikt de volgende versleutelingsalgoritmen om gegevens die zijn opgeslagen in te beveiligen of reizen tussen de onderdelen van uw oplossing StorSimple.

| Algoritme | Lengte van de sleutel | Protocollen/applications/opmerkingen |
| --------- | ---------- | ------------------------------- |
| RSA       | 2048       | RSA PKCS 1 versie 1.5 wordt gebruikt door de Azure klassieke portal versleutelen configuratiegegevens die wordt verzonden naar het apparaat: bijvoorbeeld opslag domeinaccountreferenties, StorSimple apparaatconfiguratie, en cloud opslag versleuteling toetsen. |
| AES       | 256        | AES met CBC wordt gebruikt voor het coderen van de openbare gedeelte van de sleutel gegevens versleuteling voordat deze wordt verzonden naar de Azure klassieke portal van het apparaat StorSimple. Het is ook door het apparaat StorSimple gebruikt om gegevens te coderen voordat de gegevens naar de cloud opslag-account wordt verzonden. |


## <a name="storsimple-virtual-device-security"></a>StorSimple virtueel apparaat beveiliging

[AZURE.INCLUDE [storsimple virtual device security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen (FAQ)

Hier volgen enkele vragen en antwoorden over beveiligings- en Microsoft Azure StorSimple.

**Q:** Mijn service is is gehackt. Wat moeten mijn Vervolgstappen?

**A:** Direct moet u de sleutel service data versleuteling en de opslag-account te gebruiken voor de opslag-account dat wordt gebruikt voor trapsgewijs gegevens wijzigen. Voor instructies, gaat u naar: 

- [De service gegevens versleutelingssleutel wijzigen](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Belangrijke draaiing van opslag-accounts](storsimple-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Q:** Ik heb een nieuw StorSimple apparaat die om de service registratie-toets vraagt. Hoe krijg ik het terug?

**A:** Deze sleutel is gemaakt wanneer u de StorSimple Manager-service voor het eerst hebt gemaakt. Wanneer u de service StorSimple Manager verbinding maken met het apparaat gebruikt, kunt u de servicepagina aan de slag om te bekijken of de service registratie-sleutel opnieuw genereren. Een nieuwe service registratie-code genereren, heeft dit geen invloed op de bestaande geregistreerde apparaten. Voor instructies, gaat u naar:

- [Bekijken of te genereren van de service registratie-toets](storsimple-service-dashboard.md#view-or-regenerate-the-service-registration-key)

**Q:** Ik ben mijn service gegevens versleuteling kwijt. Wat moet ik doen?

**A:** Neem contact op met Microsoft ondersteuning. Ze zich kunnen aanmelden bij een ondersteuningssessie op uw apparaat en help u de sleutel ophalen (mits voor ten minste één apparaat online is). Direct nadat u de service gegevenssleutel aanvragen, moet u deze om ervoor te zorgen dat de nieuwe sleutel bekend is alleen voor u wijzigen. Voor instructies, gaat u naar:

- [De service gegevens versleutelingssleutel wijzigen](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Q:**  Ik een apparaat voor een service gegevens versleuteling belangrijke wijziging geautoriseerd, maar is het proces voor belangrijke niet gestart. Wat moet ik doen?

**A:** Als de periode time-out is verlopen, moet u naar het apparaat voor de service gegevens versleuteling belangrijke wijziging autoriseren en start het opnieuw.

**Q:**  Ik heb de sleutel service gegevens hebt gewijzigd, maar ik heb niet bijwerken van de andere apparaten binnen 4 uur. Moet ik hebben om opnieuw te beginnen?

**A:** De periode van 4 uur geldt alleen voor de wijziging wordt gestart. Nadat u het updateproces op geautoriseerde StorSimple apparaat gestart, is de autorisatie geldig totdat alle apparaten worden bijgewerkt.

**Q:** De beheerder van onze StorSimple heeft het bedrijf verlaten. Wat moet ik doen?

**A:** Wijzigen en de wachtwoorden opnieuw instellen dat toegang geeft tot het apparaat StorSimple en de sleutel service gegevens versleuteling om ervoor te zorgen dat de nieuwe gegevens niet bekend is niet-geautoriseerd personeel wijzigen. Voor instructies, gaat u naar:

- [Gebruik van de service StorSimple Manager storsimple wachtwoorden wijzigen](storsimple-change-passwords.md)
- [De service gegevens versleutelingssleutel wijzigen](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [CHAP configureren voor uw apparaat StorSimple](storsimple-configure-chap.md)

**Q:** Ik wil het wachtwoord StorSimple momentopname Manager bieden aan een host die verbinding met het apparaat StorSimple maakt, maar het wachtwoord is niet beschikbaar. Wat kan ik doen?

**A:** Als u het wachtwoord vergeten bent, moet u een nieuwe id maken. Vervolgens moet u alle bestaande gebruikers informeren dat het wachtwoord heeft gewijzigd en dat ze hun clients als u wilt gebruiken het nieuwe wachtwoord moeten bijwerken. Voor instructies, gaat u naar:

- [Het wachtwoord StorSimple momentopname Manager wijzigen](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)
- [Een apparaat verifiëren](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Q:** Het certificaat voor externe toegang tot de Windows PowerShell voor StorSimple is op het apparaat gewijzigd. Hoe werk ik mijn RAS-clients?

**A:** U kunt downloaden van het nieuwe certificaat van de service StorSimple Manager en geef deze in het archief van uw RAS-clients zijn geïnstalleerd. Voor instructies, gaat u naar:

- [Cmdlet certificaat importeren](https://technet.microsoft.com/library/hh848630.aspx)

**Q:** Mijn gegevens beveiligd als de StorSimple Manager is de service is gehackt is?

**A:** Gegevensverbinding voor webservices configuratie is altijd versleuteld met uw openbare sleutel wanneer u deze in een webbrowser bekijken. Omdat de service heeft geen toegang tot de persoonlijke sleutel, is de service niet mogelijk om alle gegevens weer te geven. Als de StorSimple Manager-service is is gehackt, is er geen effect, omdat er geen sleutels die zijn opgeslagen in de StorSimple Manager-service.

**Q:** Als iemand toegang tot het versleutelingscertificaat van gegevens krijgt, wordt mijn gegevens worden is gehackt?

**A:** Microsoft Azure slaat van de klant gegevens versleutelingssleutel (.pfx-bestand) in een versleutelde indeling. Omdat het .pfx-bestand is versleuteld en de service StorSimple de sleutel service data versleuteling het .pfx-bestand te decoderen niet hebt, wordt er bij het gewoon aan de toegang tot de .pfx-bestand niet geen geheimen weergegeven.

**Q:** Wat gebeurt er als u een overheidsinstellingen wordt gevraagd van Microsoft voor mijn gegevens?

**A:** Omdat alle gegevens over de service is versleuteld en de persoonlijke sleutel bij het apparaat blijft, moet de overheidsinstellingen de klant vragen om de gegevens. 

## <a name="next-steps"></a>Volgende stappen

[Uw apparaat StorSimple Deploy](storsimple-deployment-walkthrough.md).
 
