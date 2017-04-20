<properties
    pageTitle="De toegang tot Log Analytics beheren | Microsoft Azure"
    description="Toegang tot Log analyses met een verscheidenheid aan beheertaken op gebruikers, accounts, OMS werkruimten en Azure accounts beheren."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>De toegang tot Log Analytics beheren

Als u wilt beheren toegang tot Log analyses, gebruikt u een aantal beheertaken op gebruikers, accounts, OMS werkruimten en Azure-accounts. Als u wilt een nieuwe werkruimte maken in de bewerkingen Management Suite Kantoorbeheersysteem, kiest u de naam van een werkruimte, koppelen aan uw account en een geografische locatie te kiezen. Een werkruimte is in principe een container met accountgegevens en eenvoudige configuratiegegevens voor het account. U of andere leden van uw organisatie kunnen meerdere OMS werkruimten gebruiken om verschillende soorten gegevens die worden verzameld uit alle of gedeelten van uw IT-infrastructuur te beheren.

Het artikel [aan de slag met Log Analytics](log-analytics-get-started.md) leest u hoe om snel en wordt uitgevoerd en de rest van dit artikel beschreven uitgebreider enkele van de acties die u moet de toegang tot OMS beheren.

Hoewel u niet nodig hebt mogelijk om uit te voeren van elke beheertaak bij de eerste, worden besproken alle veelgebruikte taken die u in de volgende secties gebruiken kunt:

- Bepaal het aantal werkruimten die u nodig hebt
- Accounts en gebruikers beheren
- Een groep toevoegen aan een bestaande werkruimte
- Een bestaande werkruimte koppelen aan een Azure-abonnement
- Upgraden van een werkruimte naar een betaald data-abonnement
- Een abonnement gegevenstype wijzigen
- Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte
- Sluit uw OMS-werkruimte

## <a name="determine-the-number-of-workspaces-you-need"></a>Bepaal het aantal werkruimten die u nodig hebt

Een werkruimte is een Azure bron en is een container waarbij gegevens die worden verzameld, samengevoegd, geanalyseerd en in de portal OMS gepresenteerd.

Het is mogelijk om meerdere OMS Log Analytics-werkruimten te maken en voor gebruikers toegang hebben tot een of meer werkruimten. In het algemeen wilt u het aantal werkruimten minimaliseren, zodat u query en relateren over de meeste gegevens. Dit onderwerp vindt wanneer kan het handig zijn om meer dan één werkruimte te maken.

Vandaag, vindt u een logboek Analytics-werkruimte:

- Een geografische locatie voor gegevensopslag
- Granulatie voor facturering
- Gegevens moeten worden geïsoleerd

Op basis van de bovenstaande kenmerken, kunt u meerdere werkruimten maken als:

- U een globale bedrijf bent en u gegevens die zijn opgeslagen in bepaalde gebieden om gegevens te garanderen of naleving redenen nodig hebt.
- U Azure gebruikt en u wilt voorkomen dat doorverbinden van uitgaande datakosten doordat een logboek Analytics-werkruimte in de regio hetzelfde als de Azure resources beheert.
- U wilt toewijzen kosten over verschillende afdelingen of groepen van bedrijven op basis van hun gebruik. Wanneer u een werkruimte voor elke afdeling of groep zakelijk maakt, ziet uw Azure factuur en het gebruik instructie u de kosten die voor elke werkruimte afzonderlijk.
- U bent een beheerde serviceprovider en nodig hebt om de de log analytics-gegevens voor elke klant u beheert geïsoleerd van andere klantgegevens.
- U meerdere klanten beheert en u wilt dat elke klant of afdeling of groep zakelijk om hun eigen gegevens, maar niet de gegevens voor andere klanten of afdelingen of groepen bedrijven weer te geven.

Wanneer u een agenten gebruikt om gegevens te verzamelen, kunt u elke agent om naar de vereiste werkruimte kunt configureren.

Als u System Center Operations manager gebruikt, kan elke groep Operations Manager-beheer kan worden verbonden met slechts één werkruimte. U kunt de controle-Agent van Microsoft installeren op computers beheerd door Operations Manager en het rapport agent op zowel Operations Manager en een andere Log Analytics-werkruimte.

