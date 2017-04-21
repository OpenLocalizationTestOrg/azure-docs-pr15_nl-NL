# <a name="compute"></a>Berekenen

Azure kunt u implementeren en te controleren van uw toepassingscode die wordt uitgevoerd in een Microsoft-Datacenter. Wanneer u een toepassing maken en uitvoeren op Azure, de code en configuratie samen heet een Azure gehost service. Gehoste services zijn gemakkelijk te beheren, schaal omhoog en omlaag, opnieuw configureren en bijwerken met nieuwe versies van de code van uw toepassing. In dit artikel bevat informatie over het Azure gehost service-toepassingsmodel.<a id="compare" name="compare"></a>

## Inhoudsopgave<a id="_GoBack" name="_GoBack"></a>

-   [Voordelen van Azure-toepassing Model][]
-   [Gehoste Service Core concepten][]
-   [Overwegingen bij het ontwerpen van gehoste Service][]
-   [Ontwerpen van uw toepassing voor schaal][]
-   [Definitie van gehoste Service- en configuratieservices][]
-   [De definitie servicebestand][]
-   [Het configuratiebestand Service][]
-   [Maken en implementeren van een gehoste Service][]
-   [Verwijzingen][]

## <a id="benefits"> </a>Azure-toepassing Model voordelen

Wanneer u uw toepassing als een gehoste service implementeert, Azure Hiermee maakt u een of meer virtuele machines (VMs) dat van uw toepassing code bevat en de VMs wordt opgestart op fysieke machines op een van de Azure datacenters wonen. Terwijl clientaanvragen voor de gehoste toepassing het datacenter invoert, taakverdeling deze aanvragen gelijkmatig voor de VMs. Terwijl uw toepassing wordt gehost in Azure wordt aangegeven, wordt deze drie belangrijke voordelen:

-   **Beschikbaarheid.** Beschikbaarheid betekent dat Azure zorgt ervoor dat uw toepassing zo veel mogelijk wordt uitgevoerd en kan reageren op aanvragen van clients. Als uw toepassing wordt beëindigd (vanwege een onverwerkte uitzondering, bijvoorbeeld), en vervolgens Azure dit detecteert, en deze automatisch opnieuw uw toepassing te starten. Als de computer uw toepassing wordt uitgevoerd op ervaringen een soort hardware mislukt, wordt klikt u vervolgens Azure ook detecteren dit automatisch een nieuwe VM maken op een andere werken fysieke machine en voert u uw code daarvandaan. Opmerking: In de volgorde voor uw toepassing om Microsoft Service Level Agreement van 99.95% beschikbaar, moet u ten minste twee VMs uitvoering van de toepassing programmacode hebben. Hiermee kunt één VM clientaanvragen verwerken terwijl Azure uw code van een mislukte VM naar een nieuwe, goede VM verplaatst.

-   **Schaalbaarheid.** Azure kunt u eenvoudig en het aantal VMs uitvoering van de toepassing programmacode u omgaat met de werkelijke belasting van uw toepassing dynamisch te wijzigen. Hiermee kunt u naar het aanpassen van uw toepassing naar de werklast dat uw klanten erop plaatst terwijl betaalt alleen voor de VMs die u nodig hebt wanneer u deze nodig hebt. Als u wijzigen hoeveel VMs wilt, reageert Azure binnen minuten geboden die het mogelijk om het aantal VMs wordt uitgevoerd met vaak desgewenst dynamisch te wijzigen.

-   **Beheerbaarheid.** Omdat Azure een Platform als een Service (PaaS is) aanbod, beheert de infrastructuur (de hardware zelf, elektriciteit en netwerken) vereist om deze computers met behouden. Azure beheert ook het platform, zorgen dat een recente besturingssysteem met alle de juiste patches en beveiligingsupdates, evenals de updates van onderdelen zoals de .NET Framework en Internet Information Server. Omdat alle VMs Windows Server 2008 uitvoert, bevat Azure aanvullende functies zoals diagnostische controle, ondersteuning voor externe bureaublad, firewalls en configuratie van certificaten store. Al deze functies worden gegeven aan geen extra kosten. Zelfs wanneer u uw toepassing in Azure wordt aangegeven uitvoert, de licentie voor Windows Server 2008-besturingssysteem (OS) niet wordt vermeld. Aangezien alle de VMs Windows Server 2008 uitvoert, zijn werkt een code, die wordt uitgevoerd op Windows Server 2008 goed als actief in Azure wordt aangegeven.

