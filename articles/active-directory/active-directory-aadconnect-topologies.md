<properties
    pageTitle="Azure AD Connect: Ondersteunde topologieën | Microsoft Azure"
    description="In dit onderwerp details ondersteunde en niet-ondersteunde topologieën voor Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Topologieën voor Azure AD verbinding maken
Het doel van dit onderwerp is om verschillende lokale en Azure AD topologieën bij het synchroniseren van Azure AD Connect als de integratie van belangrijke-oplossing te beschrijven. Ondersteunde en niet-ondersteunde configuraties worden beschreven.

De legenda voor afbeeldingen in het document:

Beschrijving | Pictogram
-----|-----
Lokale Active Directory bos| ![AD](./media/active-directory-aadconnect-topologies/LegendAD1.png)
Active Directory met gefilterde importeren| ![AD](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Azure AD Connect synchronisatieserver| ![Synchroniseren](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure AD Connect synchronisatie server "tijdelijke modus"| ![Synchroniseren](./media/active-directory-aadconnect-topologies/LegendSync2.png)
GALSync met FIM2010 of MIM2016| ![Synchroniseren](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Azure AD Connect synchronisatie-server, gedetailleerde| ![Synchroniseren](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure AD-adreslijst |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
Niet-ondersteunde scenario | ![Niet-ondersteunde](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>Één bos, één van Azure AD-tenant
![Enkel bos één Tenant](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

De meest voorkomende topologie is één on-premises met een of meerdere domeinen en een enkel Azure AD-tenant. Voor Azure AD-verificatie, wordt Wachtwoordsynchronisatie gebruikt. De aangepaste installatie van Azure AD Connect ondersteunt alleen deze topologie.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Eén bos, meerdere synchronisatie servers één Azure AD-tenant
![Enkel bos niet-ondersteunde gefilterd](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Deze wordt niet ondersteund als u wilt dat meerdere Azure AD Connect synchronisatie servers verbonden met de dezelfde Azure AD-tenant, behalve voor een [tijdelijke server](#staging-server). Het is niet-ondersteunde, zelfs als deze zijn geconfigureerd voor synchronisatie van elkaar wederzijds uitsluiten set gegroepeerde objecten. U mogelijk hebt beschouwd als volgt als u alle domeinen in de bos vanaf één server of verdelen over verschillende server niet kan bereiken.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Meerdere forests, Eén Azure AD-tenant
![Meerdere bos één Tenant](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Veel hebben organisaties omgevingen met meerdere lokale Active Directory-forests. Er zijn verschillende redenen met meer dan één lokale Active Directory bos. Voorbeelden van veelgebruikte zijn ontwerpen met account-resource forests en daardoor nadat ik heb een fusie of overname.

Wanneer er meerdere forests, alle forests moet bereikbaar voor één Azure AD Connect synchronisatieserver. U beschikt niet over de server toevoegen aan een domein. Klik zo nodig aan alle bos hebt bereikt, kan de server op een DMZ-netwerk worden geplaatst.

De installatiewizard Azure AD Connect biedt verschillende mogelijkheden om samen te voegen van gebruikers in meerdere forests weergegeven. Het doel is dat een gebruiker slechts één keer in Azure AD wordt weergegeven. Zijn er enkele veelvoorkomende topologieën die u in het aangepaste installatiepad in de installatiewizard kunt configureren. Selecteer de betreffende optie dat staat voor uw topologie op de pagina **een unieke aanduiding voor uw gebruikers**. De samenvoeging is alleen geconfigureerd voor gebruikers. Individuele groepen zijn niet samengevoegd met de standaardinstellingen is geconfigureerd.

Algemene topologieën worden besproken in het volgende gedeelte: [afzonderlijk topologieën](#multiple-forests-separate-topologies), [volledige NET](#multiple-forests-full-mesh-with-optional-galsync)en [Account-Resource](#multiple-forests-account-resource-forest).

De standaard-configuratie in Azure AD Connect synchroniseren wordt ervan uitgegaan:

1. Gebruikers hebben slechts één ingeschakelde account en de bos waar dit account zich bevindt, wordt gebruikt om de gebruiker te verifiëren. Deze verondersteld is voor beide Wachtwoordsynchronisatie en voor Federatie. UserPrincipalName en sourceAnchor/immutableID is afkomstig van deze bos.
2. Gebruikers kunnen slechts één postvak.
3. De bos waarop het postvak van een gebruiker heeft de beste gegevenskwaliteit voor kenmerken zichtbaar in de Exchange globale Address List (GAL). Als er geen postvak op de gebruiker is, kan elke bos bijdragen van deze kenmerkwaarden worden gebruikt.
4. Als u een gekoppelde postvak hebt, klikt u vervolgens is er ook een ander account in een ander domein wordt gebruikt voor het aanmelden.

Als uw omgeving niet overeenkomen met deze hypothesen, gebeurt het volgende:

- Als u meer dan één actieve account of meer dan één postvak hebt, wordt de synchronisatie-engine haalt één en de andere negeren.
- Een gekoppelde Postvak met geen andere actieve account is niet geëxporteerd naar Azure AD. Het gebruikersaccount wordt niet weergegeven als een lid in een groep. Een gekoppelde postvak in DirSync zou altijd worden weergegeven als een normale postvak. Deze wijziging is met opzet een verschillende gedrag betere ondersteuning meerdere bos scenario's.

Meer informatie vindt u in [de standaard-configuation begrijpen](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Meerdere forests, meerdere synchronisatie servers één Azure AD-tenant
![Meerdere bos meerdere synchroniseren niet worden ondersteund](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Deze wordt niet ondersteund als u wilt meer dan één Azure AD-verbinden met synchronisatie server met één verbonden Azure AD-tenant. De uitzondering is het gebruik van een [tijdelijke server](#staging-server).

### <a name="multiple-forests--separate-topologies"></a>Meerdere forests – afzonderlijk topologieën
**Gebruikers worden slechts één keer aangegeven in alle mappen**

![Eenmaal bos voor meerdere gebruikers](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Meerdere bos gescheiden topologieën](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

In deze omgeving alle forests on-premises implementatie worden behandeld als afzonderlijke entiteiten en geen gebruiker wordt gemaakt in een andere bos.
Elke bos heeft een eigen Exchange-organisatie en er is geen GALSync tussen de forests. Deze topologie mogelijk de situatie na een fusie/aanschaf of in een organisatie die waar elke eenheid bedrijven functioneert geïsoleerd van elkaar verschillen. Deze forests worden in dezelfde organisatie in Azure AD en weergegeven met een geïntegreerd GAL.
In deze afbeelding, wordt elk object in elke bos eenmaal weergegeven in de metaverse en samengevoegd in de doeltenant Azure AD.

### <a name="multiple-forests--match-users"></a>Meerdere forests – overeenkomst gebruikers
**De gebruikersidentiteit bestaat in meerdere mappen**

Algemene voor deze scenario's is dat verdeling en beveiligingsgroepen kunnen bevatten een combinatie van gebruikers, contactpersonen en FSP's (refererende beveiliging principes)

FSP's worden gebruikt in Hiermee om aan te geven van leden uit andere forests in een beveiligingsgroep. Alle FSP's worden omgezet in het werkelijke object in Azure AD.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Meerdere forests – volledige mesh met optionele GALSync
**De gebruikersidentiteit bestaan in meerdere mappen. Werken met behulp van: E-mail-kenmerk**

![Bos voor meerdere gebruikers E-mail](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![De volledige NET meerdere bos](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Een volledige meshtopologie kan gebruikers en bronnen in elke bos staan en vaak zou er tweerichtingsvertrouwensrelatie tussen de forests.

Als Exchange in meer dan één bos aanwezig is, kan desgewenst er een on-premises implementatie GALSync-oplossing. Elke gebruiker wordt aangeduid als contactpersoon in alle andere forests. GALSync is meestal geïmplementeerd Forefront identiteit Manager 2010 of Microsoft identiteitsbeheer 2016. Azure AD Connect kan niet worden gebruikt voor on-premises implementatie GALSync.

In dit scenario zijn identiteit objecten gekoppeld met het e-kenmerk. Een gebruiker met een postvak in één bos samengevoegd met de contactpersonen in de andere forests.

### <a name="multiple-forests--account-resource-forest"></a>Meerdere Forests – Account-Resource bos
**De gebruikersidentiteit bestaan in meerdere mappen. Werken met behulp van: ObjectSID en msExchMasterAccountSID kenmerken**

![Meerdere bos gebruikers ObjectSID](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Meerdere bos AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

In een account-resource bos topologie hebt u een of meer account forests met actieve gebruikersaccounts. U hebt ook een of meer resource forests met uitgeschakelde accounts.

In dit scenario één (of meer) **resource bos** -vertrouwensrelaties alle **account forests**. De resource bos heeft meestal een uitgebreide AD-schema in Exchange en Lync. Alle Exchange en Lync-services, evenals andere gedeelde services zich bevinden in deze bos. Gebruikers hebben een uitgeschakelde gebruikersaccount in deze bos en het postvak is gekoppeld aan het account bos.

## <a name="office-365-and-topology-considerations"></a>Office 365 en topologie overwegingen
Sommige Office 365-werkbelastingen moeten bepaalde beperkingen u ondersteunde topologieën. Als u gebruiken op een van deze wilt, klikt u vervolgens het onderwerp ondersteunde topologieën voor de werklast te lezen.

Werkbelasting |  
--------- | ---------
Exchange Online | Als er meer dan één organisatie lokale Exchange-(dat wil zeggen Exchange is geïmplementeerd in meer dan één bos), en vervolgens moet u Exchange 2013 SP1 of hoger. Meer informatie vindt u hier: [hybride implementaties met meerdere Active Directory-forests](https://technet.microsoft.com/library/jj873754.aspx)
Skype voor bedrijven | Als u meerdere forests on-premises implementatie gebruikt, wordt alleen de account-resource bos topologie ondersteund. Details voor ondersteunde topologieën u hier vindt: [milieu vereisten voor Skype voor bedrijven Server 2015](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>Tijdelijke server
![Tijdelijke Server](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect ondersteunt het installeren van een tweede server in de **tijdelijke modus**. Een server in deze modus leest gegevens van alle verbonden mappen maar schrijft iets niet naar verbonden mappen. Dit is via de normale synchronisatie cyclus en daarom heeft een bijgewerkte kopie van de identiteitsgegevens. In een noodgevallen waar de primaire server u mislukt kunt niet via de server tijdelijk opslaan. Dit doet u in de wizard Azure AD Connect. Deze tweede server kan bij voorkeur zich bevinden in een ander datacenter aangezien er geen infrastructuur worden gedeeld met de primaire-server. Elke configuratiewijziging op de primaire server naar de tweede server, moet u handmatig kopiëren.

Een tijdelijk opslaan server kan ook worden gebruikt om een nieuwe aangepaste configuratie en het effect dat er op uw gegevens te testen. U kunt de wijzigingen bekijken en pas de configuratie. Wanneer u tevreden over de nieuwe configuratie bent, kunt u de server tijdelijk opslaan van de actieve server maken en instellen van de oude active server in de modus waarin u tijdelijk opslaat.

Hierdoor kan ook worden gebruikt voor het vervangen van de synchronisatie van active server. De nieuwe server voorbereiden en stel deze in de modus waarin u tijdelijk opslaat. Zorg ervoor dat het is in goede staat, waarin u tijdelijk opslaat (waardoor dit actieve), modus uitschakelen en de actieve server afsluiten.

Het is mogelijk aan meer dan één tijdelijk opslaan server hebt wanneer u meerdere back-ups in verschillende datacenters wilt.

## <a name="multiple-azure-ad-tenants"></a>Meerdere Azure AD-tenants
Microsoft raadt een enkel tenant in Azure AD voor een organisatie.
Voordat u van plan bent het gebruik van meerdere Azure AD-tenants, duidelijk deze onderwerpen veelvoorkomende scenario's, zodat u kunt een enkel tenant gebruiken.

Onderwerp |  
--------- | ---------
Delegeren met administratieve eenheden | [Administratieve eenheden management in Azure AD](active-directory-administrative-units-management.md)

![Meerdere bos Multi-tenant](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Er is een 1:1-relatie tussen een server Azure AD Connect-synchronisatie en een Azure AD-tenant. Voor elke Azure AD-tenant moet u één serverinstallatie van de Azure AD Connect synchroniseren. De exemplaren van Azure AD-tenant zijn standaard zo geïsoleerd en gebruikers in een niet-gebruikers in de andere tenant zien. Als deze scheiding is bedoeld, en dit een ondersteunde configuratie is, maar anders moet u de enkel Azure AD-tenant model.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Elk object slechts één keer in een Azure AD-tenant
![Enkel bos gefilterd](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

In deze topologie is één Azure AD Connect synchronisatieserver verbonden met elke Azure AD-tenant. De synchronisatie-servers van Azure AD Connect moeten worden geconfigureerd voor het filteren van elk hebben een wederzijds set gegroepeerde objecten te bewerken. U kunt bijvoorbeeld elke server aan een bepaald domein of OU beperken. Een DNS-domein kan alleen worden geregistreerd in één Azure AD-tenant. De UPN van de gebruikers in de on-premises implementatie AD afzonderlijke naamruimten ook moet gebruiken. Bijvoorbeeld achtervoegsels in de bovenstaande drie afzonderlijke UPN foto zijn geregistreerd in het lokale AD: contoso.com, fabrikam.com en wingtiptoys.com. Gebruikers in elk on-premises implementatie AD-domein gebruikt u een andere naamruimte.

Er is geen GALsync tussen de exemplaren van Azure AD-tenant. Het adresboek in Exchange Online en Skype voor bedrijven alleen ziet u gebruikers in dezelfde tenant.

Deze topologie heeft de volgende beperkingen tot anderszins ondersteund scenario's:

- Slechts één van de Azure AD-tenants kunt hybride implementatie van Exchange met de lokale Active Directory inschakelen.
- Windows 10-apparaten kunnen alleen worden gekoppeld aan één Azure AD-tenant.

Het vereiste voor elkaar wederzijds uitsluiten set gegroepeerde objecten geldt ook voor write-backs. Sommige write-backs-functies worden niet ondersteund met deze topologie omdat deze functies wordt ervan uitgegaan een enkele configuratie dat on-premises implementatie:

-   Groep write-backs met standaardinstellingen is geconfigureerd
-   Apparaat write-backs

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Elk object meerdere keren in een Azure AD-tenant
![Enkel bos Multi-Tenant niet worden ondersteund](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Enkel bos meerdere verbindingslijnen niet worden ondersteund](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- Deze wordt niet ondersteund als u wilt synchroniseren van de gebruiker zich meerdere Azure AD-tenants.
- Het is niet-ondersteunde om te maken van een configuratie wijzigen zodat gebruikers in één Azure AD weergegeven als contactpersonen in een andere Azure AD-tenant.
- Deze wordt niet ondersteund als u wilt wijzigen van Azure AD Connect-synchronisatie verbinding maakt met meerdere Azure AD-tenants.

### <a name="galsync-by-using-writeback"></a>GALsync met behulp van write-backs
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD-tenants zijn standaard zo geïsoleerd.

- Deze wordt niet ondersteund als u wilt wijzigen van de configuratie van Azure AD Connect-synchronisatie gegevens uit een andere Azure AD-tenant lezen.
- Deze wordt niet ondersteund als u wilt gebruikers exporteren als contactpersonen naar een andere lokale AD Azure AD Connect synchroniseren met.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync met on-premises synchronisatie-server
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

Dit wordt ondersteund als u wilt gebruiken, FIM2010/MIM2016 on-premises implementatie aan gebruikers GALsync tussen twee Exchange-organisaties. Gebruikers in een organisatie weergegeven als externe gebruikers/contactpersonen in de andere organisatie. Deze verschillende lokale advertenties kunnen vervolgens worden gesynchroniseerd met hun eigen Azure AD-tenants.

## <a name="next-steps"></a>Volgende stappen
Zie informatie over het installeren van Azure AD Connect voor deze scenario's, [aangepaste installatie van Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

Meer informatie over de configuratie [Azure AD Connect synchroniseren](active-directory-aadconnectsync-whatis.md) .

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
