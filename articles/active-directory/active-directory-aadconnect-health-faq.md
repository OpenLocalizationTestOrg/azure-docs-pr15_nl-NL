<properties
    pageTitle="Azure AD Connect systeemstatus Veelgestelde vragen"
    description="Deze Veelgestelde vragen vindt u antwoorden op vragen over gezondheid Azure AD verbinding maken. Deze Veelgestelde vragen over behandelt vragen over het gebruik van de service, inclusief de facturering model, mogelijkheden, beperkingen, en ondersteuning."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Azure AD Connect systeemstatus Veelgestelde vragen (FAQ)

Deze Veelgestelde vragen vindt u antwoorden op vragen over gezondheid Azure AD verbinding maken. Deze Veelgestelde vragen over behandelt vragen over het gebruik van de service, inclusief de facturering model, mogelijkheden, beperkingen, en ondersteuning.

## <a name="general-questions"></a>Algemene vragen



**V: ik beheren meerdere Azure AD-mappen. Hoe schakel ik tussen de wijziging met Azure Active Directory Premium?**

U kunt schakelen tussen verschillende Azure AD-tenants door de momenteel ondertekend selecteren in de gebruikersnaam in te voeren in de rechterbovenhoek en het juiste account te kiezen. Als het account is hier niet wordt vermeld, selecteert u afmelden en gebruik vervolgens de referenties van de globale beheerder van de map met Azure Active Directory-Premium ingeschakeld aan te melden.

## <a name="installation-questions"></a>Installatievragen



**V: Wat is de invloed van de Azure AD verbinding systeemstatus-Agent installeren op afzonderlijke servers?**

De impact van de installatie van het Microsoft Azure AD verbinding systeemstatus agenten ADFS, Web Application-proxyservers, Azure AD Connect (sycn)-servers domeincontrollers is minimale met betrekking tot de CPU, geheugen verbruik netwerkbandbreedte en opslag.

De onderstaande getallen zijn een benadering.

- Processorgebruik: ~ 1% verhogen
- Geheugengebruik: maximaal 10% van de totale geheugen

>[AZURE.NOTE] In het geval van de agent is niet meer te communiceren met Azure, slaat de agent lokaal, de gegevens naar een gedefinieerde maximumlimiet. De agent overschrijft de gegevens in de "cache" op basis van 'laatst verwerkt'.

- Lokale buffer opslag voor Azure AD verbinding systeemstatus kunt vinden: ~ 20 MB
- Het wordt aanbevolen dat u een schijfruimte van 1024 MB (1 GB) voor de AD FS controle kanaal voor Azure AD verbinding systeemstatus agenten verwerkingstijd van de controlegegevens voordat deze wordt overschreven inrichten voor AD FS-servers.

**V: ontvangen ik me start opnieuw op mijn servers tijdens de installatie van de Azure AD verbinding systeemstatus kunt vinden?**

Nee. Start opnieuw op de server is niet nodig voor de installatie van de agenten zijn vereist. Installatie van enkele van de vereiste stappen kan echter opnieuw starten van de server.

Bijvoorbeeld op Windows Server 2008 R2 de installatie van .net moet Framework 4.5 worden server opgestart.


**V: worden Azure AD verbinding systeemstatus Services werken via een Pass Through-http-proxy?**

Ja.  Voor op gaan bewerkingen uitvoeren, kunt u de status-Agent doorsturen van uitgaande HTTP-aanvragen via een HTTP-Proxy configureren. Zie voor meer informatie [configureren Azure AD verbinding systeemstatus agenten Gebruik HTTP-Proxy.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
Als u nodig hebt voor het configureren van een proxy tijdens de registratie van Agent, moet u mogelijk vooraf uw proxyinstellingen van Internet Explorer wijzigen.
1. Open Internet Explorer-instellingen > Internet -> Opties -> verbindingen -> LAN-instellingen.
2. Selecteer een proxyserver gebruiken voor uw LAN.
3. Als u verschillende proxy-poorten voor HTTP en HTTPS/Secure hebt, selecteert u Geavanceerd.

**V: systeemstatus-Services van Azure AD Connect ondersteunt basisverificatie wanneer u verbinding maakt met HTTP-proxy's?**

Nee. Een methode voor het opgeven van willekeurige gebruikersnaam en wachtwoord voor basisverificatie wordt momenteel niet ondersteund.


**V: welke versie van AD DS worden ondersteund door Azure AD verbinding servicestatus voor AD DS?**

Cmdlets voor controle van AD DS wordt ondersteund terwijl er op de volgende versies van OS geïnstalleerd:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

## <a name="operations-questions"></a>Bewerkingen vragen



**V: heb ik nodig controle op mijn proxyservers van AD FS-toepassing of mijn Web Application-proxyservers inschakelen?**

Nee, controle niet hoeft te worden ingeschakeld op de Web Application Proxy (WAP)-Servers. Inschakelen op de AD FS-servers.


**V: hoe Azure AD verbinding systeemstatus waarschuwingen krijgen opgelost?**

Azure AD verbinding systeemstatus waarschuwingen ophalen van een voorwaarde success opgelost. Azure AD verbinding systeemstatus agenten detecteren en meldt u de voorwaarden succes met de service periodiek. Voor een paar waarschuwingen is de onderdrukken op basis van tijd. Met andere woorden, als dezelfde foutvoorwaarde is niet-naleving binnen 72 uur uit genereren van waarschuwingen de waarschuwing automatisch opgelost.




**V: welke firewallpoorten heb ik nodig om te openen voor de Azure AD verbinding systeemstatus Agent om te werken?**

Moet u beschikken over TCP/UDP-poorten 443 en 5671 geopend om de Azure AD verbinding systeemstatus Agent moeten kunnen communiceren met de eindpunten van de service Azure AD status.


**V: Waarom zie ik twee servers met dezelfde naam in de Azure AD verbinding systeemstatus Portal?**

Wanneer u een agent van een server verwijdert, wordt de server niet automatisch verwijderd uit de Portal Azure AD verbinding maken.  Als u handmatig een agent verwijderd uit een server of verwijderd van de server zelf, moet u de serververmelding handmatig verwijderen uit de status verbinding maken met Azure AD-portal. Ga voor meer informatie naar [verwijderen van een exemplaar van de server of service.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Als u een server reimaged of een nieuwe server die zijn gemaakt met de dezelfde details (zoals de naam van de computer) en heeft het al geregistreerde server niet verwijderd uit de status verbinding maken met Azure AD-portal, de agent geïnstalleerd op de nieuwe server, wordt er twee vermeldingen met dezelfde naam.  
In dit geval moet u de vermelding die deel uitmaken van de oudere server handmatig verwijderen. De gegevens voor deze server moet verouderde.

## <a name="related-links"></a>Verwante koppelingen

* [Azure AD Connect systeemstatus](active-directory-aadconnect-health.md)
* [Azure AD Connect systeemstatus Agent installatie](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect systeemstatus bewerkingen](active-directory-aadconnect-health-operations.md)
* [Gebruik van Azure AD status verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Met behulp van Azure AD verbinding servicestatus voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Gebruik van Azure AD status verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect systeemstatus versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
