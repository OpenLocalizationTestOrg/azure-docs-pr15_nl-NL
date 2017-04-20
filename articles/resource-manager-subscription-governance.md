<properties
   pageTitle="Aanbevolen procedures voor ondernemingen verplaatsen naar Azure | Microsoft Azure"
   description="Beschrijving van een scaffold die ondernemingen gebruiken kunnen om ervoor te zorgen een veilige en beheerbare-omgeving."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure enterprise scaffold - duidelijke abonnement beheermodel

Ondernemingen worden steeds gesteld door de openbare cloud voor de flexibiliteit en flexibiliteit. Ze van de cloud strengths om te genereren inkomsten of optimaliseren van resources voor het bedrijf gebruikt. Microsoft Azure biedt een groot aantal diensten dat ondernemingen zoals bouwstenen om op te lossen tal van werkbelasting en toepassingen kunnen samenstellen. 

Maar, weten waar u wilt beginnen, is dikwijls moeilijk. Na het bepalen van de Azure gebruikt om, enkele vragen die vaak zich voordoen:

- "Hoe ik onze juridische vereisten voor de gegevens te garanderen in sommige landen vergaderen?"
- "Hoe kan ik ervoor zorgen dat iemand niet per ongeluk een kritieke systeem verandert?"
- "Hoe weet ik wat elke resource ondersteunt zodat ik kunt account voor deze en bol. dat alleen terug nauwkeurig?"

Het uitvoeren van een lege-abonnement met geen rails beveiliging is lastige. In dit lege ruimte kunt nadelig zijn voor de verplaatsing naar Azure.

In dit artikel vindt een beginpunt voor technische professionals naar adres dat u een beheermodel en deze overeenkomen met de behoefte aan flexibiliteit. Hiermee wordt het concept van een enterprise-scaffold die organisaties helpt in uitvoering en beheer van hun Azure abonnementen ingevoerd. 

## <a name="need-for-governance"></a>Dat u een beheermodel

Wanneer u deze verplaatst naar Azure, moet u het onderwerp van beheermodel vroeg om ervoor te zorgen de succesvol gebruik van de cloud binnen de onderneming beantwoorden. Helaas, de tijd en bureaucracy voor het maken van een systeem volledig beheermodel betekent dat sommige bedrijven groepen Ga rechtstreeks naar leveranciers zonder tussenkomst enterprise IT. Deze methode kunt laat de onderneming openen op problemen als de bronnen niet goed worden beheerd. De kenmerken van de openbare cloud - flexibiliteit, flexibiliteit en verbruik gebaseerde prijzen - zijn belangrijk voor bedrijven-groepen die moeten voldoen snel aan de vereisten van klanten (interne en externe). Maar, enterprise IT nodig heeft om ervoor te zorgen dat gegevens en systemen effectief zijn beveiligd.

In de praktijk steigers gebruikt voor het maken van de basis van de structuur. De scaffold het algemene overzicht handleidingen en bevat ankerpunten voor meer permanente systems te koppelen. Een enterprise-scaffold is dezelfde: een set flexibele besturingselementen en Azure mogelijkheden die structuur naar de omgeving en ankers voor services is gebaseerd op de openbare cloud geven. Biedt de kiezen (IT en bedrijven groepen) een foundation maken en toevoegen van nieuwe services.

De scaffold is gebaseerd op procedures die we hebt verzameld uit veel afspraken met clients van verschillende groottes. Het bereik van deze clients in kleine organisaties ontwikkelen van oplossingen in de cloud Fortune 500 ondernemingen en softwareleveranciers van onafhankelijke die zijn migreren en ontwikkelen van oplossingen in de cloud. De enterprise-scaffold is 'legitieme"flexibel ter ondersteuning van traditionele IT-werkbelastingen zowel agile werkbelasting; zoals, is de ontwikkelaars software-als-een-service (SaaS)-toepassingen maken op basis van Azure mogelijkheden staat.

