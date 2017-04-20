<properties
   pageTitle="Classificatie van gegevens voor Azure | Microsoft Azure"
   description="In dit artikel bevat een introductie over de grondbeginselen van classificatie van gegevens en wordt de waarde, specifiek in de context van cloud computing en het gebruik van Microsoft Azure gemarkeerd"
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yurid"/>

# <a name="data-classification-for-azure"></a>Classificatie van gegevens voor Azure

In dit artikel biedt een introductie over de grondbeginselen van classificatie van gegevens en wordt de waarde, specifiek in de context van cloud computing en het gebruik van Microsoft Azure gemarkeerd. 

## <a name="data-classification-fundamentals"></a>Classificatie over grondbeginselen van gegevens

Classificatie geslaagd gegevens in een organisatie is vereist voor brede chatstatus van de behoeften van uw organisatie en een goed begrip van waarin de activa van uw gegevens zich bevinden.  
 
Gegevens zich in een van de drie eenvoudige Staten: 

- In rust 
- Klik in het proces 
- Tijdens overdracht 
 
Alle drie staten vereisen unieke technische oplossingen voor classificatie van gegevens, maar de toegepaste beginselen van classificatie van gegevens moet hetzelfde voor elk label. Gegevens die is ingedeeld als vertrouwelijk moet blijven vertrouwelijke wanneer u op de rest, klik in het proces en tijdens overdracht. 
 
Gegevens kan ook zijn gestructureerde of ongestructureerde. Normale classificatie-processen voor de gestructureerde gegevens uit databases en spreadsheets zijn minder complexe en tijdrovende dan die voor ongestructureerde gegevens zoals documenten, broncode en e-mail beheren. 

> [AZURE.TIP] voor meer informatie over mogelijkheden voor Azure en aanbevolen procedures voor gegevens lezen versleuteling [Aanbevolen procedures voor versleuteling Azure-gegevens](azure-security-data-encryption-best-practices.md)

In het algemeen, organisaties hebben meer ongestructureerde gegevens dan gestructureerde gegevens. Ongeacht of gegevens gestructureerde of ongestructureerde, is het belangrijk dat u voor het beheren van gegevens gevoeligheid. Wanneer de juiste wijze geïmplementeerd, helpt gegevens classificatie activa worden beheerd met groter supervisie dan gegevens activa die worden beschouwd als openbare of gratis distributie gevoelige of vertrouwelijke gegevens. 

### <a name="controlling-access-to-data"></a>Toegang tot gegevens beheren 

Verificatie en machtiging zijn vaak verward met elkaar en hun rollen verkeerd begrepen. In feite zijn ze heel anders, zoals weergegeven in de volgende afbeelding.  

![Toegang tot gegevens en beheermogelijkheden](./media/azure-security-data-classification/azure-security-data-classification-fig1.png)

### <a name="authentication"></a>Verificatie 

Verificatie meestal bestaat uit ten minste twee delen: een gebruikersnaam of gebruikers-ID aan te geven van een gebruiker en een token, zoals een wachtwoord om te bevestigen dat de gebruikersnaam referentie geldig is. Het proces biedt geen de geverifieerde gebruiker met toegang tot alle items of services; Er wordt gecontroleerd of de gebruiker is die er gezegd dat ze zijn.   

> [AZURE.TIP] [Azure Active Directory](../active-directory/active-directory-whatis.md) biedt identiteit cloudgebaseerde waarmee u kunt de verificatie en machtiging van gebruikers. 

### <a name="authorization"></a>Autorisatie
 
Autorisatie is het proces aan te bieden een geverifieerde gebruiker toegang tot een toepassing, gegevensverzameling, gegevensbestand of een ander object. Het toewijzen van geverifieerde gebruikers machtigen voor het gebruikt, wijzigen of verwijderen items die ze toegang tot vestigen op classificatie van gegevens is vereist. 