### <a name="workspace-information"></a>Informatie over de werkruimte

Klik in de portal OMS kunt u werkruimtegegevens met de weergeven en kies of u wilt ontvangen van Microsoft.

#### <a name="view-workspace-information"></a>Informatie van de werkruimte weergeven

1. In OMS, klikt u op de tegel **-Instellingen** .
2. Klik op het tabblad **Accounts** .
3. Klik op het tabblad **Informatie over de werkruimte** .  
  ![Informatie over de werkruimte](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Accounts en gebruikers beheren

Elke werkruimte kan bevatten meerdere gebruikersaccounts die zijn gekoppeld, en elke gebruikersaccount (Microsoft-account of organisatieaccount) meerdere OMS-werkruimte kunt beschikken.

De Microsoft-account of organisatieaccount gebruikt om te maken van de werkruimte wordt standaard de beheerder van de werkruimte. De beheerder kan uitnodigen extra Microsoft-accounts of kiest u gebruikers van Azure Active Directory.

Mensen toegang geven tot de werkruimte OMS wordt gecontroleerd op 2 plaatsen:

- In Azure wordt aangegeven, kunt u Rolgebaseerd toegangsbeheer voor toegang tot het Azure abonnement en de bijbehorende Azure resources. Dit wordt ook gebruikt voor PowerShell en REST API-toegang.
- Klik in de portal OMS toegang tot alleen de OMS portal - niet het bijbehorende Azure abonnement.

Als u mensen toegang geven bij de portal OMS, maar niet op de Azure abonnement dat is gekoppeld aan, worden klikt u vervolgens de tegels van de oplossing automatisering, back-up- en Site-herstel niet weergegeven gegevens aan gebruikers wanneer ze aanmeldingsproblemen de OMS-portal.

Toestaan dat alle gebruikers om de gegevens in deze oplossingen, zorgen ze ten minste beschikken over **reader** toegang heeft tot voor de automatisering-Account, kluis back-up- en Site-herstel kluis die is gekoppeld aan de werkruimte OMS.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Toegang tot Log analyses met behulp van de Azure portal beheren

Als u mensen toegang tot de Log Analytics-werkruimte met Azure machtigingen geven, in de portal van Azure bijvoorbeeld, klikt u vervolgens de dezelfde gebruikers hebben toegang tot de portal Log Analytics. Als er gebruikers zijn in de portal van Azure, kunnen ze Ga naar de beheerportal OMS door te klikken op de taak **OMS Portal** bij het weergeven van de resource van de werkruimte Log Analytics.

Enkele punten waaraan u moet denken over de Azure-portal:

- Dit is niet *Rolgebaseerd toegangsbeheer*. Als u toegangsmachtigingen *Reader* in Azure portal voor de werkruimte Log Analytics hebt, kunt u met behulp van de portal OMS wijzigingen maken. De portal OMS heeft een concept van beheerder, Inzender en alleen-lezen-gebruiker. Als het account dat u aangemeld bij met bent in de Azure Active Directory gekoppeld aan de werkruimte wordt u een beheerder in de portal OMS, anders zult u een Inzender.

- Wanneer u aanmelden bij de OMS-portal met http://mms.microsoft.com, klikt u vervolgens al dan niet standaard, ziet u de lijst **Selecteer een werkruimte** . De presentatie bevat alleen werkruimten die zijn toegevoegd met behulp van de portal OMS. Als u wilt zien van de werkruimten u toegang hebt tot met Azure-abonnement, moet u om op te geven van een tenant als onderdeel van de URL. Bijvoorbeeld:

  `mms.microsoft.com/?tenant=contoso.com`De tenant-id is vaak laatste deel van het e-mailadres dat u aanmelden met.

- Als het account u aanmelden met een account in de tenant Azure Active Directory, dat wil meestal de hoofdletters/kleine letters zeggen tenzij u bezig met aanmelden als een provider bent, zult u een *beheerder* in de portal OMS. Als uw account niet in de tenant Azure Active Directory, zult u een *gebruiker* in de portal OMS.

- Als u navigeren rechtstreeks naar een portal wilt dat u toegang tot het gebruik van Azure machtigingen hebben, moet u de resource opgeven als onderdeel van de URL. Het is mogelijk om deze URL via PowerShell.

  Bijvoorbeeld `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  De URL eruitziet:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>Gebruikers beheren in de portal OMS

U beheren gebruikers en groepen op het tabblad **Gebruikers beheren** onder het tabblad **Accounts** op de pagina instellingen. U kunt er, de taken uitvoeren in de volgende secties.  

![gebruikers beheren](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Een gebruiker toevoegen aan een bestaande werkruimte

Gebruik de volgende stappen als u een gebruiker of groep toevoegen aan een werkruimte OMS. De gebruiker of groep kunnen weergeven en klik op alle meldingen die zijn gekoppeld aan deze werkruimte.

>[AZURE.NOTE] Als u toevoegen van een gebruiker of groep van uw organisatie Azure Active Directory-account wilt, moet u eerst ervoor zorgen dat u uw account OMS hebt gekoppeld aan uw Active Directory-domein. Zie [een Azure Active Directory-organisatie naar een bestaande werkruimte toevoegen](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. In OMS, klikt u op de tegel **-Instellingen** .
2. Klik op het tabblad **Accounts** en klik op het tabblad **Gebruikers beheren** .
3. Kies het accounttype toevoegen in de sectie **Gebruikers beheren** : **Organisatieaccount**, **Microsoft-Account**, **Microsoft Support**.
    - Als u Microsoft-Account hebt gekozen, typ het e-mailadres van de gebruiker die is gekoppeld aan het Microsoft-Account.
    - Als u organisatie-Account, kunt u een deel van de gebruiker of groep van naam of het e-mailalias en een lijst met gebruikers en groepen worden weergegeven. Selecteer een gebruiker of groep.
    - Microsoft Support via een Microsoft Support verlenen tijdelijk toegang krijgen tot uw werkruimte om te helpen bij het oplossen van engineering.

    >[AZURE.NOTE] Het aantal Active Directory-groepen die is gekoppeld aan één OMS account drie voor de beste prestatieresultaten te beperken, één voor beheerders, één voor inzenders en één voor alleen-lezen-gebruikers. Werken met meer groepen mogelijk van invloed zijn de prestaties van Log Analytics.

5. Kies het type van de gebruiker of groep wilt toevoegen: **beheerder**, **Inzender**of **Alleen-lezen-gebruiker** .  
6. Klik op **toevoegen**.

  Als u een Microsoft-account toevoegt, wordt een uitnodiging voor deelname aan de werkruimte verzonden naar de e-mail die u hebt opgegeven. Nadat de gebruiker wordt gevolgd door de instructies in de uitnodiging voor deelname aan OMS, de gebruiker de waarschuwingen en accountgegevens voor dit account OMS kunt bekijken en is mogelijk gegevens van de gebruiker op het tabblad **Accounts** van de pagina **Instellingen** weergeven.
  Als u een organisatie-account toevoegt, wordt de gebruiker Log Analytics direct toegang tot zijn.  
  ![e-mail met uitnodiging](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Een bestaande gebruikerstype bewerken

U kunt de Accountrol voor een gebruiker die is gekoppeld aan uw account OMS wijzigen. Hebt u de volgende rol-opties:

 - *Beheerder*: kunnen gebruikers beheren, bekijken en reageren op alle meldingen, en toevoegen en verwijderen van servers

 - *Inzender*: kunt weergeven en reageren op alle meldingen, en toevoegen en verwijderen van servers

 - *Alleen-lezen-gebruiker*: gebruikers die zijn gemarkeerd als alleen-lezen niet kunnen:
   1. Oplossingen toevoegen/verwijderen. De galerie met oplossingen is verborgen.
   2. Tegels toevoegen/wijzigen/verwijderen op **Mijn Dashboard**.
   3. De **Instellingen voor** pagina's weergeven. De pagina's zijn verborgen.
   4. In de zoekopdracht weergave, PowerBI configuratie opgeslagen zoekacties en waarschuwingen zijn taken verborgen.


#### <a name="to-edit-an-account"></a>Een account bewerken

1. In OMS, klikt u op de tegel **-Instellingen** .
2. Klik op het tabblad **Accounts** en klik op het tabblad **Gebruikers beheren** .
3. Selecteer de rol voor de gebruiker die u wilt wijzigen.
2. Klik op **Ja**in het bevestigingsvenster.

### <a name="remove-a-user-from-a-oms-workspace"></a>Een gebruiker van een werkruimte OMS verwijderen

Gebruik de volgende stappen als u een gebruiker verwijderen uit een werkruimte OMS. Houd er rekening mee dat dit niet van de gebruiker werkruimte gesloten. In plaats daarvan deze Hiermee verwijdert u de koppeling tussen die gebruiker en de werkruimte. Als een gebruiker gekoppeld aan meerdere werkruimten is, kunnen die gebruiker nog steeds niet kunt aanmelden bij OMS en ziet u de andere werkruimten.

1. In OMS, klikt u op de tegel **-Instellingen** .
2. Klik op het tabblad **Accounts** en klik op het tabblad **Gebruikers beheren** .
3. Klik op **verwijderen** naast de naam van de gebruiker die u wilt verwijderen.
4. Klik op **Ja**in het bevestigingsvenster.


### <a name="add-a-group-to-an-existing-workspace"></a>Een groep toevoegen aan een bestaande werkruimte

1.  Voer stap 1 -4 in ' Als u wilt een gebruiker toevoegen aan een bestaande werkruimte', hierboven.
2.  Selecteer onder **Kies gebruiker/groep** **groep**.
    ![een groep toevoegen aan een bestaande werkruimte](./media/log-analytics-manage-access/add-group.png)
3.  Voer de naam weer te geven of e-mailadres voor de groep die u wilt toevoegen.
4.  Selecteer de groep in de lijst met zoekresultaten en klik vervolgens op **toevoegen**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Een bestaande werkruimte koppelen aan een Azure-abonnement

Is het mogelijk te maken van een werkruimte via de website [microsoft.com/oms](https://microsoft.com/oms) .  Bepaalde limieten zijn echter voor deze werkruimten, de vermaarde een limiet van 500MB/dag van de gegevens uploads als u een gratis-account gebruikt. Als u wijzigingen wilt voor deze werkruimte moet u om uw bestaande werkruimte bij een Azure-abonnement te *koppelen*.

>[AZURE.IMPORTANT] Als u wilt koppelen in een werkruimte, moet uw Azure-account al toegang hebben tot de werkruimte die u wilt koppelen.  Met andere woorden, moeten het account dat u gebruikt voor toegang tot de portal van Azure **hetzelfde** als de account die u gebruikt voor toegang tot uw werkruimte OMS. Als dit niet het geval is, raadpleegt u [een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Naar een werkruimte koppelen aan een Azure-abonnement in de portal OMS

Om te kunnen een werkruimte koppelen aan een Azure-abonnement in de portal OMS, moet de gebruiker die zijn aangemeld bij een betaald Azure-account al hebt. De werkruimte die u actief gebruikt wordt gekoppeld aan de Azure-account.

1. In OMS, klikt u op de tegel **-Instellingen** .
2. Klik op het tabblad **Accounts** en klik op het tabblad **Azure-abonnement en gegevens plannen** .
3. Klik op de data-abonnement dat u gebruiken wilt.
4. Klik op **Opslaan**.  
  ![abonnement en gegevens-abonnementen](./media/log-analytics-manage-access/subscription-tab.png)

Uw nieuwe data-abonnement wordt weergegeven in de portal OMS-lint boven aan uw webpagina.

![OMS-lint](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Naar een werkruimte koppelen aan een Azure-abonnement in de portal van Azure

1.  Meld u aan bij de [portal van Azure](http://portal.azure.com).
2.  Blader naar **Log Analytics (OMS)** en selecteer deze.
3.  Hier ziet u de lijst met bestaande werkruimten. Klik op **toevoegen**.  
    ![lijst met werkruimten](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  Onder **OMS werkruimte**, klikt u op **of een bestaande koppeling**.  
    ![bestaande koppeling](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Klik op **configureren instellingen vereist**.  
    ![vereiste instellingen configureren](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Hier ziet u de lijst met werkruimten die nog niet zijn gekoppeld aan uw Azure-account. Selecteer een werkruimte.  
    ![Selecteer werkruimten](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Indien nodig kunt u waarden voor de volgende items:
    - Abonnement
    - Resourcegroep
    - Locatie
    - Prijzen van laag  
        ![waarden wijzigen](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Klik op **maken**. De werkruimte wordt nu gekoppeld aan uw Azure-account.

>[AZURE.NOTE] Als u de werkruimte die u wilt koppelen niet ziet, heeft klikt u vervolgens uw Azure abonnement geen toegang naar de werkruimte OMS is dat u met de website OMS gemaakt.  Moet u toegang wilt verlenen aan dit account uit binnen uw OMS werkruimte met de OMS-website. Zie [een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace)hiervoor.



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Upgraden van een werkruimte naar een betaald data-abonnement

Er zijn drie werkruimtegegevens typen voor OMS plannen: **gratis**, **Standard**en **Premium**.  Als u een *vrije* -abonnement, is het mogelijk dat u uw gegevens initiaal van 500 MB hebt raken.  U moet uw werkruimte upgraden naar een ***pay-as-you-go plan*** voor het verzamelen van gegevens buiten deze limiet. U kunt op elk gewenst moment uw abonnement type converteren.  Zie voor meer informatie over het Kantoorbeheersysteem prijzen, [Prijzen Details](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Werkruimte abonnementen kunnen alleen worden gewijzigd als ze *gekoppeld* aan een Azure-abonnement zijn.  Als u uw werkruimte gemaakt in Azure wordt aangegeven of als u hebt *al* uw werkruimte die zijn gekoppeld, kunt u dit bericht negeren.  Als u uw werkruimte met de [OMS-website](http://www.microsoft.com/oms)hebt gemaakt, moet u de stappen op de [koppeling een bestaande werkruimte bij een Azure-abonnement](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>Met behulp van de rechten van de invoegtoepassing OMS voor System Center

De invoegtoepassing OMS voor System Center biedt een recht voor de Premium-abonnement van OMS Log Analytics, beschreven op [OMS prijzen](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

Wanneer u de invoegtoepassing OMS voor System Center koopt, wordt de invoegtoepassing OMS toegevoegd als een recht op overeenkomsten System Center. Een Azure abonnement dat wordt gemaakt onder deze overeenkomst kunt maken gebruik van het recht. Hiermee kunt u, bijvoorbeeld om te laten meerdere OMS-werkruimten die het recht van de invoegtoepassing OMS gebruiken.

Om ervoor te zorgen dat gebruik van een werkruimte OMS wordt toegepast op de rechten van de invoegtoepassing OMS, moet u:

1. Uw werkruimte OMS koppelen aan een Azure-abonnement die deel uitmaakt van de Enterprise Agreement met zowel de aankoop van OMS-invoegtoepassing en het gebruik van de Azure-abonnement
2. Selecteer de Premium-abonnement voor de werkruimte

Wanneer u uw gebruik in de portal Azure of OMS bekijkt, kunt u de rechten van de invoegtoepassing OMS niet weergegeven. U kunt echter rechten in de Enterprise-Portal zien.  

Als u wijzigen van de Azure-abonnement dat uw werkruimte OMS is gekoppeld wilt aan, kunt u de cmdlet Azure PowerShell [Verplaatsen-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) gebruiken.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Azure betrokkenheid van een Enterprise Agreement gebruiken

Als u gebruiken zelfstandige prijzen voor OMS onderdelen wilt, u betaalt voor elk onderdeel van OMS afzonderlijk en het gebruik worden weergegeven op uw Azure factuur.

Als u een Azure monetaire doorvoeren in de enterprise-registratie waaraan uw Azure-abonnementen zijn gekoppeld, wordt automatisch een gebruik van Log Analytics afgetrokken ten opzichte van alle resterende monetaire doorvoeren.

Als u wilt wijzigen kunt het Azure abonnement dat de werkruimte OMS is gekoppeld aan u de cmdlet Azure PowerShell [Verplaatsen-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) gebruiken.  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Een werkruimte wijzigen in een betaald data-abonnement

1.  Meld u aan bij de [portal van Azure](http://portal.azure.com).
2.  Blader naar **Log Analytics (OMS)** en selecteer deze.
3.  Hier ziet u de lijst met bestaande werkruimten. Selecteer een werkruimte.  
    ![lijst met werkruimten](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  Klik onder **Instellingen**, klikt u op **prijzen laag**.  
    ![prijzen van laag](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  Klik onder **prijzen laag**, selecteert u een data-abonnement en klik vervolgens op **selecteren**.  
    ![plan selecteren](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Wanneer u uw weergave in de portal van Azure vernieuwt, ziet u **prijzen laag** bijgewerkt voor het abonnement dat u hebt geselecteerd.  
    ![prijzen laag bijwerken](./media/log-analytics-manage-access/manage-access-change-plan04.png)

U kunt nu gegevens buiten de initiaal "gratis" gegevens verzamelen.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte

U kunt uw werkruimte Log Analytics Kantoorbeheersysteem koppelen aan een Azure Active Directory-domein. Hiermee kunt u gebruikers uit Active Directory rechtstreeks toevoegen aan uw werkruimte OMS zonder een afzonderlijke Microsoft-account.

Als u de werkruimte via de Azure-portal maken of uw werkruimte aan een Azure-abonnement koppelen wordt uw Azure Active Directory als uw organisatie-account zijn gekoppeld.

Wanneer u de werkruimte van de portal OMS maakt wordt u gevraagd om de koppeling naar een Azure-abonnement en een organisatieaccount.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte

1. Klik op de pagina instellingen in OMS **Accounts** op en klik vervolgens op het tabblad **Informatie over de werkruimte** .  
2. Bekijk de informatie over de organisatie-accounts en klik vervolgens op **Organisatie toevoegen**.  
    ![organisatie toevoegen](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Voer de identiteitsgegevens voor de beheerder van de van de Azure Active Directory-domein. Hierna ziet u een bevestiging met de mededeling dat uw werkruimte is gekoppeld aan uw Azure Active Directory-domein.
    ![gekoppelde werkruimte bevestiging](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Zodra uw account is gekoppeld aan een organisatie-Account, kan niet koppelen worden verwijderd of gewijzigd.

## <a name="close-your-oms-workspace"></a>Sluit uw OMS-werkruimte

Wanneer u een werkruimte OMS sluit, worden alle gegevens die zijn gerelateerd aan uw werkruimte verwijderd uit de OMS-service binnen 30 dagen na het sluiten van de werkruimte.

Als u een beheerder bent en er meerdere gebruikers die zijn gekoppeld aan de werkruimte, wordt de koppeling tussen die gebruikers en de werkruimte is verbroken. Als de gebruikers die gekoppeld aan andere werkruimten zijn, kunnen ze vervolgens blijven OMS gebruiken met die andere werkruimten. Echter als ze niet gekoppeld aan andere werkruimten zijn moet ze maken van een nieuwe werkruimte als u wilt gebruiken OMS.

### <a name="to-close-an-oms-workspace"></a>Om te sluiten van een werkruimte OMS

1. In OMS, klikt u op de tegel **-Instellingen** .
2. Klik op het tabblad **Accounts** en klik op het tabblad **Informatie over de werkruimte** .
3. Klik op **sluiten werkruimte**.
4. Selecteer een van de redenen voor het afsluiten van uw werkruimte of een andere reden invoeren in het tekstvak.
5. Klik op **sluiten werkruimte**.

## <a name="next-steps"></a>Volgende stappen

- Zie [verbinding maken met Windows-computers Log Analytics](log-analytics-windows-agents.md) agenten toevoegen en gegevens te verzamelen.
- [Oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md) voor functionaliteit toevoegen en gegevens te verzamelen.
- [Proxy- en firewall instellingen configureren in Log Analytics](log-analytics-proxy-firewall.md) als uw organisatie gebruikmaakt van een proxyserver of firewall zodat agenten met de Log Analytics-service communiceren kunnen.
