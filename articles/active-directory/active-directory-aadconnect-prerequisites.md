<properties
   pageTitle="Azure AD Connect: Vereisten en hardware | Microsoft Azure"
   description="In dit onderwerp worden de minimumvereisten en de hardwarevereisten voor Azure AD Connect"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Vereisten voor Azure AD verbinding maken
In dit onderwerp worden de minimumvereisten en de hardwarevereisten voor Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Voordat u Azure AD Connect installeert
Voordat u Azure AD Connect hebt geïnstalleerd, zijn er enkele dingen die u nodig hebt.

### <a name="azure-ad"></a>Azure AD
- Een Azure of een [Azure proefabonnement](https://azure.microsoft.com/pricing/free-trial/). Deze functie is alleen vereist voor toegang tot de portal van Azure en niet voor het gebruik van Azure AD Connect. Als u PowerShell- of Office 365 hoeft u niet een Azure-abonnement Azure AD Connect gebruiken. Als u al een Office 365-licentie kunt u ook de Office 365-portal. Met een betaald Office 365-licentie kunt u ook openen in de portal van Azure vanuit de Office 365-portal.
    - U kunt ook de functie van de voorbeeldweergave van Azure AD gebruiken in de [portal van Azure](https://portal.azure.com). Deze portal hoeft niet een Azure-licentie.
- [Toevoegen en controleer of het domein](active-directory-add-domain.md) u van plan bent om in Azure AD te gebruiken. Bijvoorbeeld als u van plan bent u contoso.com gebruiken voor uw gebruikers en zorg ervoor dat dit domein is geverifieerd en u het standaarddomein contoso.onmicrosoft.com niet alleen gebruikt.
- Een Azure AD-tenant kan door 50k standaardobjecten. Wanneer u uw domein verifiëren, is de limiet groter, zodat 300 k objecten. Als u nog meer objecten nodig in Azure AD hebt moet u een kwestie ondersteuning als u de limiet verhoogd nog verder wilt openen. Als u nodig meer dan 500 k objecten hebt moet u een licentie, zoals Office 365, Azure AD eenvoudige, Azure AD Premium of Enterprise mobiliteit Suite.

### <a name="prepare-your-on-premises-data"></a>Uw on-premises gegevens voorbereiden
- [Optionele synchronisatiefuncties die u in Azure AD inschakelen kunt](active-directory-aadconnectsyncservice-features.md) bekijken en beoordelen van welke functies die u moet inschakelen.

### <a name="on-premises-servers-and-environment"></a>On-premises implementatie-servers en -omgeving
- Het AD schema versie en bos functionele niveau moet Windows Server 2003 of later. Alle versies kunnen worden uitgevoerd in de domeincontrollers zo lang maken als het schema en bos niveau vereisten wordt voldaan.
- Als u van plan bent het gebruik van de functie **wachtwoord write-backs** moet de op Windows Server 2008 (met de meest recente SP) of later. Als uw DCs op 2008 (pre-R2) moet u ook [hotfix KB2386717](http://support.microsoft.com/kb/2386717)toepassen.
- De domeincontroller die wordt gebruikt door Azure AD moet schrijfbare. Deze wordt niet ondersteund als u wilt gebruiken, een alleen-lezen domeincontroller (alleen-lezen domeincontroller) en Azure AD Connect niet voldoet aan alle omleidingen schrijven.
- Azure AD Connect kan niet worden geïnstalleerd op Small Business Server of Windows Server Essentials. De server moet worden gebruikt als Windows Server standard of verbeterd hebt geselecteerd.
- De server Azure AD Connect moet een volledige gebruikersinterface is geïnstalleerd. Deze wordt niet ondersteund als u wilt installeren op de server core.
- Azure AD Connect moet zijn geïnstalleerd op Windows Server 2008 of hoger. Deze server mogelijk een domeincontroller of een lidserver als express-instellingen. Als u aangepaste instellingen gebruikt, wordt de server kan ook zijn zelfstandig en niet hoeft te worden toegevoegd aan een domein.
- Als u Azure AD Connect op Windows Server 2008 installeert, zorg er dan voor dat de meest recente patches toepassen op Windows Update. De installatie is niet kan worden gestart op een server patch niet geïnstalleerd.
- Als u van plan bent de functie **Wachtwoordsynchronisatie**gebruiken, moet de server Azure AD Connect op Windows Server 2008 R2 SP1 of hoger.
- De server Azure AD Connect moet [.NET Framework 4.5.1](#component-prerequisites) of hoger en [Microsoft PowerShell 3.0](#component-prerequisites) of hoger is geïnstalleerd.
- Als Active Directory Federation Services wordt ingezet, moet de servers waarop AD FS of Web-toepassingsproxy zijn geïnstalleerd Windows Server 2012 R2 of hoger. [Windows Extern beheer](#windows-remote-management) moet zijn ingeschakeld op deze servers voor installatie op afstand.
- Als Active Directory Federation Services wordt ingezet, moet u [SSL-certificaten](#ssl-certificate-requirements).
- Als Active Directory Federation Services wordt ingezet, moet u [naamresolutie](#name-resolution-for-federation-servers)configureren.
- Azure AD Connect vereist een SQL Server-database voor de opslag identiteitsgegevens. Een SQL Server 2012 Express LocalDB (een light-versie van SQL Server Express) wordt standaard geïnstalleerd en de serviceaccount voor de service is gemaakt op de lokale computer. SQL Server Express heeft een limiet van 10GB waarmee u ongeveer 100.000 objecten te beheren. Als u nodig hebt voor het beheren van een grotere hoeveelheid directory-objecten, moet u de installatiewizard verwijzen naar een andere installatie van SQL Server.
- Als u een aparte SQL Server gebruikt, pas deze vereisten is voldaan:
    - Azure AD Connect ondersteuning biedt voor alle versies van Microsoft SQL Server uit SQL Server 2008 (met SP4) bij SQL Server-2014. Microsoft Azure SQL-Database wordt **niet ondersteund** als een database.
    - U kunt geen niet hoofdlettergevoelig SQL-sortering moet gebruiken. Deze worden aangeduid met een \_CI_ in hun naam. Het is **niet ondersteund** voor het gebruik van een hoofdlettergevoelige sortering, die wordt aangeduid met \_CS_ in hun naam.
    - U kunt slechts één synchronisatie-engine per exemplaar van de database hebben. Het is **niet ondersteund** exemplaar van de database delen met FIM/MIM synchroniseren, DirSync of Azure AD-synchronisatie.

### <a name="accounts"></a>Accounts
- Een Azure AD globaal beheerdersaccount voor de Azure AD-map die u wilt integreren met. Dit moet een **account voor school of organisatie** en kan niet worden een **Microsoft-account**.
- Als u express-instellingen gebruiken of een upgrade vanuit DirSync uitvoert, moet u een Enterprise-beheerdersaccount hebt voor uw lokale Active Directory.
- [Accounts in Active Directory](./connect/active-directory-aadconnect-accounts-permissions.md) als u het installatiepad van aangepaste instellingen gebruiken.

### <a name="azure-ad-connect-server-configuration"></a>Configuratie van de server Azure AD Connect
- Als uw globale beheerders MFA ingeschakeld, moeten klikt u vervolgens de URL- **https://secure.aadcdn.microsoftonline-p.com** zich in de lijst met vertrouwde websites. U wordt gevraagd of u dit toevoegen aan de lijst met vertrouwde websites, als deze niet wordt toegevoegd voordat u wordt gevraagd om een uitdaging MFA. Internet Explorer kunt u deze toevoegen aan uw vertrouwde sites.

### <a name="connectivity"></a>Connectiviteit
- De server Azure AD Connect moet DNS-resolutie voor zowel intranet of internet. De DNS-server moet kunnen zijn voor het oplossen van namen zowel naar uw lokale Active Directory, evenals de Azure AD-eindpunten.
- Als u firewalls op uw Intranet hebt en u moet open poorten tussen de Azure AD Connect-servers en domeincontroller, raadpleegt u [Azure AD Connect-poorten](active-directory-aadconnect-ports.md) voor meer informatie.
- Als uw proxy of firewall limiet URL's kunnen worden geopend, klikt u vervolgens de URL's beschreven in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) moet worden geopend.
    - Als u de Microsoft Cloud in Duitsland of de cloud Microsoft Azure overheid gebruikt, raadpleegt u [exemplaren aandachtspunten voor de service van de synchronisatie van de Azure AD Connect](active-directory-aadconnect-instances.md) voor URL's.
- Azure AD Connect is standaard TLS 1.0 gebruiken om te communiceren met Azure AD. U kunt dit wijzigen naar TLS 1.2 volgens de stappen in [TLS 1.2 inschakelen voor Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
- Als u een uitgaande proxy gebruikt om verbinding te maken met Internet, moet de volgende instelling in het bestand **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** voor de installatiewizard en Azure AD Connect synchronisatie kunnen verbinding maken met Internet en Azure AD worden toegevoegd. Deze tekst moet worden ingevoerd onderaan in het bestand. In deze code &lt;PROXYADRESS&gt; de werkelijke proxy IP-adres of de hostnaam naam vertegenwoordigt.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Als uw proxyserver verificatie is vereist, klikt u vervolgens de [serviceaccount](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) moet zich bevinden in het domein en moet u het installatiepad van aangepaste instellingen gebruiken om op te geven van een [aangepaste service-account](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components). Moet u ook een andere wijziging machine.config. Met deze reageren wijziging in de installatiewizard machine.config en synchronisatie-engine op verificatieaanvragen van de proxyserver. In de installatiewizard van alle worden pagina's, met uitzondering van de pagina **configureren** de ondertekende in gebruikersreferenties gebruikt. Klik op de pagina **configureren** aan het einde van de installatiewizard is de context en het [serviceaccount](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) die is gemaakt door u daarom. De sectie machine.config ziet er als volgt.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Zie MSDN voor meer informatie over de [standaardproxy Element](https://msdn.microsoft.com/library/kd3cf2ex.aspx).

Als u problemen met verbinding hebt, raadpleegt u [problemen oplossen met de verbinding oplossen](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Andere
- Optioneel: Een test gebruikersaccount voor de verificatie van synchronisatie.

## <a name="component-prerequisites"></a>Vereisten voor onderdelen

### <a name="powershell-and-net-framework"></a>PowerShell en .net Framework
Azure AD Connect is afhankelijk van Microsoft PowerShell en .NET Framework 4.5.1. Moet u deze versie of een latere versie op de server is geïnstalleerd. Afhankelijk van uw versie van Windows Server, doet u het volgende:

- Windows Server 2012R2
  - Microsoft PowerShell al dan niet standaard is geïnstalleerd, geen actie moet worden ondernomen.
  - .NET framework 4.5.1 en toekomstige versies worden aangeboden via Windows Update. Zorg ervoor dat u de meest recente updates hebt geïnstalleerd op Windows Server in het Configuratiescherm.
- Windows Server 2008R2 en Windows Server 2012
  - De nieuwste versie van Microsoft PowerShell is beschikbaar in **Windows Management Framework 4.0**, beschikbaar op [Microsoft Downloadcentrum](http://www.microsoft.com/downloads).
  - .NET framework 4.5.1 en toekomstige versies zijn beschikbaar op [Microsoft Downloadcentrum](http://www.microsoft.com/downloads).
- Windows Server 2008
  - De laatste ondersteunde versie van PowerShell is beschikbaar in **Windows Management Framework 3.0**, beschikbaar op [Microsoft Downloadcentrum](http://www.microsoft.com/downloads).
 - .NET framework 4.5.1 en toekomstige versies zijn beschikbaar op [Microsoft Downloadcentrum](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>TLS 1.2 inschakelen voor Azure AD Connect
Azure AD Connect al dan niet standaard voor het coderen van de communicatie tussen de synchronisatie-engine server en Azure AD TLS 1.0 gebruikt. U kunt dit wijzigen door het .net toepassingen configureren voor gebruik TLS 1.2 al dan niet standaard op de server. Meer informatie over TLS 1.2 vindt u in [Microsoft beveiligingsadvies 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. TLS 1.2 kan niet worden ingeschakeld op Windows Server 2008. Moet u Windows Server 2008R2 of hoger. Zorg ervoor dat u hebt de .net 4.5.1 hotfix voor uw besturingssysteem hebt geïnstalleerd, raadpleegt u [Microsoft beveiligingsadvies 2960358](https://technet.microsoft.com/security/advisory/2960358). U hebt dit of een latere versie al geïnstalleerd op uw server.
2. Als u Windows Server 2008R2 gebruikt, klikt u vervolgens controleert u of dat TLS 1.2 is ingeschakeld. In Windows Server 2012-server en latere versies, moet al TLS 1.2 zijn ingeschakeld.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. Voor alle besturingssystemen, stel deze registersleutel en start opnieuw op de server.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Als u ook TLS 1.2 inschakelen tussen de synchronisatie-engine-server en een externe SQL Server wilt, zorg ervoor dat u hebt de vereiste versies voor [de ondersteuning van TLS 1.2 voor Microsoft SQL Server](https://support.microsoft.com/kb/3135244)hebt geïnstalleerd.

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Vereisten voor Federatie installatie en configuratie

### <a name="windows-remote-management"></a>Windows Extern beheer
Als u Azure AD Connect Active Directory Federation Services of de Web-toepassingsproxy implementeren, moet u de volgende eisen om ervoor te zorgen connectiviteit en -configuratie worden uitgevoerd:

- Als de doelserver domein toegevoegd, controleert u of Windows externe beheerde is ingeschakeld
    - Opdracht gebruiken in een verhoogde PSH opdrachtvenster`Enable-PSRemoting –force`
- Als de doelserver een niet-domein gekoppelde WAP-computer is, zijn er een aantal aanvullende vereisten
    - Op de computer van de doeltoepassing (WAP machine):
         - Controleer of de winrm (Windows Extern beheer / WS-Management) service wordt uitgevoerd via de Services-module
         - Opdracht gebruiken in een verhoogde PSH opdrachtvenster`Enable-PSRemoting –force`
    - Op de computer waarop de wizard wordt uitgevoerd (als de doelcomputer niet van het domein gekoppelde of niet-vertrouwde domein is):
        - De opdracht gebruiken in een verhoogde PSH opdrachtvenster`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - In Serverbeheer:
             - DMZ WAP host toevoegen aan groep machines (Serverbeheer-beheren >...-Servers toevoegen > tabblad DNS gebruiken)
             - Tabblad Serverbeheer alle Servers: klik met de rechtermuisknop op WAP server en kies beheren als..., voer lokale (niet domein) referenties voor de machine WAP
             - Voor het valideren van externe PSH connectivity in het tabblad Server Manager alle Servers: klik met de rechtermuisknop op WAP-server en kiest u Windows PowerShell.  Een externe PSH-sessie moet worden geopend om ervoor te zorgen externe PowerShell-sessies kunnen worden vastgesteld.

### <a name="ssl-certificate-requirements"></a>Vereisten voor SSL-certificaat
**Belangrijk:** het is raadzaam het dezelfde SSL-certificaat over alle knooppunten van uw AD FS-farm, evenals alle webtoepassing-proxyservers gebruiken.

- Het certificaat moet een X509 certificaat.
- Klik op de federatieservers in een testomgeving kunt u een zelfondertekend certificaat. We raden echter voor een productieomgeving aan dat u het certificaat bij een openbare Certificeringsinstantie aanvragen.
    - Als een certificaat dat is niet openbaar vertrouwde gebruikt, ervoor zorgen dat het certificaat dat is geïnstalleerd op elke Web Application-proxyserver vertrouwde op de lokale server en klik op alle federatieservers is
- De identiteit van het certificaat moet overeenkomen met de naam van de Federatie service (bijvoorbeeld sts.contoso.com).
    - De identiteit is een van beide onderwerp alternatieve (SAN) extensie van type DNS-naam of, als er geen items SAN, de onderwerpnaam van het opgegeven als een eigennaam.  
    - Meerdere SAN fragmenten kunnen aanwezig zijn in het certificaat, mits een van deze overeenkomt met de naam van de service Federatie.
    - Als u van plan bent om te gebruiken werkplek deelnemen aan een extra SAN is vereist voor de waarde **enterpriseregistration.** gevolgd door het achtervoegsel UPN (User Principal Name) van uw organisatie, bijvoorbeeld **enterpriseregistration.contoso.com**.
- Certificaten op basis van de volgende generatie (CNG) toetsen CryptoAPI en opslag van sleutels providers worden niet ondersteund. Dit betekent dat u een certificaat op basis van een provider (cryptografische serviceprovider) en niet een KSP (opslag van sleutels provider) moet gebruiken.
- Jokertekens certificaten worden ondersteund.

### <a name="name-resolution-for-federation-servers"></a>Naamresolutie voor federatieservers
- DNS-records voor de AD FS-federatie servicenaam (bijvoorbeeld sts.contoso.com) voor zowel het intranet (uw interne DNS-server) en de extranet (openbare DNS via uw domeinregistrar) instellen. Voor het intranet DNS record Zorg ervoor dat u A records en niet de CNAME-records. Dit is vereist voor windows-verificatie van uw domein gekoppelde computer goed.
- Als u meer dan één AD FS-server of Web Application-proxyserver implementeert, klikt u vervolgens de zorg ervoor dat u uw taakverdeling hebt geconfigureerd en of de DNS-records voor de AD FS-federatie servicenaam (bijvoorbeeld sts.contoso.com) naar de taakverdeling verwijzen.
- Zorg ervoor dat de servicenaam van de AD FS-federatie (bijvoorbeeld sts.contoso.com) wordt toegevoegd aan de intranetzone in Internet Explorer voor geïntegreerde windows-verificatie om te werken voor browsertoepassingen met Internet Explorer in uw intranet. Dit kan worden beheerd via Groepsbeleid en geïmplementeerd in alle computers van uw domein toegevoegd.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD Connect ondersteunende onderdelen
Hier volgt een lijst met onderdelen die Azure AD Connect is geïnstalleerd op de server waarop Azure AD Connect is geïnstalleerd. Deze lijst is voor de expresinstallatie van een eenvoudige.  Als u besluit om een andere SQL Server op de installatiepagina van de synchronisatie-services gebruiken, is klikt u vervolgens SQL Express LocalDB niet geïnstalleerd lokaal.

- Azure AD Connect systeemstatus
- Microsoft Online Services-aanmeldhulp voor IT-Professionals (geïnstalleerd, maar geen afhankelijkheid op is geïnstalleerd)
- Microsoft SQL Server 2012 opdrachtregel hulpprogramma 's
- Microsoft SQL Server 2012 Express LocalDB
- Microsoft SQL Server 2012 Native Client
- Microsoft Visual C++ 2013 distributiepakket

## <a name="hardware-requirements-for-azure-ad-connect"></a>Hardwarevereisten voor Azure AD Connect
De volgende tabel ziet de minimale vereisten voor de computer van de synchronisatie van Azure AD Connect.

| Aantal objecten in Active Directory | CPU | Geheugen | Grootte van de harde schijf |
| ------------------------------------- | --- | ------ | --------------- |
| Minder dan 10.000 | 1,6 GHz | 4 GB | 70 GB |
| 10.000 – 50.000 | 1,6 GHz | 4 GB | 70 GB |
| 50.000 – 100.000 | 1,6 GHz | 16 GB | 100 GB |
| Voor 100.000 of meer objecten is de volledige versie van SQL Server vereist|  |  |  |
| 100.000 – 300.000 | 1,6 GHz | 32 GB | 300 GB |
| 300.000 – 600.000 | 1,6 GHz | 32 GB | 450 GB |
| Meer dan 600.000 | 1,6 GHz | 32 GB | 500 GB |

De minimale vereisten voor computers met AD FS of endwebservers van toepassing is als volgt:

- Processor: Twee core 1,6 GHz of hoger
- GEHEUGEN: 2GB of hoger
- Azure VM: A2 configuratie of hoger

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