Succesvolle autorisatie vereist implementatie van een om voor het valideren van de behoeften van de afzonderlijke gebruikers toegang tot bestanden en informatie op basis van een combinatie van de rol, beveiligingsbeleid en risico beleid overwegingen. Bijvoorbeeld: gegevens uit specifieke LOB-(LOB) toepassingen mogelijk niet moet worden gebruikt door alle werknemers en alleen een klein aantal werknemers hebben toegang tot bestanden HRM (h) waarschijnlijk nodig. Maar voor organisaties om te bepalen wie toegang heeft tot gegevens, zoals een effectieve systeem voor het verifiëren van gebruikers ook als wanneer en hoe, op hun plaats staan moeten. 

> [AZURE.TIP] Zorg dat u gebruikmaken van Azure Role-Based Access besturingselement (RBAC) als u wilt verlenen alleen het bedrag van access die gebruikers moeten hun taken uitvoeren in Microsoft Azure. Lees [de roltoewijzingen gebruiken voor het beheren van toegang tot uw Azure Active Directory-bronnen](../active-directory/role-based-access-control-configure.md) voor meer informatie. 

### <a name="roles-and-responsibilities-in-cloud-computing"></a>Rollen en verantwoordelijkheden in cloud computing 

Hoewel cloud providers risico's beheren helpen kunnen, klanten nodig hebt om ervoor te zorgen dat classificatie gegevensbeheer en afdwingen juist is geïmplementeerd om het gewenste niveau van data management-services.  
 
Gegevens classificatie verantwoordelijkheden varieert afhankelijk van het welk model cloud-service op hun plaats staan is, zoals wordt weergegeven in de volgende afbeelding. De drie modellen van de primaire cloud-service zijn infrastructuur als een service (IaaS), het platform als een service (PaaS) en software als service (SaaS). Implementatie van gegevens classificatie regelingen wordt ook afhankelijk zijn van de vertrouwen op en de verwachtingen van de cloud-provider. 

![Rollen](./media/azure-security-data-classification/azure-security-data-classification-fig2.png)

Hoewel u verantwoordelijk bent voor classificatie van uw gegevens, moet de cloud-providers geschreven verplichtingen over hoe ze beveiligt en voor het behoud van de privacy van de klantgegevens die zijn opgeslagen in de cloud.  

- **IaaS providers** vereisten zijn beperkt tot ervoor te zorgen dat de virtuele omgeving geschikt voor classificatie mogelijkheden om gegevens en nalevingsvereisten van de klant. IaaS providers hebben een kleinere rol in de classificatie van gegevens, omdat ze alleen nodig hebt om ervoor te zorgen dat klantgegevens nalevingsvereisten adressen. Echter providers moeten nog steeds ervoor zorgen dat hun virtuele omgevingen gegevens classificatie vereisten naast het beveiligen van hun datacenters adres.
- **PaaS providers** verantwoordelijkheden worden gecombineerd, omdat het platform kan worden gebruikt in een gelaagde benadering beveiliging voor een hulpmiddel classificatie. PaaS providers mogelijk die verantwoordelijk is voor de verificatie en mogelijk enkele autorisatieregels en moeten ondersteuning bieden voor beveiliging en mogelijkheden om gegevens classificatie naar hun toepassingslaag. Net zoals IaaS providers PaaS providers nodig hebt om ervoor te zorgen dat hun platform aan de vereisten van alle relevante gegevens classificatie voldoet.
- **SaaS providers** vaak worden beschouwd als onderdeel van een keten autorisatie en wordt nodig hebt om ervoor te zorgen dat de gegevens die zijn opgeslagen in de SaaS toepassing kan worden beheerd door classificatie type. SaaS toepassingen kunnen worden gebruikt voor LOB-toepassingen, en door hun aard behoefte aan het middel te verifiëren en autoriseren gegevens die wordt gebruikt en dat is opgeslagen. 

## <a name="classification-process"></a>Indeling proces 

Veel organisaties die begrijpen dat u een classificatie van gegevens te laten voor de uitvoering van deze computer een eenvoudige uitdaging zien: waar u moet beginnen?