De enterprise-scaffold is bedoeld als het fundament van elke nieuwe abonnement binnen Azure. Hiermee kunnen beheerders om ervoor te zorgen werkbelasting voldoen aan de minimale beheermodel van een organisatie zonder verhinderen dat bedrijven groepen en ontwikkelaars snel hun eigen doelstellingen.

> [AZURE.IMPORTANT] Beheermodel is essentieel voor het succes van Azure. In dit artikel is bedoeld voor de technische uitvoering van een enterprise-scaffold, maar alleen raakt op de bredere proces en relaties tussen de onderdelen. Beleid beheermodel van boven naar beneden loopt en wordt bepaald door wat het bedrijf wil bereiken. Natuurlijk het maken van een beheermodel voor Azure vertegenwoordigers van bevat IT, maar belangrijker het sterke weergave uit groep leidinggevenden en beveiliging en risicobeheer moet hebben. Uiteindelijk gaat een enterprise-scaffold over het risico bedrijven om de opdracht en doelstellingen van de organisatie te vergemakkelijken beperken.

De volgende afbeelding worden de onderdelen van de scaffold. Het fundament, is afhankelijk van een effen plan voor afdelingen, accounts en abonnementen. De stijlen bestaan uit resourcemanager beleidsregels en sterke naming standaarden. De rest van scaffold afkomstig zijn uit core Azure mogelijkheden en functies die een veilige en beheerbare-omgeving.

![scaffold onderdelen](./media/resource-manager-subscription-governance/components.png)

> [AZURE.NOTE] Azure is snel gegroeid sinds de introductie in 2008. Deze groei vereist Microsoft engineering teams na te denken over hun methode voor het beheren en implementeren van services. Het model Azure resourcemanager is geïntroduceerd in 2014 en Hiermee vervangt u het implementatiemodel klassieke. Resourcemanager kunnen organisaties eenvoudiger implementeren, te organiseren en beheren van Azure resources. Resourcemanager bevat parallelization bij het maken van resources voor snellere implementatie van complexe, onderling afhankelijke oplossingen. Het bevat ook gedetailleerde toegangsbeheer en de mogelijkheid om te tag resources met metagegevens. Microsoft raadt u alle resources via het objectmodel resourcemanager te maken. De enterprise-scaffold is expliciet bedoeld voor de Resource Manager-model.

## <a name="define-your-hierarchy"></a>De hiërarchie definiëren

Het fundament van de scaffold wordt de registratie van Azure Enterprise (en de Enterprise-Portal). De registratie enterprise definieert de vorm en gebruiken van Azure services binnen een bedrijf is de structuur van de beheermodel core. In de enterprise agreement kunnen klanten verder onderverdelen de omgeving in afdelingen, accounts en ten slotte abonnementen. Een Azure-abonnement is de basiseenheid waarin alle resources voorkomen. Deze definieert ook diverse limieten binnen Azure, zoals het aantal cores, resources, enzovoort.

![hiërarchie](./media/resource-manager-subscription-governance/agreement.png)

Elke enterprise verschilt en de hiërarchie in de vorige afbeelding kan voor aanzienlijke flexibiliteit voor hoe Azure binnen het bedrijf is ingedeeld. Voordat u de richtlijnen in dit document bevat implementeert, moet u de hiërarchie model en begrijpen wat de gevolgen op facturering, toegang tot bronnen en complexiteit.

De drie algemene patronen voor Azure inschrijvingen zijn:

- Het **functionele** patroon

    ![functionele](./media/resource-manager-subscription-governance/functional.png)

- Het patroon dat in **business-eenheid** 

    ![bedrijven](./media/resource-manager-subscription-governance/business.png)

- De **geografische** patroon

    ![geografische](./media/resource-manager-subscription-governance/geographic.png)

U toepassen de scaffold op het niveau van het abonnement voor het uitbreiden van het beheermodel vereisten van de onderneming in het abonnement.

## <a name="naming-standards"></a>Naamgevingsvereisten

