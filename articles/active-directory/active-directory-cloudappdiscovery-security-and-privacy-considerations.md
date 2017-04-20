<properties
    pageTitle="App-Discovery-beveiliging en Privacy-overwegingen cloud | Microsoft Azure"
    description="In dit onderwerp worden de beveiliging en privacy overwegingen die betrekking hebben op Cloud-App Discovery."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud-App Discovery beveiliging en Privacy-overwegingen

Microsoft is erop gebrand om uw privacy te beschermen en beveiligen van uw gegevens tijdens het uitvoeren van software en services die u helpen de beveiliging van uw organisatie beheren. <br>
We herkennen dat wanneer u uw gegevens naar anderen, belasten dat vertrouwen is vereist strikte beveiliging technische investeringen en expertise back.
Microsoft voldoet aan de strikte naleving en richtlijnen voor beveiliging van secure software development levenscyclus procedures voor het besturingssysteem van een service. <br>
Is de hoogste prioriteit bij Microsoft beveiligen en beschermen van gegevens.

Dit onderwerp wordt uitgelegd hoe gegevens is die worden verzameld, verwerkt en beveiligd binnen Azure Active Directory Cloud-App Discovery




##<a name="overview"></a>Overzicht

Cloud-App Discovery is een functie van Azure AD en wordt gehost in Microsoft Azure. <br>
De Cloud-App Discovery-eindpunt-agent wordt gebruikt om de toepassing discovery gegevens verzamelen van beheerde IT machines. <br>
De verzamelde gegevens veilig via een versleuteld kanaal naar verzonden de Azure AD Cloud-App Discovery-service. <br>
De Cloud-App Discovery-gegevens voor een organisatie is vervolgens zichtbaar in de portal van Azure. <br>


<center>![De werking van Cloud-App Discovery](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png)</center> <br>


De volgende secties volgt u de stroom van gegevens en wordt beschreven hoe deze is beveiligd, zoals het naar de Cloud-App Discovery-service wordt verplaatst van uw organisatie en uiteindelijk naar de Cloud-App Discovery-portal.



## <a name="collecting-data-from-your-organization"></a>Het verzamelen van gegevens uit uw organisatie

Functie wilt gebruiken Azure Active Directory Cloud-App discovery om inzicht te krijgen in de toepassingen die door andere werknemers in uw organisatie wordt gebruikt, moet u eerst de Azure AD Cloud-App Discovery-eindpunt-agent implementeren naar computers binnen uw organisatie.

Beheerders van de Azure Active Directory-tenant (of hun gemachtigde) kunnen u het installatiepakket agent downloaden van de Azure-portal. De agent kan worden handmatig geïnstalleerd of geïnstalleerd op meerdere computers in de organisatie met SCCM of Groepsbeleid.

Zie de [Implementatiehandleiding van Cloud App Discovery Groepsbeleid](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)voor verdere instructies op implementatieopties voor de.
<br>

### <a name="data-collected-by-the-agent"></a>Gegevens die worden verzameld door de agent