Een effectieve en eenvoudige manier om te implementeren classificatie van gegevens is gebruik van de taak,-abonnement controleren, gedragen zich model [MOF](https://technet.microsoft.com/solutionaccelerators/dd320379.aspx). De volgende afbeelding grafieken de taken die nodig zijn met succes classificatie van gegevens in dit model kunt implementeren.  

1. **Plannen**. Welke gegevens activa, een beheerder van de gegevens kunt implementeren van het programma classificatie en ontwikkelen van beveiliging profielen. 
2. **DO**. Nadat het beleid voor classificatie van gegevens zijn akkoord gaat, het programma te implementeren en afdwingen technologieën implementeren naar wens voor vertrouwelijke gegevens.  
3. **Controleren**. Controleer en rapporten om ervoor te zorgen dat de hulpprogramma's en methoden gebruikte zijn effectief adressering van het beleid classificatie valideren. 
4. **ACT**. De status van de toegang tot gegevens en bestanden controleren en gegevens die moeten worden herzien met een nieuwe classificatie en de revisiedatum methodologie vast te stellen van wijzigingen en adres nieuwe risico's bekijken.  

![Plannen,, controleren, gedragen zich](./media/azure-security-data-classification/azure-security-data-classification-fig3.png)
 
###<a name="select-a-terminology-model-that-addresses-your-needs"></a>Selecteer een terminologie-model dat uw behoeften adressen
 
Diverse soorten processen bestaat voor classificatie van gegevens, inclusief handmatige processen, locatie gebaseerde processen die gegevens classificeren op basis van de locatie van een gebruiker of van het systeem, processen, op basis van een toepassing zoals database / regiospecifieke-indeling, en wordt automatisch in processen die worden gebruikt door verschillende technologieën, een aantal van de plaats waar in de sectie 'Beveiligen van vertrouwelijke gegevens' verderop in dit artikel wordt beschreven.  
 
In dit artikel maakt u kennis met twee generalized terminologie modellen die zijn gebaseerd op modellen goed gebruikte en industriële nageleefd. Deze terminologie-modellen, die beide drie niveaus van classificatie gevoeligheid bieden, worden weergegeven in de volgende tabel.  

> [AZURE.NOTE] bij de indeling van een bestand of de resource die gegevens die meestal zou worden geclassificeerd op verschillende niveaus worden gecombineerd, moet het hoogste niveau van de classificatie presenteren stand brengen van de algemene indeling. Bijvoorbeeld, een bestand met gevoelige en beperkte gegevens moet worden geclassificeerd als beperkt.  

| **Gevoeligheid**   | **Terminologie model 1**   | **Terminologie model 2** |
|--------------------|---------------------------|-------------------------|
| Hoog               | Vertrouwelijke              | Beperkte              |
| Gemiddeld             | Alleen voor intern gebruik     | Gevoelige               |
| Laag                | Openbare                    | Onbeperkte            |

#### <a name="confidential-restricted"></a>Vertrouwelijk (beperkt) 

Informatie die is ingedeeld als vertrouwelijk of beperkte bevat gegevens die kunnen worden fatale naar een of meer personen en/of organisaties als beschadigde of verloren. Deze gegevens regelmatig op basis van 'moeten weten' wordt weergegeven en mogelijk onder andere: 

- Persoonlijke gegevens, met inbegrip van persoonlijke gegevens zoals Burgerservice-nummer of nationale ID-nummers, passport cijfers, creditcardnummers, van stuurprogramma-licentie getallen, medische gegevens en ziektekostenverzekering beleid-id-nummers.  
- Financiële records, waaronder financiële rekeningnummers zoals controleren of rekeningnummers investering. 
- Zakelijke materiaal, zoals documenten of gegevens die unieke of specifieke geen intellectueel eigendom.  
- Juridische gegevens, inclusief mogelijke advocaat rechten materiaal. 
- Verificatiegegevens, zoals privé cryptografische toetsen, gebruikersnaam wachtwoord paren of andere reeksen identificatie zoals privé biometrische belangrijke bestanden. 

Gegevens die wordt geclassificeerd als vertrouwelijk vaak hebben regelgeving en nalevingsvereisten voor gegevensverwerking. 

#### <a name="for-internal-use-only-sensitive"></a>Voor intern gebruik alleen (gevoelige)
 
Informatie die wordt geclassificeerd als van Gemiddeld Gevoeligheid bevat bestanden en gegevens die niet een ernstige invloed op een persoon en/of de organisatie hebben kan als verloren of verwijderd. Deze informatie mogelijk onder andere: 

- E-mail, meestal die kunnen worden verwijderd of gedistribueerd zonder dat een crisis (exclusief postvakken of e-mail van personen die zijn geïdentificeerd in de vertrouwelijke classificatie).  
- Documenten en bestanden die geen vertrouwelijke gegevens bevatten.
 
Deze indeling bevat in het algemeen wordt alles wat geen vertrouwelijke. Deze indeling kan de meeste zakelijke gegevens, bevatten, omdat de meeste bestanden die worden beheerd of dagelijkse gebruikt kunnen worden geclassificeerd als gevoelige. Met uitzondering van de gegevens die wordt openbaar gemaakt of vertrouwelijk is, kunnen alle gegevens binnen een organisatie bedrijven worden geclassificeerd als gevoelige al dan niet standaard. 

#### <a name="public-unrestricted"></a>Openbare (onbeperkte)
 
Informatie die wordt geclassificeerd als openbare omvat gegevens en bestanden die niet essentieel voor zakelijke behoeften of bewerkingen. Deze indeling kan ook bevatten gegevens die expliciet is uitgebracht voor het publiek voor het gebruik ervan, zoals marketing materiaal of drukt u op aankondigingen. Bovendien kan deze indeling gegevens zoals spam e-mailberichten die zijn opgeslagen door een e-mailservice bevatten. 

### <a name="define-data-ownership"></a>Gegevenseigendom definiëren
 
Het is belangrijk tot stand brengen van een wissen bewaarnemingsovereenkomst reeks eigendom voor alle gegevens activa. De volgende tabel bevat verschillende gegevens eigendom rollen in gegevens classificatie inspanningen en hun bijbehorende rechten.  

| **Rol**        | **Maken**    | **Wijzigen/verwijderen**   | **Gemachtigde**  | **Lezen**    | **Archief/herstellen**   |
|-----------------|---------------|---------------------|---------------|-------------|-----------------------|
| Eigenaar           | X             | X                   | X             | X           | X                     |
| Beheerder       |               |                     | X             |             |                       |
| Beheerder   |               |                     |               |             | X                     |
| Gebruiker\*          |               | X                   |               | X           |                       |
**Gebruikers kunnen worden verleend aanvullende rechten zoals bewerken en verwijderen door een beheerder* 

> [AZURE.NOTE] in deze tabel biedt een volledige lijst van rollen en rechten, maar slechts een steekproef geen. 

De **eigenaar van de activa gegevens** is de oorspronkelijke maker van de gegevens, wie kan delegeren eigendom en toewijzen van een beheerder. Wanneer een bestand is gemaakt, kunnen de eigenaar van het toewijzen van een indeling, wat betekent dat ze een verantwoordelijkheid te begrijpen wat u moet worden geclassificeerd als vertrouwelijk op basis van de beleidsregels van hun organisatie moet mogelijk. Alle gegevens van een gegevens activa eigenaar kan zijn, automatisch worden geclassificeerd als voor intern gebruik alleen (gevoelige) tenzij ze verantwoordelijk zijn voor eigenaar of vertrouwelijke (beperkt) gegevenstypen maken. De rol van de eigenaar wordt vaak worden gewijzigd nadat de gegevens is ingedeeld. Bijvoorbeeld mogelijk de eigenaar van het maken van een database met ingedeeld informatie en afstaat hun machtigen voor de beheerder van de gegevens.  

> [AZURE.NOTE] gegevens activa eigenaren wordt vaak gebruikt een combinatie van services, apparaten en media, sommige hiervan zijn persoonlijke en sommige deel uitmaakt van de organisatie. Schakel het selectievakje organisatie-beleid kunt ervoor te zorgen dat gebruik van apparaten, zoals laptops en apparaten die slimme volgens de richtlijnen voor classificatie van gegevens.  

De **gegevens activa beheerder** wordt toegewezen door de eigenaar van de activa (of hun gemachtigde) voor het beheren van de activa op basis van de overeenkomsten met de eigenaar van de activa of volgens toepasselijk vereisten. In het ideale geval kan de beheerdersrol worden geïmplementeerd in een geautomatiseerd systeem. De beheerder van een activum zorgt ervoor dat nodig toegang tot besturingselementen worden verstrekt is verantwoordelijk voor het beheren en beveiligen van activa overgedragen naar hun noodzakelijk. De verantwoordelijkheden van de activa-beheerder kunnen zijn:  

- Beveiligen van de activa overeenkomstig de richting van de eigenaar van de activa of in overeenstemming met de eigenaar van de activa 
- Ervoor te zorgen dat de classificatie beleidsregels wordt voldaan 
- Eigenaren van de activa van wijzigingen in de vooraf vastgestelde besturingselementen en/of beveiliging procedures voordat u deze wijzigingen die van kracht als zodanig 
- Rapporteren aan de eigenaar van de activa over wijzigingen in of verwijderen van de verantwoordelijkheden van de beheerder van de activa 
- Een **beheerder** vertegenwoordigt een gebruiker aan wie is verantwoordelijk voor integriteit behouden blijft, maar ze zijn niet een eigenaar van de activa gegevens, de beheerder of de gebruiker. Ja, bieden veel beheerdersrollen gegevensservices container management zonder toegang tot de gegevens. De beheerdersrol bevat back-up en herstellen van de gegevens, de records van de activa, onderhouden en kiezen, bij het verkrijgen van en werken met de apparaten en de opslag die house de activa. 
- De gebruiker activa bevat alle personen die toegang tot gegevens of een bestand is verleend. Access-toewijzing is vaak gedelegeerd door de eigenaar voor de beheerder van de activa.  

### <a name="implementation"></a>Implementatie
  
Overwegingen bij toepassen op alle classificatie methoden. De volgende punten moeten bevatten meer informatie over wie, wat, waar en wanneer en waarom een activum gegevens zou worden gebruikt, toegankelijk, gewijzigd of verwijderd. Beheer van alle activa moet worden uitgevoerd met een begrip van hoe de risico's in een organisatie worden weergaven, maar een eenvoudige methodologie kan worden toegepast, zoals gedefinieerd in het proces van de classificatie gegevens. Aanvullende overwegingen voor classificatie van gegevens bevatten de introductie van nieuwe toepassingen en hulpprogramma's en beheren van wijzigen nadat een methode classificatie is geïmplementeerd.  

### <a name="reclassification"></a>Nieuwe classificatie
 
Herindelen of het wijzigen van de status van de classificatie van een activum gegevens moet worden uitgevoerd wanneer een gebruiker of systeem bepaalt dat de gegevens van de activa urgentie of risico profiel is gewijzigd. Deze hoeveelheid is het belangrijk om ervoor te zorgen dat de status van de classificatie blijft geldig en actueel zijn. De meeste inhoud die niet handmatig geclassificeerd kan worden geclassificeerd automatisch of op basis van gebruik door een beheerder van de gegevens of de eigenaar van de gegevens. 

### <a name="manual-data-reclassification"></a>Gegevens handmatig opnieuw indelen
 
Deze hoeveelheid zou ideale geval ervoor zorgen dat de details van een wijziging vastgelegd en gecontroleerd. De meest waarschijnlijke reden voor het handmatig herindelen zou voor redenen van gevoeligheid of voor registratie in het papierformaat of een vereiste dat gegevens bekijken die oorspronkelijk verkeerd is geclassificeerd. Omdat dit artikel classificatie van gegevens en zwevend gegevens in de cloud acht, handmatige nieuwe classificatie inspanningen aandacht op basis van de door geval zou vereisen en een controle door een management risico zou ideaal naar adres classificatie vereisten. In het algemeen, namelijk zoals een poging van de organisatie beleid over wat u moet worden geclassificeerd, de standaardstatus classificatie (alle gegevens en bestanden wordt vertrouwelijke, maar niet vertrouwelijke), en maak uitzonderingen voor een hoog risico gegevens. 

### <a name="automatic-data-reclassification"></a>Automatische gegevens opnieuw indelen
 
Automatische gegevens nieuwe classificatie gebruikt dezelfde regel voor algemene als handmatige classificatie. Geldt dat geautomatiseerde oplossingen kunnen Zorg ervoor dat de regels worden gevolgd en naar wens te toegepast. Classificatie van gegevens kan worden uitgevoerd als onderdeel van een gegevens classificatie afdwingen beleid, dat kan worden afgedwongen wanneer gegevens worden opgeslagen, wordt gebruikt en tijdens overdracht autorisatie technologie gebruiken.

- Op basis van toepassing. Het niveau van een indeling met bepaalde toepassingen al dan niet standaard worden ingesteld. Gegevens uit de software customer relationship management (CRM), HR en systeemstatus recordbeheer's is bijvoorbeeld vertrouwelijke al dan niet standaard. 
- Afhankelijk van de locatie. Gegevenslocatie kunt gegevens gevoeligheid achterhalen. Gegevens die zijn opgeslagen door een HR of financiële afdeling wordt bijvoorbeeld vaker vertrouwelijk in aard.  
 
### <a name="data-retention-recovery-and-disposal"></a>Gegevensretentie, herstel en verwijdering 

Gegevens herstellen en de verwijdering, zoals nieuwe classificatie van gegevens, is een essentiële aspecten van de activa van de gegevens te beheren. De beginselen voor herstel van gegevens en buitengebruikstelling zou worden gedefinieerd door een bewaarbeleid voor gegevens en afgedwongen op dezelfde manier als nieuwe classificatie van gegevens; een dergelijke hoeveelheid uitgevoerd door de beheerder en beheerder rollen als een gezamenlijke taak.  

Wanneer u niet over een bewaarbeleid gegevens kan betekenen gegevensverlies of overtreding van wet- en regelgeving discovery vereisten. De meeste organisaties die ik heb een duidelijke gegevensretentiebeleid vaak gebruiken van een bewaarbeleid standaard 'behouden alles'. Een bewaarbeleid heeft echter extra risico's in de cloud services-scenario's. 

Een bewaarbeleid voor gegevens voor de cloud-mailproviders kan bijvoorbeeld worden beschouwd als voor "de duur van het abonnement" (mits de service is betaald voor de gegevens blijven behouden). Dergelijke een betaald wordt voor bewaarbeleid overeenkomst mogelijk niet bedrijfs- of regelgeving bewaarbeleid behandeld. Een beleid definieert voor vertrouwelijke gegevens kunt ervoor zorgen dat gegevens worden opgeslagen en verwijderd op basis van de aanbevolen procedures. Daarnaast een archivering beleid kan worden gemaakt als u wilt controlemethodologie inzicht over welke gegevens moeten worden verwijderd en wanneer. 

Bewaarbeleid voor gegevens kan worden opgelost de vereiste regelgeving en nalevingsvereisten, evenals zakelijke juridische zelf vereisten. Ingedeeld gegevens kunnen leiden tot vragen over bewaarbeleid duur en uitzonderingen voor gegevens die is opgeslagen bij een provider; deze vragen zijn waarschijnlijk meer geneigd voor gegevens die niet correct zijn geclassificeerd. 

> [AZURE.TIP] meer informatie over bewaarbeleid Azure-gegevens en meer vindt met de [Microsoft Online abonnementsovereenkomst](https://azure.microsoft.com/support/legal/subscription-agreement/) lezen

## <a name="protecting-confidential-data"></a>Beveiligen van vertrouwelijke gegevens
  
Nadat de gegevens wordt geclassificeerd, wordt zoeken en implementeren van manieren om te beveiligen van vertrouwelijke gegevens een integraal onderdeel van een strategie voor gegevensbescherming implementatie. Beveiligen van vertrouwelijke gegevens vereist extra aandacht aan hoe gegevens worden opgeslagen en verzonden gegevens in de gebruikelijke architecturen ook zoals in de cloud. 

Deze sectie bevat algemene informatie over enkele technologieën die afdwingen inspanningen om u te helpen beschermen van gegevens die is ingedeeld als vertrouwelijk kunt automatiseren. 
 
Als de volgende afbeelding ziet, kunt deze technologieën implementeren als on-premises implementatie of cloud gebaseerde oplossingen, of op een wijze hybride, klikt u met enkele ze geïmplementeerd on-premises en enkele in de cloud. (Enkele technologieën, zoals versleuteling en (rights management), ook uitbreiden naar Gebruikersapparaten.)  

![Technologieën](./media/azure-security-data-classification/azure-security-data-classification-fig4.png)

### <a name="rights-management-software"></a>Rights management-software  

Een oplossing voor het voorkomen van gegevensverlies is rights management software. In tegenstelling tot methoden die u wilt de stroom van gegevens op afsluiten punten in een organisatie onderbreken, werkt de rights management software op hoog niveau binnen gegevens opslagtechnologieën. Documenten zijn versleuteld en controle over wie ze kunt ontsleutelen wordt gebruikt voor toegang tot besturingselementen die zijn gedefinieerd in een oplossing voor het beheer van verificatie zoals een adreslijstservice.  

> [AZURE.TIP] u kunt Azure Rights Management (Azure RMS) als de informatie beveiliging oplossing gebruiken om gegevens in verschillende scenario´s te beveiligen. Alleen [Wat is Azure Rights Management?](https://docs.microsoft.com/rights-management/understand-explore/what-is-azure-rms) voor meer informatie over deze Azure oplossing.

Enkele van de voordelen van rights management software zijn: 

- Beveiligde vertrouwelijke informatie. Gebruikers kunnen hun gegevens rechtstreeks met rights management-toepassingen beveiligen. Geen extra stappen zijn vereist: cocreatie van documenten, e-mail verzenden en het publiceren van gegevens van dienst consistente gegevens beveiliging. 
- Beveiliging legt met de gegevens. Klanten blijven bepalen wie toegang tot hun gegevens, heeft in de cloud, bestaande IT-infrastructuur, of op het bureaublad van de gebruiker. Organisaties kunnen kiezen voor het coderen van hun gegevens en het beperken van toegang volgens de vereisten van hun bedrijven. 
- Standaardbeleidsregels informatie beveiliging. Beheerders en gebruikers kunnen standaardbeleid gebruiken voor veel algemene bedrijfsscenario's, zoals ' Bedrijf vertrouwelijk – is alleen-lezen' en "Niet doorsturen." Een uitgebreide set gebruik rights worden ondersteund, zoals lezen, kopiëren, afdrukken, opslaan, bewerken en doorsturen flexibiliteit bij het definiëren van aangepaste gebruiksrechten toe te passen. 

> [AZURE.TIP] u kunt gegevens in Azure opslag beveiligen met behulp van [Azure opslag Service versleuteling](../storage/storage-service-encryption.md) voor gegevens in rust. U kunt ook [Azure schijfversleuteling](azure-security-disk-encryption.md) gebruiken om te helpen beveiligen van gegevens op virtuele schijven voor Azure virtuele Machines gebruikt.

### <a name="encryption-gateways"></a>Versleuteling gateways

Versleuteling gateways werken in hun eigen lagen versleuteling dienstverlening door alle toegang tot gegevens cloudgebaseerde omleiden. Deze methode moet niet verwarren met die van een VPN (VPN). Versleuteling gateways zijn ontworpen voor een transparante laag naar cloud gebaseerde oplossingen.   

Versleuteling gateways kunnen gebruikers een gemiddelden om te beheren en beveiligde gegevens dat is ingedeeld als vertrouwelijk door te coderen van de gegevens tijdens overdracht en gegevens in rust.  
 
Versleuteling gateways worden centers voor ontsleutelen/services in de gegevensstroom tussen Gebruikersapparaten en toepassingsgegevens geplaatst. Deze oplossingen, zoals VPN's, zijn hoofdzakelijk on-premises implementatie-oplossingen. Ze zijn ontworpen om u te voorzien van een derde partij controle over versleuteling toetsen, waardoor het risico van het beheer van de gegevens en de toets met één provider plaatsen verlagen. Deze oplossingen zijn ontworpen, net zoals versleuteling, om te werken naadloos samen en transparant tussen gebruikers en de service. 

> [AZURE.TIP] u kunt Azure ExpressRoute voor het uitbreiden van uw on-premises implementatie-netwerken te gebruiken in de cloud Microsoft via een speciale privé verbinding. Lees [technisch overzicht ExpressRoute](../expressroute/expressroute-introduction.md) voor meer informatie over deze mogelijkheden. Een andere opties voor cross lokale verbinding tussen uw on-premises netwerk en [Azure is een VPN-verbinding voor de site-naar-site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

### <a name="data-loss-prevention"></a>Preventie van gegevensverlies 
Verlies van gegevens (ook wel gegevens lekkage genoemd) is een belangrijke overweging en voorkoming van externe gegevensverlies via schadelijke en ongeluk insiders grootste voor veel organisaties.  
 
Gegevensverlies preventie (DLP) technologieën kunt ervoor zorgen dat oplossingen zoals e-mailservices gegevens die is ingedeeld als vertrouwelijk niets verzenden. Organisaties kunnen profiteren van DLP-functies in bestaande producten om te voorkomen dat gegevens verloren gaan. Deze functies gebruikmaken van het clienttoegangsbeleid dat eenvoudig kunnen worden gemaakt maken of met behulp van een sjabloon die door de software-provider zijn verschaft.  
 
DLP-technologieën kunnen uitgebreide inhoud analysen tot en met trefwoord overeenkomsten, woordenlijst overeenkomsten evaluatie van reguliere expressies en andere inhoud onderzoek om te bepalen van inhoud die in overtreding is met organisatorische DLP-beleid uitvoeren. Bijvoorbeeld kunt DLP voorkomen dat het verlies van de volgende soorten gegevens: 

- Burgerservice-nummer en de nationale ID-nummers 
- Informatie van de bank 
- Creditcardnummers  
- IP-adressen 

Enkele DLP-technologieën bieden ook de mogelijkheid om te negeren de DLP-configuratie (bijvoorbeeld als een organisatie verzenden sofi-nummer informatie aan een processor salarisadministratie moet). Bovendien is het mogelijk DLP configureren zodat gebruikers worden gewaarschuwd voordat ze zelfs proberen kunt u vertrouwelijke informatie die niet moet worden overgebracht verzenden. 

> [AZURE.TIP] u kunt Office 365 DLP-mogelijkheden gebruiken om te beveiligen van uw documenten. Alleen [besturingselementen voor Office 365-naleving: preventie van gegevensverlies](https://blogs.office.com/2013/10/28/office-365-compliance-controls-data-loss-prevention/) voor meer informatie.

## <a name="see-also"></a>Zie ook

- [Aanbevolen procedures voor Azure gegevens-versleuteling](azure-security-data-encryption-best-practices.md)
- [Azure identiteits- en access bepalen aanbevolen procedures voor beveiliging](azure-security-identity-management-best-practices.md)
- [Teamblog van Azure beveiliging](http://blogs.msdn.com/b/azuresecurity/)
- [Antwoord van Microsoft Beveiligingscentrum](https://technet.microsoft.com/library/dn440717.aspx)