De eerste pijlers van de scaffold is naamgevingsvereisten. Ontwerpen naming standaarden kunnen u bronnen in de portal op een factuur en binnen scripts identificeren. Waarschijnlijk, hebt u al naamgevingsvereisten voor on-premises-infrastructuur. Bij het toevoegen van Azure in uw omgeving, moet u deze naming normen uitbreiden naar uw Azure bronnen. Efficiënter beheer van de omgeving op alle niveaus Naming standaard vergemakkelijken.

> [AZURE.TIP]
>
> - Controleer en bij het gebruik van waar u mogelijk de [patronen en procedures richtlijnen](./guidance/guidance-naming-conventions.md). Deze instructies kunt u bepalen op een zinvolle naming standaard.
> - Gebruik camelCasing voor van resourcenamen (zoals myResourceGroup en vnetNetworkName). Opmerking: Er zijn bepaalde resources, zoals accounts, opslag, waarbij de enige optie gebruik van kleine letters (en geen andere speciale tekens).
> - Kunt u met Azure resourcemanager beleid (beschreven in het volgende gedeelte) naming standaarden afdwingen.
 
## <a name="policies-and-auditing"></a>Beleidsregels en controle

De tweede pijlers van de scaffold heeft betrekking op [Azure resourcemanager beleidsregels](resource-manager-policy.md) en [controle van het logboek](resource-group-audit.md)maken. Resourcemanager beleidsregels bieden u de mogelijkheid voor het beheren van risico's in Azure. U kunt beleidsregels waarmee gegevens te garanderen zorgen door te beperken, afdwingen of bepaalde acties controle definiëren. 

- Beleid is een systeem voor het **toestaan** van standaard. U kunt acties bepalen door te definiëren en beleid toewijzen aan resources die weigeren of acties op resources wilt controleren.
- Beleidsregels worden beschreven door beleidsdefinities in een beleid definitie taal (als-dan-voorwaarden).
- U maakt beleid met JSON (Javascript-Object notatie) opgemaakte bestanden. Nadat u een beleid definieert, hebt u dit toewijzen aan een bepaald bereik: abonnement, resourcegroep. of resource.

Beleidsregels hebben meerdere acties waarmee een fijnmazige aanpak uw scenario. De acties zijn:

-   **Weigeren**: het resourceverzoek blokkeert
-   **Controle**: kunnen aanvragen, maar wordt een lijn toegevoegd aan het gebeurtenissenlogboek (dat kan worden gebruikt om te leveren waarschuwingen of voor het starten van runbooks)
-   **Toevoegen**: worden opgegeven gegevens toegevoegd aan de resource. Als er een tag "CostCenter" op een resource is, bijvoorbeeld deze tag wordt gebruikt met een standaardwaarde toevoegen.

### <a name="common-uses-of-resource-manager-policies"></a>Gangbare toepassingen van beleidsregels voor resourcemanager

Azure resourcemanager beleid is een krachtige hulpmiddel in de Azure toolkit. Hiermee kunt u voorkomen dat onverwachte kosten, om aan te geven van een kostenplaats voor resources tot en met labelen en om ervoor te zorgen dat in overeenstemming vereisten wordt voldaan. Wanneer het beleid worden gecombineerd met de ingebouwde controlefuncties, kunt u complexe en flexibele oplossingen fashion. Beleidsregels voor toestaan bedrijven die besturingselementen voor werkbelasting "Traditionele IT" en "Agile" werkbelasting; klanttoepassingen, zoals ontwikkelen. De meest voorkomende patronen we voor beleidsregels zien zijn:

- **Geografische-naleving/gegevens te garanderen** - Azure biedt regio's overal ter wereld. Ondernemingen wil vaak bepalen waar resources die zijn gemaakt (of om ervoor te zorgen gegevens te garanderen, of gewoon om ervoor te zorgen resources dicht bij de consumenten einde van de resources worden gemaakt).
- **Kostenbeheer** - abonnement op een Azure bronnen van veel typen en schaal kan bevatten. Bedrijven wilt vaak om ervoor te zorgen dat standaard abonnementen vermijden onnodig groot gebruiken, zodat u honderden dollars een maand of langer kost.
- **Standaard beheermodel door vereiste tags** - vereisen van labels, is een van de meest voorkomende en ten zeerste gewenste functies. Werken met Azure resourcemanager beleid kunnen ondernemingen om ervoor te zorgen dat een resource correct is gelabeld. De meest voorkomende tags zijn: afdeling, de eigenaar van de Resource en omgeving type (bijvoorbeeld - productie, testen en ontwikkeling)

**Voorbeelden**

"Traditionele IT" abonnement voor LOB-toepassingen

-   Afdeling en eigenaar labels voor alle resources afdwingen
-   Het maken van de resource de regio Noord-Amerikaanse beperken
-   De mogelijkheid om te maken, G-serie VMs en HDInsight Clusters beperken

"Agile" omgeving voor een zakelijke eenheid cloud-toepassingen maken

- Toestaan dat het maken van resources om te voldoen aan vereisten voor de gegevens te garanderen, alleen in een bepaalde regio.
- Afdwingen omgeving tag voor alle resources. Als u een resource is gemaakt zonder een tag, toevoegen de **omgeving: onbekend** label aan de resource.
- Audit wanneer resources worden gemaakt buiten Noord-Amerika, maar niet.
- Audit wanneer hoog-kosten voor resources worden gemaakt.

> [AZURE.TIP]
>
> De meest voorkomende benut resourcemanager beleidsregels door organisaties is om te bepalen *waar* resources kunnen worden gemaakt en *welke* typen resources kunnen worden gemaakt. Naast besturingselementen op *waar* en *welke*biedt veel ondernemingen beleid gebruiken om ervoor te zorgen resources moeten u de juiste metagegevens terug in verbruik rekening. Wij raden aan om beleid op het niveau van het abonnement voor:
>
> - Geografische-naleving/gegevens te garanderen
> - Kosten beheren
> - Vereiste labels (bepaald door de zakelijke behoeften, zoals BillTo, de eigenaar van toepassing)
>
> U kunt extra beleidsregels op lagere niveaus van reikwijdte toepassen.
 
### <a name="audit---what-happened"></a>Controle - wat is er gebeurd?

Als u wilt bekijken hoe uw omgeving werkt, moet u gebruikersactiviteit controleren. De meeste resourcetypen binnen Azure maken diagnostische logboeken die u via een hulpmiddel log of in Azure bewerkingen Management-Suite analyseren kunt. U kunt de activiteitenlogboeken verzamelen over meerdere abonnementen op te geven van een afdeling of de enterprise-weergave. Controlerecords zijn zowel een belangrijke diagnostisch hulpprogramma en een essentieel om trigger gebeurtenissen in de Azure-omgeving.

Activiteitenlogboeken aan de van resourcemanager implementaties kunnen u bepalen van de **bewerkingen** die u hebt gemaakt, plaats en wie deze uitgevoerd. Activiteitenlogboeken kunnen worden verzameld en samengevoegd met hulpprogramma's zoals Log Analytics.

## <a name="resource-tags"></a>Resource-labels

Als gebruikers in uw organisatie een resource aan het abonnement toevoegt, wordt het steeds belangrijker resources koppelen aan de juiste afdeling, klanten en -omgeving. U kunt de metagegevens koppelen aan resources door [tags](resource-group-using-tags.md). U markeringen gebruiken om te bevatten informatie over de resource of de eigenaar bent. Labels kunnen u niet alleen aggregeren en resources op verschillende manieren groeperen, maar deze gegevens gebruiken voor de toepassing van financiële. U kunt resources met maximaal 15 sleutel: waardeparen labelen. 

Labels van de resource is flexibel en moeten worden gekoppeld aan de meeste resources. Voorbeelden van veelgebruikte resource-codes zijn:

- BillTo
- Afdeling (of bedrijven eenheid)
- Omgeving (productie, fase, ontwikkeling)
- Laag (weblaag, toepassingslaag)
- De eigenaar van de toepassing
- ProjectName