De gegevens die worden beschreven in de onderstaande lijst worden verzameld door de agent wanneer een verbinding wordt gemaakt met een webtoepassing. De gegevens worden alleen voor deze toepassingen die de beheerder heeft geconfigureerd voor discovery verzameld. <br>
U kunt de lijst met de cloud-apps die de agent via het Cloud-App Discovery-blad in de Microsoft [Azure-portal](https://portal.azure.com/), klik onder **Instellingen bewaakt**bewerken->**Gegevensverzameling**->**lijst met collecties van de App**. Zie voor meer informatie [Aan de slag met Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
<br>
**Informatie-categorie**: gebruikersgegevens <br>
**Beschrijving**: <br>
De naam van de Windows-gebruiker van het proces dat een verzoek om naar de doelsite webtoepassing aangebracht (bijvoorbeeld: DOMEIN\gebruikersnaam) en de Windows beveiliging id (beveiligings-id) van de gebruiker.


**Categorie van gegevens**: de verwerking van gegevens <br>
**Beschrijving**: <br>
De naam van het proces dat het verzoek in de Web-doeltoepassing aangebracht (bijvoorbeeld: "iexplore.exe")

**Informatie-categorie**: informatie van de computer <br>
**Beschrijving**: <br>
De NetBIOS computernaam waarop de-agent is geïnstalleerd.

**Informatie-categorie**: App-verkeer is toegestaan informatie <br>
**Beschrijving**: <br>

De volgende verbindingsgegevens:

- De bron (lokale computer) en bestemming IP-adressen en poortnummers

- Het openbare IP-adres van de organisatie waarmee het verzoek uitvalt.

- De tijd van de aanvraag kunt invullen

- Het volume van verkeer verzonden en ontvangen

- Het IP-versie (4 of 6)

- Voor alleen TLS-verbindingen: de naam van het doel-host van de extensie Server naam vermelding of certificaat van de server.

De volgende HTTP-informatie:

- Methode (ophalen, bericht, enz.)

- Protocol (HTTP/1.1, enz.)

- Tekenreeks gebruikersagent

- Hostname

- TARGET URI (met uitzondering van query-tekenreeks)

- Informatie over inhoudstype

- Referrer URL-gegevens (exclusief queryreeks)



> [AZURE.NOTE] De bovenstaande HTTP-gegevens worden verzameld voor alle niet-versleutelde verbindingen.
Deze informatie wordt alleen voor TLS-verbindingen opgenomen wanneer de instelling "Uitgebreide controle" is ingeschakeld in de portal. De instelling is '' standaard ingeschakeld.
Voor meer details, hieronder en [Aan de slag met Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


Naast de gegevens die de agent informatie over de netwerkactiviteit verzamelt, ook worden verzameld anonieme gegevens over de software en hardwareconfiguratie, foutenrapporten en informatie over hoe de agent wordt gebruikt.

<br><br>
### <a name="how-the-agent-works"></a>De werking van de agent

De installatie agent bevat twee onderdelen:

- Een gebruiker-onderdeel

- Onderdeel van een kernelmodus-stuurprogramma (Windows Filtering Platform stuurprogramma)



Wanneer de agent voor het eerst wordt geïnstalleerd wordt een vertrouwd certificaat van computer opgeslagen op de computer die vervolgens wordt tot stand brengen van een beveiligde verbinding met de Cloud-App Discovery-service. <br>
Configuratie van beleid de agent regelmatig opgehaald uit de Cloud-App Discovery-service via deze beveiligde verbinding. <br>
Het beleid bevat informatie over welke cloud-toepassingen te controleren of Automatische updates moet worden ingeschakeld, onder andere.

Als de Web-verkeer is verzonden en ontvangen op de computer van Internet Explorer en Chrome, de Cloud-App Discovery-agent analyseert van het verkeer en haalt de relevante metagegevens (Zie de sectie **gegevens die worden verzameld door de agent** hierboven). <br>
Elke minuut, uploadt de agent de verzamelde metagegevens naar de Cloud-App Discovery-service via een versleuteld kanaal.

Het stuurprogrammaonderdeel het versleutelde verkeer onderschept en voegt zelf in de versleutelde stream. Meer details in de sectie **Intercepting gegevens uit versleutelde verbindingen (uitgebreide controle)** hieronder.


### <a name="respecting-user-privacy"></a>Privacy gebruiker respecteren

Ons doel is om te creëren beheerders van de hulpmiddelen om in te stellen van de verhouding tussen gedetailleerde beeldverwerkingstoepassingen in de toepassing gebruik en gebruiker privacy afhankelijk van hun organisatie. Daartoe bieden wij de volgende knoppen op de instellingenpagina in de Portal:

- **Gegevens verzamelen**: beheerders kunnen kiezen om welke toepassingen of toepassingscategorieën ze discovery gegevens ophalen willen op te geven.

- **Uitgebreide controle**: beheerders kunt kiezen om op te geven als de agent verzamelt HTTP-verkeer is toegestaan voor SSL/TLS-verbindingen (of **"uitgebreide controle"**). Meer informatie over dit in het volgende gedeelte.

- **Opties voor toestemming**: beheerders kunnen de Cloud-App Discovery-portal gebruiken om te kiezen of u wilt gebruikers van het verzamelen van gegevens door de agent een melding en of u wilt dat gebruiker toestemming voordat de agent wordt gestart verzamelen van gebruikersgegevens.

De Cloud-App Discovery eindpunt-agent verzamelt alleen de gegevens die worden beschreven in de sectie **gegevens die worden verzameld door de agent** hierboven.


### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Onderscheppen van gegevens uit versleutelde verbindingen (uitgebreide controle)
Zoals eerder vermeld, kunnen beheerders de agent om de gegevens uit versleutelde verbindingen ("uitgebreide controle") te configureren. TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) is een van de meest voorkomende protocollen gebruikt op Internet vandaag. Door te coderen communicatie met TLS, kunt een client een beveiligde, persoonlijke communicatiekanaal met een webserver; maken TLS biedt essentiële beveiliging voor het doorgeven van verificatiereferenties en voorkomen dat de openbaarmaking van gevoelige informatie.

Terwijl de end-to-end secure versleuteld kanaal verstrekt door TLS belangrijke beveiligings- en privacy kunt, wordt het protocol vaak misbruik voor schadelijke of slechte doeleinden. Veel zo is, in feite die TLS vaak heet het protocol"firewall-overslaan universele". De hoofdmap van het probleem is dat de meeste firewalls kan niet controleren TLS-communicatie omdat de toepassingslaag gegevens worden versleuteld met SSL. U weet dit, hackers vaak gebruikmaken van TLS om schadelijke nettoladingen aan een gebruiker zeker dat zelfs in de meest intelligente toepassingslaag firewalls volledig TLS blinden en TLS communicatie tussen hosts mag alleen worden doorgestuurd. Eindgebruikers gebruikmaken vaak TLS als u wilt overslaan toegang tot besturingselementen voor afgedwongen door hun firewall en proxyservers te gebruiken om te verbinden naar openbare proxy's en voor niet-TLS tunnelingprotocollen via de firewall die mogelijk anders worden geblokkeerd door beleid.

Uitgebreide controle kunt de Cloud-App Discovery-agent fungeert als een vertrouwde man-in-het-midden. Wanneer een client wordt aangevraagd voor toegang tot een beveiligde HTTPS-resource wordt het stuurprogramma eindpunt Agent onderschept de verbinding en tot stand brengt dat een nieuwe verbinding met de doelserver naar haalt de SSL-certificaat namens de klant. De agent controleert dat het certificaat kan worden vertrouwd (door te schakelen dat deze niet is ingetrokken en uitvoeren van andere controles certificaat) en, als deze keer, klikt u vervolgens de eindpunt-Agent opgehaald van de gegevens uit het servercertificaat Hiermee maakt u een eigen servercertificaat--bekend als een certificaat onderschept--die gegevens te gebruiken. Het certificaat onderschept is ondertekend op het moment door de agent eindpunt met een certificaat hoofdmap dat in de Windows store voor vertrouwd certificaat is geïnstalleerd. Dit certificaat zelfondertekend hoofdmap is gemarkeerd als niet-geëxporteerd en is ACL moest beheerders. Dit is bedoeld om u te laat nooit achter de computer waarop deze is gemaakt. Wanneer de eindgebruiker clienttoepassing het certificaat onderschept ontvangt, wordt deze deze vertrouwen omdat deze de keten helemaal naar de hoofdsite certificaat succes kunt valideren. Dit proces is voornamelijk doorzichtig maken vanuit een eindgebruiker oogpunt met enkele beperkingen, zoals hieronder beschreven.

Doordat uitgebreide controle, kan de Cloud-App Discovery eindpunt Agent ontsleutelen en controleren TLS versleuteld communicatie toestaan de service Ruis reduceren en opgeven inzichten over het gebruik van de versleutelde cloud-apps.

#### <a name="a-word-of-caution"></a>Waarschuwing
Voordat u uitgebreide controle inschakelt, is het raadzaam dat u uw bedoelingen aan uw legal en HR afdelingen communiceren en hun toestemming verkrijgen. Controleren van de eindgebruiker privé gecodeerde communicatie, kan een gevoelige onderwerp, voor de hand zijn. Controleer of de beveiliging van uw bedrijf voordat een productie album-out van uitgebreide controle, en gebruiksregels beleidsregels zijn bijgewerkt om aan te geven dat versleutelde communicatie worden gecontroleerd. Gebruikersmelding en uitzondering kan worden geacht gevoelige sites (bijvoorbeeld Bank en medische sites) kunnen ook worden nodig als u zo configureert Cloud-App Discovery om de ze te houden. Bovengenoemde, kunnen beheerders kunnen de Cloud-App Discovery-portal gebruiken om te kiezen of u wilt gebruikers van het verzamelen van gegevens door de agent een melding en of u om toestemming van de gebruiker bij de agent wordt gestart verzamelen van gebruikersgegevens.

### <a name="known-issues-and-drawbacks"></a>Bekende problemen en nadelen
Er zijn een paar zaken waar TLS onderschept invloed kan hebben op de eindgebruikerservaring:

- Uitgebreide validatie (VW) certificaten weergave van de adresbalk van de webbrowser groene fungeert als een visuele aanwijzing dat u een vertrouwde website bezoekt. TLS controle kan geen VW dupliceren in het certificaat dat deze naar de client, problemen, zodat websites met EV-certificaten normaal werken, maar de adresbalk groen niet worden weergegeven.  

- Openbare sleutel losmaken (ook bekend als het certificaat losmaken) zijn ontworpen om u te helpen voorkomen dat gebruikers man in het midden-aanvallen en rogue certificeringsinstanties. Wanneer het certificaat hoofdmap voor een vastgemaakte site niet overeenkomen met een van de bekende goede Certificeringsinstantie, wordt in de browser de verbinding met een fout weigert. Aangezien TLS onderschept in feite een man-in-het-midden is, mislukt deze verbindingen.

- Als gebruikers op het pictogram voor geblokkeerde in de browser adres balk browser om te controleren van de gegevens van de site, zien zij niet een ketting die eindigt met de certificeringsinstantie gebruikt voor het ondertekenen van het websitecertificaat, maar in plaats daarvan een certificaat ketting dat eindigt met de Windows store certificaat vertrouwde.

Als u wilt verkleinen van de exemplaren van deze problemen, we bijhouden van cloudservices en clienttoepassingen bekende gebruik uitgebreide validatie of losmaken van openbare sleutel en opdracht geven de eindpunt-Agent om te voorkomen onderscheppen risico verbindingen. Zelfs in dat geval echter, ontvangt u nog steeds rapporten van het gebruik van deze cloud-apps en het volume van de gegevens die worden overgebracht, maar omdat ze niet uitgebreide gecontroleerd, geen informatie over hoe de apps zijn gebruikt is beschikbaar.


## <a name="sending-data-to-cloud-app-discovery"></a>Gegevens verzenden naar Cloud-App Discovery

Zodra de metagegevens zijn verzameld door de agent, dit is in cache opgeslagen op de computer gedurende ongeveer een minuut of totdat de in de cache opgeslagen gegevens een grootte van 5MB heeft bereikt. Er wordt vervolgens gecomprimeerd en verzonden via een beveiligde verbinding met de Cloud-App Discovery-service.

Als de-agent niet mogelijk om te communiceren met de Cloud-App Discovery-service voor welke reden dan ook is, wordt de verzamelde metagegevens wordt opgeslagen in de cache voor een lokale bestanden die alleen kan worden gebruikt door gebruikers met bevoegdheden op de computer (zoals de beheerdersgroep). <br>
De agent probeert automatisch opnieuw verzenden van de metagegevens in de cache opgeslagen totdat deze is ontvangen door de Cloud-App Discovery-service.



## <a name="receiving-the-data-at-the-service-end"></a>De gegevens aan het einde van de service ontvangen

De agenten worden geverifieerd bij de Cloud-App-Discovery-service met behulp van het certificaat voor serververificatie specifieke client machine hierboven vermelde en doorgestuurd gegevens via een versleuteld kanaal. <br>
De Cloud-App Discovery-service analytics verkooppijplijn verwerkt metagegevens voor elke klant afzonderlijk door deze logisch partitioneren door alle diverse stadia van de pijplijn analytics.
De geanalyseerde metagegevens stations de verschillende rapporten in de portal.

De onverwerkte metagegevens en de geanalyseerde metagegevens worden voor maximaal 180 dagen opgeslagen. Daarnaast kunnen klanten kiezen voor het vastleggen van de geanalyseerde metagegevens in een Azure blob storage-account van hun keuze.
Dit is handig voor offline analyse van metagegevens, evenals langer behoud van de gegevens.

## <a name="accessing-the-data-using-the-azure-portal"></a>Toegang tot de gegevens met behulp van de Azure portal

In een poging om te beveiligen van de metagegevens die worden verzameld, al dan niet standaard hebben alleen globale beheerders van de tenant toegang tot de Cloud-App Discovery-functie in de portal van Azure. <br>
Beheerders kunt echter deze Gemachtigdentoegang aan andere gebruikers of groepen.



> [AZURE.NOTE] Zie voor meer informatie [Aan de slag met Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

<br>
Elke gebruiker die toegang tot de gegevens in de portal moet beschikken over een licentie Azure AD Premium.



##<a name="additional-resources"></a>Aanvullende informatie


* [Hoe kan ik verzoek tot niet toegestande cloud-apps die worden gebruikt Ontdek binnen mijn organisatie](active-directory-cloudappdiscovery-whatis.md)
* [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
