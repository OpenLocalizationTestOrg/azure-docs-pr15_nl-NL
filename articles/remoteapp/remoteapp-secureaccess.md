
<properties 
    pageTitle="Toegang tot Azure RemoteApp en voorbij beveiligen | Microsoft Azure"
    description="Leer hoe veilig toegang tot Azure RemoteApp met behulp van voorwaardelijke toegang in Azure Active Directory"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Toegang tot Azure RemoteApp en voorbij beveiligen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

In dit artikel krijgt we een overzicht van hoe een beheerder een kanaal beveiligde toegang vanaf de eindgebruiker via Azure RemoteApp en eindigt met een beveiligde resource zoals een SQL-database of een andere toepassing back-enddatabase kan instellen. Het doel is om ervoor te zorgen dat alleen gemachtigde gebruikers die voldoen aan de gewenste voorwaarden toegang hebben tot externe toepassingen en de beveiligde back-enddatabase kan alleen worden geopend vanaf de gecontroleerde Azure RemoteApp-omgeving en niet vanuit andere locaties.

Er zijn 3 hoofdgebieden die de beheerder nodig heeft om te bekijken:

![Azure RemoteApp voorwaardelijke toegang overwegingen](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Alleen op voor informatie en antwoorden op deze vragen.

## <a name="who-can-access-the-collection"></a>Wie toegang heeft tot de verzameling?
De beheerder kiest de gebruikers die toegang tot externe toepassingen in de siteverzameling. U kunt Azure Active Directory (Azure AD) werk- of schoolaccounts (eerder genoemd, "organisatieaccount")- of Microsoft-accounts (bijvoorbeeld @outlook.com). De meeste enterprise-scenario's Azure AD-accounts; gebruiken ze kunnen u voorwaardelijke access functies verderop beschreven en worden ook alleen de mogelijkheid voor een domein behoren verzamelingen gebruiken. De rest van het artikel wordt ervan uitgegaan dat u Azure AD-accounts gebruikt met Azure RemoteApp.

**Wat we hebben bereikt:**

Via van Azure AD-accounts toegang tot Azure RemoteApp ontstaat twee dingen:

1.  We altijd weet wie toegang heeft tot de toepassingen die we hebt gepubliceerd en toegang hebt tot alle back-uiteinden die toepassingen verbinden.
2.  We bepalen de onderliggende Azure AD zodat we maken kunnen en verwijderen gebruikersaccounts, beleid wachtwoord instelt, gebruikt u meervoudige verificatie, enzovoort. 

## <a name="how-is-the-collection-accessed-from-where"></a>Hoe wordt de verzameling geraadpleegd? Waarvandaan?
Beheerders willen vaak beleid voor het openen van een openbare internetverbinding-omgeving, zoals Azure RemoteApp definiëren. Bijvoorbeeld willen ze om ervoor te zorgen dat gebruikers toegang hebben tot de omgeving van buiten het bedrijfsnetwerk bevinden meervoudige verificatie (MFA) gebruiken moeten voor toegang; of misschien ze helemaal moeten worden geblokkeerd.

Azure RemoteApp-beheerders kunnen de functionaliteit voor beschikbaar via Azure AD Premium gebruiken voor het instellen van voorwaardelijke clienttoegangsbeleid voor hun Azure RemoteApp-omgeving. Ze kunnen ook rapporten met opmaak en functies waarschuwingen gebruiken om te controleren hoe de omgeving wordt geopend.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Het instellen van voorwaardelijke toegang voor Azure RemoteApp
We gaan te doorlopen een voorbeeldscenario – de Azure RemoteApp beheerder wil toegang geven tot de omgeving wanneer gebruikers buiten het bedrijfsnetwerk bevinden zijn.

>[AZURE.NOTE] We wordt ervan uitgegaan dat u hebt Azure AD bijgewerkt naar de laag Premium en dat u ten minste één Azure RemoteApp-siteverzameling hebt gemaakt.

1.  Klik op het tabblad **Active Directory** in Azure-portal. Klik vervolgens op de map die u wilt configureren.

    Onthouden: Voorwaardelijke toegang is een eigenschap van de map en niet van Azure RemoteApp, zodat alle configuratie is voltooid op mapniveau. Dit betekent ook moet u de beheerder van de map deze wijzigingen aanbrengen.

2.  Klik op **toepassingen**en klik vervolgens op **Microsoft Azure RemoteApp** voor het instellen van voorwaardelijke toegang. Houd er rekening mee dat u voorwaardelijke toegang voor elke toepassing 'software als service' in uw adreslijst afzonderlijk instellen kunt.
![Voorwaardelijke toegang in te stellen voor Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
 

3.  Klik op het tabblad **configureren** stelt u **Toegangsregels inschakelen** in op aan.
![Toegangsregels voor Azure RemoteApp inschakelen](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
 

4.  Nu kunt u verschillende regels configureren en kies wie om te laten passen:

    1. Kies **toegang als niet aan het werk blokkeren** , toe aan het volledig voorkomen dat gebruikers toegang hebben tot Azure RemoteApp buiten de netwerkomgeving die u opgeeft.
    2. Klik op de onderstaande optie voor het definiëren van de IP-adresbereiken die uw "vertrouwde netwerk' vormen. Alles buiten die gaan verloren.

5.  Test de configuratie door het starten van de Azure RemoteApp-client vanaf een IP-adres buiten het bereik dat u hebt opgegeven. Nadat u zich met uw referenties Azure AD aanmelden ziet u een bericht als volgt:

![Geweigerd toegang tot Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)
 

### <a name="future-conditional-access-features"></a>Functies voor toekomstige voorwaardelijke toegang 
Nieuwe mogelijkheden in voorwaardelijke Access werkt de Azure Active Directory-team. Beheerders kunnen maken van nieuwe soorten regels buiten netwerk op basis van locatieregels. Een openbare voorbeeld van de nieuwe functionaliteit moet binnenkort beschikbaar.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Het controleren van toegang tot Azure RemoteApp
Een handige functie gebruiken samen met voorwaardelijke toegang is rapportagefuncties Azure Active Directory Premium. U kunt rapporten controleren wie toegang heeft tot uw omgeving gebruiken en verdachte activiteiten detecteren.

Bijvoorbeeld, ziet u de namen van de gebruikers die Azure RemoteApp, hoe vaak ze deze hebt geopend en wanneer.

1.  In de portal van Azure **Active Directory**op en klik vervolgens op de map.

2.  Ga naar het tabblad **rapporten** .

3.  In de lijst met rapporten, selecteert u de **toepassingsgebruik** onder **geïntegreerde toepassingen**.

    Voor Azure RemoteApp ziet u enkele statistische gegevens. 
![Geaggregeerde Azure RemoteApp access stat](./media/remoteapp-secureaccess/ra-accessstats.png)
 
5.  Klik op de toepassing om informatie over gebruikers toegang hebben tot Azure RemoteApp zichtbaar te maken.
![Gebruiker toegang stat voor Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)
 
### <a name="summary"></a>Overzicht
U kunt met Azure Active Directory Premium toegangsregels voor een Azure RemoteApp (en andere software als een servicetoepassingen die beschikbaar zijn via Azure AD) instellen. Regels zijn momenteel alleen beschikbaar voor beleid voor locatie op basis van netwerken, maar wordt in de toekomst uitgebreid aan andere aspecten van enterprise management.

Azure AD Premium biedt ook rapportage en functies die verder het besturingselement de beheerder uitbreiden heeft via hun Azure RemoteApp-omgeving.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Hoe zorg ik ervoor dat mijn secure resource toegankelijk is alleen vanuit Mijn Azure RemoteApp-omgeving?
We gericht in vorige gedeelten van dit artikel over het beveiligen van toegang tot de Azure RemoteApp-omgeving. We hebben die bereikt in het kiezen van de gebruikers die toegang hebben en toegangsregels aan een besturingselement voor verdere instellen hoe ze de service kunnen gebruiken.

Een gebruikelijk voor Azure RemoteApp-implementaties is dat de externe toepassingen moeten communiceren met een back-enddatabase resource, bijvoorbeeld een SQL-database. Deze resource wordt gehost beide on-premises implementatie (bijvoorbeeld in een bedrijfsnetwerk) of in de cloud (bijvoorbeeld in Azure IaaS). Beheerders wilt vaak om ervoor te zorgen dat de back-enddatabase resource alleen door geïmplementeerd via Azure RemoteApp-toepassingen en bijvoorbeeld niet een toepassing uitvoeren rechtstreeks op een gebruikerswachtwoord PC en toegang tot via openbare Internet kan worden geopend. Azure RemoteApp is vaak gezien als de omgeving centraal beheerde en beveiligde en dus het alleen pad waarmee gebruikers moeten ermee aan de resource back-enddatabase.

De oplossing is om de plaats van zowel de Azure RemoteApp-omgeving en de secure resource in de dezelfde Azure virtuele netwerk (VNET). Als de bron in een andere site is, kunt u een VPN-verbinding van site-naar-site, bijvoorbeeld om af te maken van een VNet die in beslag nemen het Azure datacenter en de klant on-premises omgeving maken.

Azure RemoteApp ondersteunt twee soorten implementaties van de siteverzameling waarin u uw eigen VNET kunt geven:

-   Niet-domein behoren: de toepassingen 'lijn van CAS' van de andere bronnen in de VNET heeft. Bijvoorbeeld: dit kan worden gebruikt om verbinding maken met toepassingen met een SQL-database SQL-verificatie (toepassingen verificatie van de gebruiker rechtstreeks ten opzichte van de database)

-   Deelnemen aan het domein: de virtuele machines die worden gebruikt door Azure RemoteApp zijn gekoppeld aan een domeincontroller in de VNET. Dit is handig wanneer de toepassingen wilt verifiëren met een Windows-domeincontroller om te krijgen van toegang tot de bron van een back-enddatabase.
![Een domein behoren verzamelen in Azure RemoteApp](./media/remoteapp-secureaccess/ra-domainjoined.png)
 
### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Het maken van een beveiligde verbinding tussen Azure en mijn on-premises omgeving
Er zijn verschillende configuratieopties om uw Azure en on-premises omgevingen verbinding te maken. Een handig overzicht van de opties is hier beschikbaar.

Met Azure RemoteApp moet u eerst uw VNet configureren en deze vervolgens gebruiken tijdens het maken van uw siteverzameling. 

## <a name="the-complete-solution"></a>De volledige oplossing
De onderstaande afbeelding ziet de volledige oplossing waar we een kanaal beveiligde toegang van de eindgebruiker via Azure RemoteApp (ARA), zijn ingebouwd in de backend-resource.
![Azure RemoteApp Secure](./media/remoteapp-secureaccess/ra-secureoverview.png) In fase 1 we de gebruikers geselecteerd en access-regels die bepalen hoe ARA kan worden geopend die zijn gemaakt. In het onderstaande voorbeeld toestaan wordt alleen toegang voor gebruikers die werken met het bedrijfsnetwerk. Niet-compatibele gebruikers worden niet helemaal de omgeving ARA toegang tot.
In "Fase 2" hebben we de backend-resource alleen via de VNet/VPN-configuratie die we bepalen zichtbaar. Azure RemoteApp zich bevindt in de dezelfde VNet. Het eindresultaat is dat de resource alleen toegankelijk zijn via de ARA-omgeving.