![labels](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Zie voor meer voorbeelden van labels [aanbevolen naamgevingsconventies voor Azure resources](./guidance/guidance-naming-conventions.md).

> [AZURE.TIP]
>
> Maak een sociaalnetwerklabels strategie die aangeeft in uw abonnementen welke metagegevens nodig is voor het bedrijf, financiën, beveiliging, risicobeheer en algemeen beheer van de omgeving. Het is raadzaam een beleid die labelen verplicht voor:
>
> - Resourcegroepen
> - Opslag
> - Virtuele Machines
> - Toepassing Service omgevingen/endwebservers

## <a name="resource-group"></a>Resourcegroep 

Resourcemanager kunt u besteedt resources zinvolle groepen voor beheer factuuradres of natuurlijke affiniteit. Zoals eerder is vermeld, heeft Azure twee implementatiemodellen. In het eerdere klassieke gegevensmodel is de basiseenheid beheer van het abonnement. Het is moeilijk te Splits op bronnen binnen een abonnement, waardoor het maken van grote aantallen abonnementen. Met het model resourcemanager gezien we de introductie van resourcegroepen. Resourcegroepen zijn containers voor resources die beschikt over de levenscyclus van een algemene of delen van een kenmerk zoals "alle SQL Server" of 'Toepassing A'.

Resourcegroepen kunnen niet worden opgenomen in elkaar en resources kunnen alleen behoren aan één resourcegroep. U kunt bepaalde acties toepassen op alle resources in een resourcegroep. Bijvoorbeeld verwijdert een resourcegroep alle bronnen binnen de resourcegroep. Meestal, schakelt u een hele toepassing of gerelateerde systeem in dezelfde resourcegroep. Een toepassing drie niveaus met de naam van Contoso-webtoepassing bevat bijvoorbeeld de webserver, toepassingsserver en SQL server in dezelfde resourcegroep.

> [AZURE.TIP]
>
> Hoe u uw resourcegroepen organiseren kan afwijken van werkbelasting 'Traditionele IT' naar "Agile IT" werkbelasting
>
> - "Traditionele IT" werkbelasting zijn meestal gegroepeerd op items binnen de levenscyclus van dezelfde, zoals een toepassing. Groepering door toepassing kan voor afzonderlijke Toepassingsbeheer.
> - "Agile IT" werkbelasting meestal kunt richten op externe klant is gericht cloud-toepassingen. De resourcegroepen moeten overeenkomen met de lagen van implementatie (zoals weblaag, App laag) en beheer.

## <a name="role-based-access-control"></a>Toegangsbeheer op basis van rollen

Waarschijnlijk vraagt u zelf "wie moet er toegang tot bronnen?" en "Hoe beheer ik deze toegang uit?" Toestaan of verbieden toegang tot de portal van Azure en toegang tot de resources in de portal beheren is essentieel. 

Wanneer Azure in eerste instantie uitgebracht is, toegang tot besturingselementen op een abonnement eenvoudige zijn: beheerder of beheerder van collega. Toegang tot een abonnement in de klassieke model impliciete toegang tot alle bronnen in de portal. Het gebrek aan fijnmazige besturingselement tot de verspreiding van abonnementen een niveau redelijk toegangsbeheer verstrekken voor een registratie Azure hebben geleid.

De verspreiding van abonnementen is niet meer nodig. U kunt met Rolgebaseerd toegangsbeheer gebruikers toewijzen aan standaardrollen (zoals 'Lezer' en "schrijver" voorkomende rollen). U kunt ook aangepaste rollen definiëren.

> [AZURE.TIP]
>  
> - Uw zakelijke identiteit store (meestal Active Directory) verbinden met Azure Active Directory met de AD Connect-functie.
> - De beheerder/Co-beheerders van een abonnement met een beheerde identiteit bepalen. **Geen** toewijzen beheerder/Co-beheerders aan de eigenaar van een nieuw abonnement. Gebruik in plaats daarvan RBAC rollen **eigenaar** rechten wilt aan een groep of een afzonderlijke.
> - Azure gebruikers toevoegen aan een groep (bijvoorbeeld toepassing X eigenaren) in Active Directory. Gebruik de gesynchroniseerde groep te leveren leden van de juiste machtigingen voor het beheren van het resourceveld groep met de toepassing.
> - Volg het principe van het toekennen van het **laagste bevoegdheidsniveau** vereist om de verwachte werkzaamheden uitvoeren. Bijvoorbeeld:
>     - Implementatie-groep: Een groep die zich alleen voor de implementatie van resources.
>     - VM Management: Een groep waarvoor u kan VMs (voor bewerkingen) opnieuw starten

## <a name="azure-resource-locks"></a>Azure resource vergrendelingen

Als uw organisatie wordt core services toegevoegd aan het abonnement, wordt het steeds belangrijker om ervoor te zorgen dat deze services beschikbaar zijn voor bedrijven-onderbreking voorkomen. [Resource-vergrendelingen](resource-group-lock-resources.md) kunt u bewerkingen op hoogwaardige resources waarvoor wijzigt of verwijdert deze belangrijke gevolgen voor uw toepassingen of cloudinfrastructuur zou hebben beperken. U kunt vergrendelingen toepassen op een abonnement, resourcegroep of resource. U kunt meestal vergrendelingen toepassen op moeten beschikken over resources zoals virtuele netwerken, gateways en opslag-accounts. 

Resource-vergrendelingen twee waarden voor het momenteel ondersteund: CanNotDelete als alleen-lezen. CanNotDelete houdt in dat gebruikers (met de juiste machtigingen) kunnen wel lezen of het wijzigen van een resource, maar deze niet verwijderen. Alleen-lezen betekent dat bevoegde gebruikers niet verwijderen of wijzigen van een resource.

Als u wilt maken of verwijderen van management vergrendelingen, hebt u toegang tot `Microsoft.Authorization/*` of `Microsoft.Authorization/locks/*` acties.
Van de ingebouwde functies, worden alleen de eigenaar en de gebruiker toegang beheerder deze acties verleend.

> [AZURE.TIP] Opties voor het netwerk van Core moeten worden beveiligd met vergrendelingen. Onbedoeld verwijderen van een gateway, de site-naar-site VPN zou desastreuze bij een Azure-abonnement. Azure niet is toegestaan om te verwijderen van een virtueel netwerk wordt gebruikt, maar toepassing meer beperkingen is een handige voorzorgsmaatregelen. Beleidsregels zijn ook essentieel voor het onderhoud van de juiste besturingselementen. Het is raadzaam een **CanNotDelete** vergrendelen aan het beleid die worden gebruikt toe te passen.
>
> - Virtual Network: CanNotDelete
> - Beveiligingsgroep netwerk: CanNotDelete
> - Beleid: CanNotDelete

## <a name="core-networking-resources"></a>Belangrijkste netwerken resources

Toegang tot bronnen kunt zijn interne (binnen het bedrijfsnetwerk) of externe (via internet). Het is makkelijk voor gebruikers in uw organisatie aan resources in de verkeerde plaats per ongeluk hebt opgeslagen en opent u deze mogelijk naar schadelijke access. Als u met de on-premises apparaten, moeten ondernemingen juiste besturingselementen om ervoor te zorgen dat de juiste besluiten te nemen Azure gebruikers toevoegen. Voor abonnement governance, we core resources identificeren die eenvoudige beheer van access biedt. De belangrijkste resources bestaan uit:

- **Virtuele netwerken** zijn containerobjecten voor subnetten. Hoewel niet strikt noodzakelijk is, wordt het vaak gebruikt wanneer u verbinding maakt interne bedrijfsresources-toepassingen.
- **Beveiligingsgroepen netwerk** zijn vergelijkbaar met een firewall en regels van hoe een resource kunt "overleggen" voorzien via het netwerk. Ze kunnen u gedetailleerde bepalen hoe / als een subnet (of VM) verbinding met Internet of andere subnetten in hetzelfde virtuele netwerk maken kunt.

![Core netwerken](./media/resource-manager-subscription-governance/core-network.png)

> [AZURE.TIP]
>  
> - Virtuele netwerken die voor externe werkbelasting en interne werkbelasting maken. Deze methode wordt de kans per ongeluk plaatsen virtuele machines die zijn bedoeld voor interne werkbelasting in een externe omlaag ruimte kleiner.
> - Beveiligingsgroepen netwerk zijn essentieel voor deze configuratie. Ten minste access blokkeren met internet, interne virtuele netwerken en toegang met het bedrijfsnetwerk blokkeren van externe netwerken van virtuele.

### <a name="automation"></a>Automatisering

Beide tijdrovende afzonderlijk bronnen beheren is en mogelijk fout vatbaar voor bepaalde bewerkingen. Azure vindt u verschillende mogelijkheden voor het automatiseren zoals Azure automatisering, logica Apps en Azure-functies. [Azure automatisering](./automation/automation-intro.md) kunnen beheerders maken en runbooks voor het verwerken veelvoorkomende taken bij het beheren van resources te definiëren. U maken runbooks met behulp van een PowerShell-code-editor of een grafische-editor. U kunt complexe met meerdere fasen werkstromen produceren. Azure automatisering wordt vaak gebruikt om algemene taken zoals ongebruikte resources afgesloten of het maken van resources in antwoord op een specifieke trigger zonder menselijke tussenkomst verwerken.

> [AZURE.TIP]
>
> - Maak een automatisering Azure-account en controleer de beschikbare runbooks (grafische en command lijn) beschikbaar in de [Galerie met Runbook](./automation/automation-runbook-gallery.md).
> - Importeren en aanpassen van belangrijke runbooks voor eigen gebruik.
>
> Een algemene scenario is de mogelijkheid om te starten en afsluiten virtuele machines volgens een schema. Zijn er voorbeeld runbooks die beschikbaar zijn in de galerie die zowel verwerken van dit scenario en leert u hoe u uitvouwen.


## <a name="azure-security-center"></a>Azure Beveiligingscentrum

Misschien is een van de grootste upblokkering naar cloud adoptie de bezwaren over beveiliging. IT risicomanagers en beveiliging afdelingen nodig om ervoor te zorgen dat resources in Azure wordt aangegeven beveiligd zijn. 

Het [Azure Beveiligingscentrum](./security-center/security-center-intro.md) vindt u een centrale weergave van de beveiligingsstatus van resources in de abonnementen en aanbevelingen die helpen voorkomen dat beschadigde resources. Deze kunt meer gedetailleerde beleidsregels (bijvoorbeeld toepassen beleidsregels aan specifieke resourcegroepen waarmee de onderneming te hun houding om het risico dat wordt voldaan aan te passen) inschakelen. Tot slot is Azure Beveiligingscentrum een open platform waarmee Microsoft-partners en onafhankelijke leveranciers software die u in de Beveiligingscentrum Azure ter verbetering van de mogelijkheden kunt maken. 

> [AZURE.TIP]
>
> Azure Beveiligingscentrum is standaard ingeschakeld in elk abonnement. Echter, moet u het verzamelen van virtuele machines toe te staan dat Azure Beveiligingscentrum voor het installeren van de agent en beginnen met het verzamelen van gegevens inschakelen.
>
> ![gegevens verzamelen](./media/resource-manager-subscription-governance/data-collection.png)

## <a name="next-steps"></a>Volgende stappen

- U hebt geleerd over abonnement beheermodel, is het tijd om te zien van deze aanbevelingen in Word Web App. Zie [voorbeelden van Azure abonnement beheermodel implementeren](resource-manager-subscription-examples.md).

*[Karel Kuhnhausen](https://github.com/karlkuhnhausen) bijgedragen aan de in dit onderwerp.*
