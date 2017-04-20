<properties
    pageTitle="MFA Server met Windows Server 2012 R2 AD FS | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe aan de slag met Azure meervoudige verificatie en AD FS in Windows Server 2012 R2."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>


# <a name="secure-your-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>Beveiligen van uw cloud en on-premises bronnen Azure meervoudige verificatieserver gebruikt met AD FS in Windows Server 2012 R2

Als u Active Directory Federation Services (AD FS) en beveiligde cloud of on-premises implementatie resources wilt, kunt u Azure meervoudige verificatieserver voor gebruik met AD FS configureren. Deze configuratie wordt geactiveerd verificatie in twee stappen voor hoogwaardige eindpunten.

In dit artikel bespreken we Azure meervoudige verificatieserver gebruikt met AD FS in Windows Server 2012 R2. Lees voor meer informatie over het [beveiligde cloud en on-premises bronnen via Azure meervoudige verificatieserver met AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multi-factor-authentication-server"></a>Secure Windows Server 2012 R2 AD FS met Azure meervoudige verificatie-Server

Wanneer u Azure meervoudige verificatieserver hebt geïnstalleerd, hebt u de volgende opties:

- Azure meervoudige verificatieserver lokaal installeren op dezelfde als de AD FS-server
- Installeren van de Azure meervoudige verificatie-adapter lokaal op de AD FS-server, en vervolgens meervoudige verificatieserver op een andere computer

Voordat u begint, worden op de hoogte van de volgende informatie:

- U bent niet verplicht voor het installeren van Azure meervoudige verificatieserver op de AD FS-server. U moet de meervoudige verificatie-adapter installeren voor AD FS op een Windows Server 2012 R2 met AD FS. U kunt de server op een andere computer installeren als dit een ondersteunde versie is en u de AD FS-adapter afzonderlijk op uw server AD FS-federatie installeren. Zie de volgende procedures om te leren hoe de adapter afzonderlijk installeren.

- Wanneer de Server MFA AD FS-adapter is ontworpen, is deze verwacht dat AD FS de naam van de gebruikmakende partij aan de adapter doorgeven. De partijnaam van de gebruikmakende kan vervolgens worden gebruikt als de naam van een toepassing. Echter dit bleken niet moeten de hoofdletters/kleine letters. Als uw organisatie wordt gebruikt voor SMS-bericht of mobiele app verificatiemethoden te gebruiken, wordt in de tekenreeksen die is gedefinieerd in de instellingen van het bedrijf een tijdelijke aanduiding, < $$*toepassingsnaam*> bevatten. Deze waarde wordt niet automatisch vervangen wanneer u de AD FS-adapter gebruiken. Het is raadzaam dat u de tijdelijke aanduiding uit de juiste tekenreeksen verwijderen wanneer u AD FS beveiligen.

- Het account waarmee u zich aanmelden moet de gebruiker machtigen voor het maken van beveiligingsgroepen in uw Active Directory-service.

- De installatiewizard voor meervoudige verificatie AD FS-adapter Hiermee maakt u een beveiligingsgroep naam PhoneFactor Admins in uw exemplaar van Active Directory. Vervolgens wordt de AD FS-serviceaccount van uw service Federatie toegevoegd aan deze groep. Het is raadzaam dat u op uw domeincontroller verifiëren dat de groep PhoneFactor Admins daadwerkelijk wordt gemaakt en dat de AD FS-serviceaccount deel uit van deze groep maakt. Indien nodig, moet u handmatig de AD FS-account toevoegen aan de groep PhoneFactor Admins op uw domeincontroller.

- Lees meer over voor informatie over het installeren van de Web-Service SDK met de portal van de gebruiker [de gebruiker-portal voor Azure meervoudige verificatieserver implementeren.](multi-factor-authentication-get-started-portal.md)


### <a name="install-azure-multi-factor-authentication-server-locally-on-the-ad-fs-server"></a>Azure meervoudige verificatieserver lokaal installeren op de AD FS-server

1. Download en installeer Azure meervoudige verificatieserver op de AD FS-server. Lees meer over het [aan de slag met Azure meervoudige verificatieserver](multi-factor-authentication-get-started-server.md)voor informatie over de installatie.
2. In de beheerconsole van Azure meervoudige verificatieserver, klikt u op de **AD FS** -pictogram en selecteer vervolgens de opties **toestaan gebruiker registratie** en **gebruikers toestaan deel te methode selecteren**.
3. Selecteer desgewenst nog meer opties die u wilt opgeven voor uw organisatie.
4. Klik op de **AD FS-Adapter installeren**.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Als het Active Directory-venster wordt weergegeven, betekent dit dat twee dingen. Uw computer is verbonden met een domein en de Active Directory-configuratie voor het beveiligen van de communicatie tussen de AD FS-adapter en de meervoudige verificatie-service is niet volledig. Klik op **volgende** om automatisch voltooien van deze configuratie, of Selecteer de **Automatische configuratie van Active Directory overslaan en configureren van instellingen handmatig** selectievakje in en klik vervolgens op **volgende**.
6. Als de lokale groep-vensters wordt weergegeven, betekent dit dat twee dingen. Uw computer is niet verbonden met een domein en de lokale groepsconfiguratie voor het beveiligen van de communicatie tussen de AD FS-adapter en de meervoudige verificatie-service is niet volledig. Klik op **volgende** om automatisch voltooien van deze configuratie, of Selecteer de **Automatische configuratie van de lokale groep overslaan en configureren van instellingen handmatig** selectievakje in en klik vervolgens op **volgende**.
7. Klik in de installatiewizard op **volgende**. Azure meervoudige verificatieserver wordt gemaakt van de groep PhoneFactor Admins en voegt de AD FS-serviceaccount aan de groep PhoneFactor Admins.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Klik op **volgende**op de pagina **Installer starten** .
9. Klik in het installatieprogramma meervoudige verificatie AD FS-adapter op **volgende**.
10. Klik op **sluiten** wanneer de installatie is voltooid.
11. Wanneer de adapter is geïnstalleerd, moet u deze met AD FS registreren. Open Windows PowerShell en voer de volgende opdracht:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Als u wilt uw nieuw ingeschreven adapter gebruiken, door het beleid globale verificatie in AD FS te bewerken. In de AD FS-beheerconsole, gaat u naar het knooppunt **Verificatiebeleid** . Klik op de koppeling **bewerken** naast de sectie **Globale instellingen** in de sectie **Meervoudige verificatie** . Klik in het venster **Algemene verificatie-beleid bewerken** **Meervoudige verificatie** als een extra verificatiemethode selecteren en klik vervolgens op **OK**. De adapter is geregistreerd als WindowsAzureMultiFactorAuthentication. Start de AD FS-service voor de registratie zijn doorgevoerd.

<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

In dit stadium is meervoudige verificatieserver ingesteld als een extra verificatie-provider voor gebruik met AD FS.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Een zelfstandige exemplaar van de AD FS-adapter installeren met behulp van de Web-Service SDK
1. Installeer de Web-Service SDK op de server waarop meervoudige verificatieserver wordt uitgevoerd.
2. Kopieer de volgende bestanden uit de \Program Files\Multi-Factor verificatieserver-adreslijst op de server waarop u van plan bent de AD FS-adapter installeren:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. Het installatiebestand MultiFactorAuthenticationAdfsAdapterSetup64.msi uitvoeren.
4. Klik in het installatieprogramma meervoudige verificatie AD FS-adapter op **volgende** om de installatie te starten.
5. Klik op **sluiten** wanneer de installatie is voltooid.

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>Bewerk het bestand MultiFactorAuthenticationAdfsAdapter.config

Volg deze stappen om het bestand MultiFactorAuthenticationAdfsAdapter.config bewerken:

1. Het knooppunt **UseWebServiceSdk** ingesteld op **waar**.  
2. Stel de waarde voor **WebServiceSdkUrl** naar de URL van de meervoudige verificatie Web Service SDK. Bijvoorbeeld: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, waarbij certificatename de naam van uw certificaat.  
3. De Register-MultiFactorAuthenticationAdfsAdapter.ps1-script bewerken door toe te voegen *- ConfigurationFilePath &lt;pad&gt; * naar het einde van de `Register-AdfsAuthenticationProvider` opdracht, waar * &lt;pad&gt; * het volledige pad naar het bestand MultiFactorAuthenticationAdfsAdapter.config.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>De SDK van Web-Service configureren met een gebruikersnaam en wachtwoord

Er zijn twee opties voor het configureren van de Web-Service SDK. De eerste is met een gebruikersnaam en wachtwoord, de tweede met een clientcertificaat. Volg deze stappen voor de eerste optie of gaat u verder voor het tweede.  

1. Stel de waarde voor **WebServiceSdkUsername** bij een account dat deel uitmaakt van de beveiligingsgroep van PhoneFactor Admins. Gebruik de &lt;domein&gt;& #92; &lt;gebruikersnaam in te voeren&gt; opmaken.  
2. Stel de waarde voor **WebServiceSdkPassword** op wachtwoord van het juiste account.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>De SDK van Web-Service configureren met een clientcertificaat

Als u niet dat een gebruikersnaam en wachtwoord wilt, volgt u deze stappen om de SDK van Web-Service configureren met een clientcertificaat.

1. Een clientcertificaat aanvragen bij een certificeringsinstantie voor de server waarop de SDK van Web Service wordt uitgevoerd. Informatie over het [verkrijgen van certificaten](https://technet.microsoft.com/library/cc770328.aspx).  
2. Het clientcertificaat importeren naar de lokale computer persoonlijk certificaat dat is store op de server waarop de SDK van Web Service wordt uitgevoerd. Zorg ervoor dat de certificeringsinstantie openbare certificaat zich in vertrouwde basiscertificaten certificaat store.  
3. De openbare en persoonlijke sleutels van het clientcertificaat exporteren naar een .pfx-bestand.  
4. De openbare sleutel in de indeling Base64 exporteren naar een .cer-bestand.  
5. In Serverbeheer controleren of de functie webserver (IIS) \Web Server\Security\IIS toewijzing verificatie via clientcertificaat is geïnstalleerd. Als dit niet is geïnstalleerd, selecteert u **rollen toevoegen en functies** toe te voegen voor deze functie.  
6. **Configuratie-Editor** in IIS-beheer Dubbelklik in de website met de Web-Service SDK virtuele map. Het is belangrijk hiervoor op het niveau van de website en niet op het niveau virtuele map.  
7. Ga naar de sectie **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. Stel ingeschakeld in **waar**.  
9. OneToOneCertificateMappingsEnabled ingesteld op **waar**.  
10. Klik op de knop **...** naast oneToOneMappings en klik vervolgens op de koppeling **toevoegen** .  
11. Open het Base64 .cer-bestand die u eerder hebt geëxporteerd. Verwijder *---beginnen certificaat---*, *---BEËINDIGEN certificaat---*en eventuele regeleinden. Kopieer de resulterende tekenreeks.  
12. Set-certificaat op de tekenreeks in de vorige stap hebt gekopieerd.  
13. Stel ingeschakeld in **waar**.  
14. UserName instellen bij een account dat deel uitmaakt van de beveiligingsgroep van PhoneFactor Admins. Gebruik de &lt;domein&gt;& #92; &lt;gebruikersnaam in te voeren&gt; opmaken.  
15. Stel het wachtwoord op het juiste accountwachtwoord en sluit configuratie-Editor.  
16. Klik op de koppeling **toepassen** .  
17. Dubbelklik in de virtuele map Web Service SDK op **verificatie**.  
18. Controleer of dat ASP.NET-imitatie en basisverificatie zijn ingesteld op **ingeschakeld**en dat alle andere items zijn ingesteld op **uitgeschakeld**.  
19. Dubbelklik in de virtuele map Web Service SDK op **SSL-instellingen**.  
20. Certificaten ingesteld op **accepteren**en klik vervolgens op **toepassen**.  
21. Kopieer de .pfx-bestand dat u eerder hebt geëxporteerd naar de server waarop de AD FS-adapter wordt uitgevoerd.  
22. Het .pfx-bestand importeren naar de lokale computer persoonlijk certificaat dat is store.  
23. Klik met de rechtermuisknop en selecteer **Persoonlijke sleutels beheren**en vervolgens leestoegang tot het account waarmee u aan te melden bij de AD FS-service.  
24. Open het clientcertificaat en kopieer de vingerafdruk van het tabblad **Details** .  
25. In het bestand MultiFactorAuthenticationAdfsAdapter.config, kiest u **WebServiceSdkCertificateThumbprint** de tekenreeks in de vorige stap hebt gekopieerd.  


Ten slotte, als u wilt de adapter hebt geregistreerd, uitvoeren de \Program Files\Multi-Factor verificatie Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 script in PowerShell. De adapter is geregistreerd als WindowsAzureMultiFactorAuthentication. Start de AD FS-service voor de registratie zijn doorgevoerd.

## <a name="related-topics"></a>Verwante onderwerpen

Zie de [Veelgestelde vragen Azure meervoudige verificatie](multi-factor-authentication-faq.md) voor het oplossen van problemen