## <a id="concepts"> </a>Service Core concepten die worden gehost

Wanneer uw toepassing is geïmplementeerd als een gehoste service in Azure wordt aangegeven, wordt uitgevoerd als een of meer *rollen.* Een *rol* verwijst alleen naar toepassingsbestanden en configuratie. U kunt een of meer rollen definiëren voor uw toepassing, elk voorzien van een eigen set toepassingsbestanden en configuratie. Voor elke rol in uw toepassing, kunt u het aantal VMs of *rol exemplaren*, om uit te voeren. De onderstaande afbeelding weergeven twee eenvoudige voorbeelden van een toepassing gezien als een gehoste service met rollen en rol exemplaren.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Afbeelding 1: Één rol met drie exemplaren (VMs) uitgevoerd in een Azure Datacenter

![afbeelding][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Afbeelding 2: Twee rollen, elk met twee instanties (VMs), uitgevoerd in een Azure Datacenter

![afbeelding][1]

Rol exemplaren verwerken meestal Internet clientaanvragen voor het invoeren van het datacenter zogenaamde een *invoer eindpunt*. Een enkele rol kan 0 of meer invoer eindpunten hebben. Elk eindpunt geeft aan een protocol (HTTP, HTTPS of TCP) en een poort. Dit geldt voor het configureren van een rol invoer eindpunten: HTTP luisteren op poort 80 en HTTPS luisteren op poort 443. De onderstaande afbeelding ziet u een voorbeeld van twee verschillende rollen met verschillende invoer eindpunten waarmee clientaanvragen voor deze.

![afbeelding][2]

Wanneer u een gehoste service in Azure wordt aangegeven maakt, kunt deze een openbaar gebruikt IP-adres zijn dat clients gebruiken kunnen voor toegang tot deze is toegewezen. U moet ook een URL-voorvoegsel die is toegewezen aan dat IP-adres na het maken van de gehoste service selecteren. Dit voorvoegsel moet uniek zijn als u het *voorvoegsel*in principe reserveert. cloudapp.net URL zodat dat niemand anders deze kan hebben. Clients communiceren met uw rol-sessies met behulp van de URL. Meestal u wordt niet distribueren of publiceren het Azure *voorvoegsel*. cloudapp.net-URL. U wordt in plaats daarvan een domeinnaam kopen bij uw DNS-domeinregistrar van keuze en de naam van uw DNS-clientaanvragen omleiden naar de URL van de Azure configureren. Zie [een aangepaste domeinnaam in Azure configureren][]voor meer informatie.

## <a id="considerations"> </a>Overwegingen bij het ontwerpen van de Service die worden gehost

Bij het ontwerpen van een toepassing wilt uitvoeren in een cloud-omgeving, zijn er verschillende overwegingen om na te denken over zoals latentie, hoge beschikbaarheid en schaalbaarheid.

Bepalen waar u wilt zoeken naar uw toepassingscode is belang als u een gehoste service in Azure. Meestal wordt implementeren van de toepassing datacenters die zich het dichtst bij uw klanten latentie verkleinen en de beste prestaties mogelijke kunt ophalen.
U kunt een datacenter dichter bij uw bedrijf of dichter bij uw gegevens echter, hebt u enkele teneinde of juridische bezwaren over uw gegevens en waar deze zich bevindt. Er zijn zes datacenters overal ter wereld kunnen hostingprovider van uw toepassingscode. De volgende tabel ziet de beschikbare locaties:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Land/regio**

</td>
<td style="width: 200px;">
**Submenu regio 's**

</td>
</tr>
<tr>
<td>
Verenigde Staten

</td>
<td>
Zuid-centraal & Noord centraal

</td>
</tr>
<tr>
<td>
Europa

</td>
<td>
Noord- en West

</td>
</tr>
<tr>
<td>
Azië

</td>
<td>
Zuidoost & Oost

</td>
</tr>
</tbody>
</table>
Wanneer een gehoste service maakt, selecteert u een onderliggend regio waarin wordt aangegeven dat de locatie waar u uw code uit te voeren.

Als u wilt bereiken beschikbaarheid en schaalbaarheid, is het ernstige belangrijk dat van uw toepassing gegevens worden opgeslagen in een centrale opslagplaats toegankelijk zijn voor meerdere exemplaren van de rol. Met deze biedt, Azure diverse opslagopties zoals BLOB's, tabellen en SQL-Database. Zie het artikel [Gegevens opslag aanbiedingen in Azure wordt aangegeven][] voor meer informatie over deze opslagtechnologieën. De onderstaande afbeelding ziet hoe de binnen de Azure Datacenter taakverdeling clientaanvragen voor andere rol exemplaren die toegang tot de dezelfde gegevensopslag hebben.

![afbeelding][3]

Meestal wilt u uw toepassingscode en uw gegevens in het midden met dezelfde gegevens vinden, zoals in deze sectie voor lage latentie (betere prestaties geeft) wanneer de gegevens in uw toepassingscode wordt geopend. Daarnaast wordt niet geheven voor bandbreedte als gegevens binnen de dezelfde Datacenter rond wordt verplaatst.

## <a id="scale"> </a>Ontwerpen van uw toepassing voor schaal

Soms wilt u mogelijk één toepassing (zoals een eenvoudige website) maken en deze gehost in Azure wordt aangegeven. Maar vaak uw toepassing kan bestaan uit verschillende rollen die samenwerken. Bijvoorbeeld, in de onderstaande afbeelding er twee instanties van de rol van de Website, drie exemplaren van de rol doorvoeren en één exemplaar van de rol rapport genereren. Deze functies zijn alle die samenwerken en de code voor al deze kan worden verpakt samen en geïmplementeerd als één eenheid maximaal Azure.

![afbeelding][4]

De belangrijkste reden splitsen van een toepassing in verschillende rollen elke uitgevoerd op een eigen set van de rol-exemplaren (dat wil zeggen VMs) is aan de rollen onafhankelijk schaal. Bijvoorbeeld tijdens de feestdagen veel klanten mogelijk bestellen producten van uw bedrijf, zodat u kunt het aantal exemplaren die rol actief zijn de rol van uw Website, evenals het aantal exemplaren die rol actief zijn uw rol doorvoeren vergroten. Na de feestdagen krijgt u mogelijk een groot aantal producten die worden geretourneerd, zodat u nog steeds u een groot aantal exemplaren van de Website, maar minder doorvoeren exemplaren moet mogelijk. Tijdens de rest van het jaar hoeft u misschien maar een paar Website- en doorvoeren exemplaren. In dit alles moet u slechts één exemplaar van het rapport genereren. De flexibiliteit van rollen gebaseerde implementaties in Azure wordt aangegeven kunt u eenvoudig uw toepassing naar uw zakelijke behoeften aanpassen.

Meestal wordt de rol van exemplaren binnen uw gehoste service met elkaar communiceren. Bijvoorbeeld de rol van de website accepteert order van een klant maar klikt u vervolgens deze de volgorde verwerkt in de rol doorvoeren exemplaren compatibele. De beste manier om werk formulier één set van de rol exemplaren die naar een andere set exemplaren is met de wachtrij plaatsen technologie die is verstrekt door Azure, geven de wachtrijservice of de Service Bus wachtrijen. Het gebruik van een wachtrij is een kritieke onderdeel van het verhaal hier. De wachtrij kan de gehoste service de schaal van de rollen onafhankelijk zodat u kunt het saldo vanaf de werklast ten opzichte van de kosten aanpassen. Als het aantal berichten in de wachtrij na verloop van tijd toeneemt, klikt u vervolgens kunt u de schaal van het aantal exemplaren van rol volgorde verwerkt. Als het aantal berichten in de wachtrij na verloop van tijd afneemt, kunt u het aantal exemplaren van doorvoeren rol verkleinen. Op deze manier u alleen betaalt voor de exemplaren die zijn vereist voor het verwerken van de werkelijke werklast.

De wachtrij bevat ook betrouwbaarheid. Wanneer het verkleinen van het aantal exemplaren van rol doorvoeren, geeft Azure wordt bepaald welke exemplaren te beëindigen. Het kan besluiten een exemplaar in het midden van de verwerking van een bericht wachtrij beëindigen. Echter, omdat de berichtverwerking niet wordt voltooid, het bericht zichtbaar opnieuw voor een andere volgorde verwerkt rol exemplaar dat deze opgehaald en verwerkt. Vanwege wachtrij bericht zichtbaarheid berichten gegarandeerd uiteindelijk verwerkt. De wachtrij fungeert ook als een taakverdeling door de berichten in alle rol-exemplaren die berichten aanvraagt effectief distribueren.

Voor de Website rol exemplaren, kunt u het verkeer naar deze binnenkort bewaken en wilt schalen dat ze het aantal omhoog of omlaag als u ook. De wachtrij kunt u de schaal van het aantal exemplaren van de Website rol afzonderlijk van de rol doorvoeren exemplaren aanpassen. Dit is zeer krachtig en geeft u een groot aantal flexibiliteit. Als uw toepassing uit extra rollen bestaat, kunt u natuurlijk aanvullende wachtrijen toevoegen als de leiding om te communiceren ertussen om te kunnen gebruikmaken van dezelfde schaal en voordelen van kosten.

## <a id="defandcfg"> </a>Gehost servicedefinitie- en configuratieservices

Een gehoste service implementeren naar Azure, moet u ook een servicebestand definitie en een configuratiebestand service hebt. Beide bestanden XML-bestanden zijn en u kunt de implementatieopties voor uw gehoste service declaratieve opgeven. De definitie servicebestand worden alle van de rollen waaruit uw gehoste service en hoe ze communiceren. Het configuratiebestand service wordt het aantal exemplaren voor elke rol- en instellingen voor het configureren van elk exemplaar van de rol beschreven.

## <a id="def"> </a>Het definitiebestand Service

Zoals u ik eerder, de definitie van de service (CSDEF) wordt in het bestand een XML-bestand die goed beschrijft de verschillende rollen waaruit de volledige toepassing is. Het volledige schema voor het XML-bestand vindt u hier: [http://msdn.microsoft.com/library/windowsazure/ee758711.aspx]-[].
Het bestand CSDEF bevat een WebRole of WorkerRole element voor elke rol die u wilt dat in uw toepassing. Implementeren van een rol als een Webrol (via het element WebRole), betekent dat de code wordt uitgevoerd op een rol exemplaar met Windows Server 2008 en Internet Information Server (IIS).
Een rol implementeren als de rol van een werknemer (via het WorkerRole-element) betekent dat het exemplaar van de rol Windows Server 2008 wordt hebben op is geïnstalleerd (IIS wordt niet geïnstalleerd).

U kunt zeker maken en implementeren van een werknemer rol waarop een andere manier luistert voor binnenkomende verzoeken (bijvoorbeeld uw code kan maken en gebruiken van een HttpListener .NET). Aangezien de instanties van de rol worden alle Windows Server 2008 uitgevoerd, kan uw code bewerkingen die normaal gesproken beschikbaar zijn voor een toepassing wordt uitgevoerd op Windows Server uitvoeren
2008.

Voor elke rol, geeft u aan de gewenste VM grootte die exemplaren van deze functie moeten gebruiken. De onderstaande tabel ziet u de verschillende beschikbare vandaag VM-grootte en de kenmerken van elk:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**VM grootte**

</td>
<td>
**CPU**

</td>
<td>
**RAM**

</td>
<td>
**Schijfopruiming**

</td>
<td>
**Piek netwerk i/o-**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Extra klein**

</td>
<td>
1 x 1,0 GHz

</td>
<td>
768 MB

</td>
<td>
20GB

</td>
<td>
\~5 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Kleine**

</td>
<td>
1 x 1,6 GHz

</td>
<td>
1,75 GB

</td>
<td>
225GB

</td>
<td>
\~100 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Gemiddeld**

</td>
<td>
2 x 1,6 GHz

</td>
<td>
3,5 GB

</td>
<td>
490GB

</td>
<td>
\~200 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Grote**

</td>
<td>
4 x 1,6 GHz

</td>
<td>
7 GB

</td>
<td>
1TB

</td>
<td>
\~400 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Extra groot**

</td>
<td>
8 x 1,6 GHz

</td>
<td>
14 GB

</td>
<td>
2TB

</td>
<td>
\~800 Mbps

</td>
</tr>
</tbody>
</table>
U wordt geheven per uur voor elke VM u als een exemplaar van de rol en wordt ook geheven voor alle gegevens dat uw rol-sessies buiten het datacenter verzenden. U geen btw geheven voor gegevens invoeren van het datacenter. Zie voor meer informatie [Azure prijzen] []. In het algemeen, is het raadzaam te veel exemplaren van de kleine rol in plaats van een paar grote exemplaren gebruiken zodat u uw toepassing beter bestand is mislukt. Nadat alle, de minder exemplaren van de rol die u hebt, meer desastreuze een fout in een van deze is in uw algemene toepassing. Zoals eerder, moet u ten minste twee instanties voor elke rol implementeren om te krijgen van de 99.95% serviceovereenkomst die Microsoft biedt.

De service-definitiebestand (CSDEF) is ook waar u veel kenmerken over elke rol in uw toepassing wilt opgeven. Hier volgen enkele van de gebruiksvriendelijker items die u beschikbaar:

-   **Certificaten**. U certificaten gebruiken voor het coderen van gegevens of als uw webservice SSL ondersteunt. Alle certificaten moeten worden geüpload naar Azure. Zie voor meer informatie [Certificaten beheren in Azure wordt aangegeven][]. Deze instelling XML-installeert eerder geüpload certificaten bij het exemplaar van de rol certificaat winkel, zodat ze gebruikt door uw toepassingscode worden.

-   **Namen van de configuratie-instelling**. Voor waarden die u wilt dat uw toepassingen te lezen terwijl u op een exemplaar van de rol uitvoert. De werkelijke waarde van de configuratie-instellingen is ingesteld in de service (CSCFG) configuratiebestand die kan worden bijgewerkt op elk gewenst moment zonder dat u opnieuw te implementeren van uw code. Ja, kunt u uw toepassingen zodanig de gewijzigde configuratiewaarden vinden zonder eventuele downtime code.

-   **Input eindpunten**. Hier geeft u een HTTP, HTTPS of TCP eindpunten (met poorten) die u wilt laten zien voor het externe netwerk via uw *voorvoegsel*. cloadapp.net-URL. Wanneer Azure uw rol implementeert, wordt deze automatisch de firewall configureren voor het exemplaar van de rol.

-   **Interne eindpunten**. Hier geeft u een HTTP- of TCP eindpunten gewenste blootgesteld aan andere rol-exemplaren die zijn geïmplementeerd als onderdeel van uw toepassing. Interne eindpunten ervoor dat alle exemplaren van een rol binnen de toepassing contact opnemen met elkaar, maar zijn niet toegankelijk zijn voor alle exemplaren van de rol die zich buiten uw toepassing.

-   **Importeren Modules**. Deze installeren desgewenst handig onderdelen op uw rol exemplaren. Onderdelen bestaan voor diagnostische monitoring, extern bureaublad en Azure verbindt (waarmee uw exemplaar van de rol toegang tot lokale bronnen via een beveiligde verbinding).

-   **Lokale opslag**. Hiermee wordt een submap op het exemplaar van de rol voor uw toepassing toegewezen. Dit wordt in het artikel [Gegevens opslag aanbiedingen in Azure][] uitgebreider beschreven.

-   **Opstarttaken**. Opstarttaken kunnen u een vereiste onderdelen installeren op een exemplaar van de rol, zoals deze wordt gestart. De taken kunnen uitvoeren met verhoogde bevoegdheid als beheerder indien nodig.

## <a id="cfg"> </a>Het configuratiebestand Service

De service-configuratiebestand (CSCFG) is een XML-bestand waarmee instellingen die kunnen worden gewijzigd zonder opnieuw te implementeren uw toepassing wordt beschreven. Het volledige schema voor het XML-bestand vindt u hier: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
Het bestand CSCFG bevat een element rol voor elke rol in uw toepassing. Hier volgen enkele van de items die u in het bestand CSCFG opgeven kunt:

-   **Versie van het besturingssysteem**. Dit kenmerk kunt u het besturingssysteem (OS)-versie dat u wilt dat die wordt gebruikt voor de rol overal uitvoering van de toepassing programmacode te selecteren. Deze OS is bekend als *Gast OS*en elke nieuwe versie bevat de meest recente beveiligings-patches en er zijn updates beschikbaar op het moment dat de Gast OS wordt uitgebracht. Als u de kenmerkwaarde Versie_besturing ingesteld op "\*', en vervolgens de Gast OS op elk van uw rol-sessies Azure automatisch bijgewerkt wanneer er nieuwe Gast OS versies beschikbaar komen. U kunt echter afmelden bij automatische updates door te selecteren een specifieke Gast OS versie. Als u bijvoorbeeld het kenmerk Versie_besturing op een waarde van "WA-GAST-OS-2,8\_201109-01 ' zorgt ervoor dat uw rol overal om wat wordt beschreven op deze pagina met webonderdelen: [http://msdn.microsoft.com/library/hh560567.aspx][]. Zie voor meer informatie over Gast OS versies, [Upgrades voor het besturingssysteem van Azure gasten beheren].

-   **Exemplaren**. De waarde van dit element geeft het aantal exemplaren van rol gewenste ingerichte uitvoeren van de code voor een bepaalde rol. Aangezien u kunt een nieuw CSCFG-bestand uploaden naar Azure (zonder opnieuw te implementeren in uw toepassing), is het gebruikelijke manier eenvoudig wijzigt u de waarde voor dit element en uploadt u een nieuw bestand CSCFG dynamisch vergroten of verkleinen van het aantal exemplaren van rol uitvoering van de toepassing programmacode. Hiermee kunt u eenvoudig uw toepassing schaal omhoog of omlaag naar de vereisten voldoen aan bepaalde werkelijke werkbelasting terwijl ook bepalen hoeveel wordt geheven voor het uitvoeren van de exemplaren van de rol.

-   **Waarden voor de configuratie-instellingen**. Met dit element geeft aan dat waarden voor instellingen (zoals gedefinieerd in het bestand CSDEF). Uw rol kan deze waarden lezen terwijl deze wordt uitgevoerd. De waarden van deze configuratie-instellingen worden meestal gebruikt voor verbindingstekenreeksen met de met SQL-Database of op Azure opslag, maar ze kunnen worden gebruikt voor doeleinden gewenste.

## <a id="hostedservices"> </a>Maken en implementeren van een gehoste Service

Een gehoste service maken, moet u eerst gaat u naar de [Beheerportal van Azure] en inrichten van een gehoste service door het opgeven van een voorvoegsel DNS- en de Datacenter uiteindelijk uw code die wordt uitgevoerd gewenste in. Klik in uw omgeving ontwikkeling, die u kunt maken van uw service-definitiebestand (CSDEF), maken van uw toepassingscode en pakket (zip) alle volgende bestanden naar een service-pakketbestand (CSPKG). Het configuratiebestand van service (CSCFG), moet u ook voorbereiden. Als u wilt uw rol implementeren, moet u de CSPKG en CSCFG-bestanden met de API van Azure-Service Management uploaden. Zodra geïmplementeerd, Azure wordt inrichten rol instanties in het datacenter (op basis van de configuratiegegevens), uw toepassingscode extraheren uit het pakket, kopiëren naar de rol-exemplaren en de exemplaren opstarten. Nu is uw code actief.

De onderstaande afbeelding ziet u de CSPKG en CSCFG-bestanden die u op uw computer ontwikkeling maakt. Het bestand CSPKG bevat het bestand CSDEF en de code voor twee rollen. Azure Hiermee na het uploaden van de CSPKG en CSCFG-bestanden met de API van Azure-Service Management, maakt u de rol exemplaren in het midden van de gegevens. In dit voorbeeld in het bestand CSCFG aangegeven dat Azure drie exemplaren van de rol moet maken \#1 en twee instanties van de rol \#2.

![afbeelding][5]

Zie het artikel [distribueren en bijwerken van Azure-toepassingen][] voor meer informatie over het implementeren van een upgrade, en uw rollen, opnieuw te configureren.<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Verwijzingen

-   [Een gehoste Service voor Azure maken][]

-   [Gehoste Services in Azure beheren][]

-   [Migreren Azure-toepassingen][]

-   [Azure-toepassingen configureren][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Geschreven Jeffrey Richter (Wintellect)</p>

</div>

  [Voordelen van Azure-toepassing Model]: #benefits
  [Gehoste Service Core concepten]: #concepts
  [Overwegingen bij het ontwerpen van gehoste Service]: #considerations
  [Ontwerpen van uw toepassing voor schaal]: #scale
  [Definitie van gehoste Service- en configuratieservices]: #defandcfg
  [De definitie servicebestand]: #def
  [Het configuratiebestand Service]: #cfg
  [Maken en implementeren van een gehoste Service]: #hostedservices
  [Verwijzingen]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [Een aangepaste domeinnaam configureren in Azure wordt aangegeven]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Gegevens opslag aanbiedingen in Azure wordt aangegeven]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Beheer van certificaten in Azure wordt aangegeven]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://msdn.Microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://msdn.Microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [Upgrades naar de Azure gasten OS beheren]: http://msdn.microsoft.com/library/ee924680.aspx
  [Azure Management Portal]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Distribueren en bijwerken van Azure-toepassingen]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Een gehoste Service voor Azure maken]: http://msdn.microsoft.com/library/gg432967.aspx
  [Gehoste Services in Azure beheren]: http://msdn.microsoft.com/library/gg433038.aspx
  [Migreren Azure-toepassingen]: http://msdn.microsoft.com/library/gg186051.aspx
  [Azure-toepassingen configureren]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
