<properties 
    pageTitle="Aan de slag met de Server Azure meervoudige verificatie"
    description="Dit is de pagina van de Azure meervoudige verificatie die wordt beschreven hoe u aan de slag met Azure MFA Server."
    services="multi-factor-authentication"
    keywords="verificatie-server, azure meerdere factor app activering verificatiepagina verificatie server downloaden"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Aan de slag met de Server Azure meervoudige verificatie




<center>![Cloud](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Nu dat we of gebruik on-premises implementatie meervoudige verificatie hebt vastgesteld, laten we aan de slag. Een nieuwe installatie van de server en het integreren en deze instelling met lokale Active Directory heeft betrekking op deze pagina. Als u al de PhoneFactor server zijn geïnstalleerd en op zoek bent Zie [upgraden naar de Azure meervoudige-Server](multi-factor-authentication-get-started-server-upgrade.md) wilt bijwerken of als u op zoek bent voor informatie over het installeren van de web-service Zie [De webservice van Azure meervoudige verificatie Server mobiele App](multi-factor-authentication-get-started-server-webservice.md).


## <a name="download-the-azure-multi-factor-authentication-server"></a>Download de Server Azure meervoudige verificatie



Er zijn twee verschillende manieren downloaden waarmee u kunt de Server Azure meervoudige verificatie. Beide wordt uitgevoerd via de portal van Azure. De eerste is door de Provider voor meervoudige Auth rechtstreeks beheren. Het tweede is via de service-instellingen. De tweede optie vereist een meervoudige Auth-Provider of een Azure MFA, Azure AD Premium of Enterprise mobiliteit Suite-licentie.


### <a name="to-download-the-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Om te downloaden van de server Azure meervoudige verificatie van de Azure-portal
--------------------------------------------------------------------------------

1. Meld u aan bij de Azure-Portal als beheerder.
2. Selecteer aan de linkerkant, Active Directory.
3. Klik op de pagina Active Directory boven op **Meervoudige Auth Providers**
4. Klik onderaan op **beheren**
5. Hiermee opent u een nieuwe pagina.  Klik op **Downloads.** 
 ![Downloaden](./media/multi-factor-authentication-sdk/download.png)
6. Klik op boven **Activering referenties genereren**, **downloaden.** 
 ![Downloaden](./media/multi-factor-authentication-get-started-server/download4.png)
7. Sla het downloaden.



### <a name="to-download-the-azure-multi-factor-authentication-server-via-the-service-settings"></a>Downloaden van de Server Azure meervoudige verificatie via de service-instellingen


1. Meld u aan bij de Azure-Portal als beheerder.
2. Selecteer aan de linkerkant, Active Directory.
3. Dubbelklikt u op uw exemplaar van Azure AD.
4. Klik op boven **configureren**
![downloaden](./media/multi-factor-authentication-sdk/download2.png)
5. Selecteer onder meervoudige verificatie **service-instellingen beheren**
6. Klik op de servicepagina-instellingen onder aan het scherm op **Ga naar de portal**.
![Downloaden](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Hiermee opent u een nieuwe pagina. Klik op **Downloads.**
8. Klik op boven **Activering referenties genereren**, **downloaden.**
9. Sla het downloaden.




## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Installeren en configureren van de Server Azure meervoudige verificatie
U kunt nu dat u hebt gedownload de server installeren en configureren.  Zorg ervoor dat u of de server die u wilt installeren op voldoet aan de volgende vereisten:



Serververeisten Azure meervoudige verificatie|Beschrijving|
:------------- | :------------- |
Hardware|<li>200 MB vasteschijfruimte</li><li>x32 of x64 kunnen processor</li><li>1 GB of groter RAM</li>
Software|<li>Windows Server 2008 of hoger als de host van een server OS</li><li>Windows 7 of hoger als de host van een client OS</li><li>4.0 van Microsoft .NET Framework</li><li>SDK IIS 7.0 of hoger wanneer de installatie van de gebruiker portal of web-service</li>

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Azure meervoudige verificatie firewall serververeisten
--------------------------------------------------------------------------------
Elke MFA-server moet communiceren in poort 443 uitgaande als volgt uit:

- https://PFD.phonefactor.NET
- https://pfd2.phonefactor.NET
- https://CSS.phonefactor.NET

Als uitgaande firewalls worden beperkt op poort 443, moet de volgende IP-adresbereiken worden geopend:

IP-subnetten|Netmask|IP-bereik
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Als u niet werkt met Azure meervoudige verificatie gebeurtenis bevestiging functies en als gebruikers niet kunnen verifiëren met de meervoudige Auth mobiele apps van apparaten in het bedrijfsnetwerk bevinden de IP bereiken worden verkleind als volgt uit:


IP-subnetten|Netmask|IP-bereik
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Installeren en configureren van de server Azure meervoudige verificatie
--------------------------------------------------------------------------------


1. Dubbelklik op het uitvoerbare bestand. Hiermee wordt de installatie te starten.
2. Zorg ervoor dat de map juist is op het scherm installatiemap selecteren en op volgende.
3. Nadat de installatie is voltooid, klikt u op voltooien.  Hierdoor wordt de configuratiewizard gestart.
4. De configuratiewizard welkomstscherm, schakel dit selectievakje in **de Wizard verificatie configureren voor gebruik van overslaan** en klik op **volgende**.  Hiermee wordt de wizard te sluiten en start de server.
    ![Cloud](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Klik op de knop **Genereren activering referenties** weer op de pagina die wordt gedownload van de server uit.  Kopieer deze informatie naar de Server Azure MFA in de vakken en klik op **activeren**.


Een snelle installatie met de configuratiewizard van weergeven de bovenstaande stappen  U kunt de verificatiewizard opnieuw uitvoeren door deze te selecteren in het menu Extra op de server.



##<a name="import-users-from-active-directory"></a>Gebruikers importeren uit Active Directory

Nu dat de server is geïnstalleerd en geconfigureerd, kunt u snel gebruikers importeren in de Server Azure-MFA.

### <a name="to-import-users-from-active-directory"></a>Om gebruikers te importeren uit Active Directory
--------------------------------------------------------------------------------


1. Selecteer de **gebruikers**in de Server Azure MFA, aan de linkerkant.
2. In de linkerbenedenhoek, selecteert u **importeren uit Active Directory**.
3. Nu kunt u zoeken voor individuele gebruikers of de AD-map met gebruikers in deze zoekt organisatie-eenheden.  In dit geval wordt we de gebruikers OU opgeven.
4. Markeer alle gebruikers aan de rechterkant en klik op **importeren**.  U ontvangt een pop-upvenster mededeling dat u voltooid hebt.  Sluit het venster importeren.

![Cloud](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Gebruikers een e-mailbericht verzenden
Nu dat u uw gebruikers in de server Azure meervoudige verificatie hebt geïmporteerd, wordt de aanbevolen dat u uw gebruikers een e-mailbericht dat deze wordt gemeld verzendt dat ze hebt geregistreerd in meervoudige verificatie.

Er zijn verschillende manieren voor het configureren van uw gebruikers voor het gebruik van meervoudige verificatie met de Server Azure meervoudige verificatie.  Bijvoorbeeld als u telefoonnummers van de gebruikers weten of konden de telefoonnummers importeren in de Server Azure meervoudige verificatie uit de adreslijst van hun bedrijf, het e-mailbericht wordt gebruikers wilt laten weten dat ze zijn geconfigureerd voor het gebruiken van Azure meervoudige verificatie, bieden sommige instructies over het gebruik van Azure meervoudige verificatie en kennis van de gebruiker van het telefoonnummer dat u dat ze hun authenticatie voor ontvangt.  

De inhoud van het e-mailbericht varieert afhankelijk van de methode verificatie dat is ingesteld voor de gebruiker (bijvoorbeeld telefoongesprek, SMS, mobiele app).  Bijvoorbeeld als de gebruiker een PINCODE gebruiken moet wanneer ze wordt geverifieerd, ziet het e-mailbericht deze wat hun aanvankelijke PINCODE is ingesteld op.  Gebruikers moeten zijn meestal hun PINCODE wijzigen tijdens de eerste verificatie.

Als telefoonnummers van gebruikers zijn niet geconfigureerd of geïmporteerd in de Server Azure meervoudige verificatie of gebruikers vooraf geconfigureerde zijn gebruik van de mobiele app voor verificatie, kunt u deze op een e-mailbericht waarmee ze weten dat ze zijn geconfigureerd voor het gebruik van Azure meervoudige verificatie en deze verwijzen wordt naar het voltooien van de registratie van hun account via de Portal Azure meervoudige verificatie gebruiker verzenden.  Een hyperlink worden opgenomen dat de gebruiker klikt op voor toegang tot de Portal van de gebruiker. Wanneer de gebruiker op de hyperlink, wordt hun webbrowser geopend en ze naar van hun bedrijf Azure meervoudige verificatie User Portal.   


### <a name="configuring-email-and-email-templates"></a>E-mail en e-mailsjablonen configureren

U kunt de instellingen voor het verzenden van deze e-mailberichten instellen door te klikken op het pictogram e-mail aan de linkerkant.  Dit is waar u de SMTP-gegevens van uw e-mailserver kunt invoeren en kunt u een raamcontract breed e-mailbericht door een selectievakje toe te voegen aan het venster verzenden naar het selectievakje gebruikers mails verzenden.

![E-mailinstellingen](./media/multi-factor-authentication-get-started-server/email1.png)

Klik op het tabblad E-mail inhoud ziet u alle van de verschillende e-sjablonen die beschikbaar zijn voor het kiezen uit.  Zodat afhankelijk van hoe u uw gebruikers als u wilt gebruiken, meervoudige verificatie hebt geconfigureerd, kunt u de sjabloon kiezen die het beste past bij u.

![E-mailsjablonen](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>De verwerking van de Server Azure meervoudige verificatie op gebruikersgegevens

Wanneer u de meervoudige verificatie MFA () on-premises servers gebruikt, worden de gegevens van een gebruiker wordt opgeslagen in de on-premises implementatie-servers. Geen permanente gebruikersgegevens wordt opgeslagen in de cloud. Wanneer de gebruiker een tweeledige verificatie uitvoert, verzendt de Server MFA gegevens naar de Azure MFA-cloudservice om de verificatie te voeren. Wanneer deze verificatieaanvragen worden verzonden naar de cloudservice, worden de volgende velden in de aanvraag en logboeken verzonden zodat ze beschikbaar in verificatie-/ gebruiksrapporten van de klant zijn. Sommige velden zijn optioneel zodat deze kunnen worden ingeschakeld of uitgeschakeld binnen de meervoudige verificatie-Server. Communicatie van de Server MFA naar de cloudservice MFA gebruikt SSL/TLS via poort 443 uitgaande. Deze velden zijn:

- Unieke ID - gebruikersnaam of interne MFA server-ID
- Eerste en laatste naam - optioneel
- E-mailadres - optioneel
- Telefoonnummer - bij het uitvoeren van een telefoongesprek of SMS-verificatie
- Apparaat token - bij het uitvoeren van de mobiele app-verificatie
- Verificatiemodus
- Verificatieresultaat
- De naam van de Server MFA
- MFA Server IP
- Client IP-indien beschikbaar.



Naast de bovenstaande velden met is de verificatieresultaat (success/weigering) en de reden voor elke weigeringen ook opgeslagen met de verificatiegegevens en beschikbaar via de verificatie-/ gebruiksrapporten.


## <a name="advanced-azure-multi-factor-authentication-server-configurations"></a>Geavanceerde configuraties van de Server Azure meervoudige verificatie
Gebruik de onderstaande tabel voor meer informatie over geavanceerde configuratie en configuratiegegevens.

Methode|Beschrijving
:------------- | :------------- |
[User Portal](multi-factor-authentication-get-started-portal.md)|  Informatie over het instellen en configureren van de gebruiker portal inclusief implementatie en selfservice-gebruiker.
[Active Directory Federation Services](multi-factor-authentication-get-started-adfs.md)|Informatie over het instellen van Azure meervoudige verificatie met AD FS.
[RADIUS-verificatie](multi-factor-authentication-get-started-server-radius.md)|  Informatie over het instellen en configureren van de Server Azure-MFA met straal.
[IIS-verificatie](multi-factor-authentication-get-started-server-iis.md)|Informatie over het instellen en configureren van de Azure MFA Server met IIS.
[Windows-verificatie](multi-factor-authentication-get-started-server-windows.md)|  Informatie over het instellen en configureren van de Azure MFA Server met Windows-verificatie.
[LDAP-verificatie](multi-factor-authentication-get-started-server-ldap.md)|Informatie over het instellen en configureren van de Server Azure-MFA met LDAP-verificatie.
[Extern bureaublad Gateway en Azure meervoudige verificatieserver RADIUS gebruiken](multi-factor-authentication-get-started-server-rdg.md)|  Informatie over het instellen en configureren van de Azure MFA Server met extern bureaublad-Gateway RADIUS gebruiken.
[Synchroniseren met Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)|Informatie over het instellen en configureren van synchronisatie tussen Active Directory en de Server Azure-MFA.
[De mobiele App-Web-Service van Azure meervoudige verificatie Server implementeren](multi-factor-authentication-get-started-server-webservice.md)|Informatie over het instellen en configureren van de Azure MFA server-webservice.
