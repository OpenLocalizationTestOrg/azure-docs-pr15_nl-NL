<properties
   pageTitle="Azure AD Connect: Accounts en machtigingen | Microsoft Azure"
   description="In dit onderwerp worden de accounts die worden gebruikt en gemaakt en de machtigingen die vereist zijn."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Accounts en machtigingen
De installatiewizard Azure AD Connect biedt twee verschillende paden:

- Express-instellingen wordt in de wizard meer bevoegdheden vereist zodat u kunt deze eenvoudig uw configuratie instellen zonder dat u naar gebruikers maken of machtigingen afzonderlijk configureren.

- De wizard biedt u meer en opties in de aangepaste instellingen, maar er zijn situaties waarin u nodig hebt om ervoor te zorgen dat u over de juiste machtigingen beschikt.

## <a name="related-documentation"></a>Gerelateerde documentatie
Als u de documentatie over de [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](../active-directory-aadconnect.md)niet heeft gelezen, bevat de volgende tabel koppelingen naar verwante onderwerpen.

Onderwerp |  
--------- | ---------
Installeren met Express-instellingen | [Installatie van Azure AD Connect Express](active-directory-aadconnect-get-started-express.md)
Installeren met behulp van aangepaste instellingen | [Aangepaste installatie van Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Upgrade van DirSync | [Een upgrade uitvoert vanuit Azure AD-synchronisatie (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>Instellingen voor installatie Express
Express-instellingen, klik in de installatiewizard wordt gevraagd om beheerdersreferenties AD DS Enterprise zodat uw lokale Active Directory kan worden geconfigureerd met de vereiste machtigingen voor Azure AD Connect. Als u een van DirSync upgrade, worden de AD DS Enterprise beheerders referenties worden gebruikt voor het wachtwoord voor het account dat is gebruikt door DirSync opnieuw instellen. U moet ook Azure AD-hoofdbeheerder referenties.

Pagina van wizard  | Referenties die worden verzameld | Machtigingen vereist| Wordt gebruikt voor
------------- | ------------- |------------- |-------------
N/B|Gebruiker die de installatiewizard| Beheerder van de lokale server| <li>Hiermee maakt u de lokale account die wordt gebruikt als de [synchronisatie-engine serviceaccount](#azure-ad-connect-sync-service-account).
Verbinding maken met Azure AD| Azure AD directory-referenties | De rol van globale beheerder in Azure AD | <li>Synchronisatie in de adreslijst Azure AD inschakelen.</li>  <li>Het maken van de [Azure AD-account](#azure-ad-service-account) dat wordt gebruikt voor continue synchronisatie bewerkingen in Azure AD.</li>
Verbinding maken met AD DS | Lokale Active Directory-referenties | Leden van de Enterprise-beheerders (EA)-groep in Active Directory| <li>Hiermee maakt u een [account](#active-directory-account) in Active Directory en machtigingen verleent. Dit account gemaakt wordt gebruikt om te lezen en schrijven directory-gegevens tijdens de synchronisatie.</li>

### <a name="enterprise-admin-credentials"></a>Enterprise beheerdersreferenties
Deze referenties worden alleen gebruikt tijdens de installatie en worden gebruikt nadat de installatie is voltooid. Het is ondernemingsadministrator en niet Domain Admin, om ervoor te zorgen dat de machtigingen in Active Directory kunnen worden ingesteld in alle domeinen.

### <a name="global-admin-credentials"></a>Globale beheerdersreferenties
Deze referenties worden alleen gebruikt tijdens de installatie en niet worden gebruikt nadat de installatie is voltooid. Wordt gebruikt om de [Azure AD-account](#azure-ad-service-account) gebruikt voor het synchroniseren van wijzigingen aan Azure AD te maken. Het account kunt synchroniseren ook als een functie in Azure AD.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Machtigingen voor de gemaakte AD DS-account voor express-instellingen
De [account](#active-directory-account) hebt gemaakt voor lezen en schrijven aan AD DS heeft de volgende machtigingen wanneer gemaakt express-instellingen:

Machtiging | Wordt gebruikt voor
---- | ----
<li>Directorywijzigingen repliceren</li><li>Repliceren Directory alle gewijzigd | Wachtwoordsynchronisatie
Alleen-lezen/schrijven alle eigenschappen gebruiker | Importeren en Exchange hybride
Alleen-lezen/schrijven alle eigenschappen iNetOrgPerson | Importeren en Exchange hybride
Alleen-lezen/schrijven alle eigenschappen groeperen | Importeren en Exchange hybride
Neem contact op met alleen-lezen/schrijven alle eigenschappen | Importeren en Exchange hybride
Wachtwoord opnieuw instellen | Voorbereiden voor het wachtwoord write-backs inschakelen

## <a name="custom-settings-installation"></a>Aangepaste instellingen installatie
Wanneer u met aangepaste instellingen, moet het account waarmee verbinding maken met Active Directory worden gemaakt voordat u de installatie. De machtigingen die u moet dit account verlenen vindt u in [de AD DS-account maken](#create-the-ad-ds-account).

Pagina van wizard  | Referenties die worden verzameld | Machtigingen vereist| Wordt gebruikt voor
------------- | ------------- |------------- |-------------
N/B | Gebruiker die de installatiewizard|<li>Beheerder van de lokale server</li><li>Als een volledige SQL Server gebruikt, moet de gebruiker zijn systeem beheerder (SA) in SQL</li>| Hiermee maakt u de lokale account die wordt gebruikt als de [synchronisatie-engine serviceaccount](#azure-ad-connect-sync-service-account)standaard. Het account is alleen gemaakt als de beheerder geen een bepaalde account.
Synchronisatieservices, de optie account Service installeren | AD of lokale gebruikersreferenties voor account | Gebruiker, machtigingen worden verleend de installatiewizard | Als de beheerder Hiermee geeft u een account, wordt dit account als de serviceaccount gebruikt voor de synchronisatieservice.
Verbinding maken met Azure AD | Azure AD directory-referenties| De rol van globale beheerder in Azure AD| <li>Synchronisatie in de adreslijst Azure AD inschakelen.</li>  <li>Het maken van de [Azure AD-account](#azure-ad-service-account) dat wordt gebruikt voor continue synchronisatie bewerkingen in Azure AD.</li>
Verbinding maken met uw mappen | Lokale Active Directory-referenties voor elke bos die is gekoppeld aan Azure AD | De machtigingen zijn afhankelijk van welke functies u inschakelen en vindt u in [maken de AD DS-account](#create-the-ad-ds-account) |Dit account wordt gebruikt om te lezen en schrijven directory-gegevens tijdens de synchronisatie.
AD FS-Servers | Voor elke server in de lijst verzamelt de wizard referenties wanneer de aanmeldingsreferenties van de gebruiker de wizard uitgevoerd onvoldoende om verbinding te zijn | De beheerder van het domein | Installatie en configuratie van de functie van de AD FS-server.
Web application-proxyservers |Voor elke server in de lijst verzamelt de wizard referenties wanneer de aanmeldingsreferenties van de gebruiker de wizard uitgevoerd onvoldoende om verbinding te zijn | Lokale beheerder op de computer van de doelsite | Installatie en configuratie van de functie server WAP.
Proxyverificatiegegevens vertrouwen |Federation-service referenties (de referenties de proxy wordt gebruikt om u te registreren voor een certificaat vertrouwen van de FS vertrouwt |Domeinaccount dat is een lokale beheerder van de AD FS-server | Eerste registratie van FS-WAP vertrouwen certificaat.
AD FS-serviceaccount pagina "Gebruik het hulpprogramma voor het account van een domein-Gebruikersoptie" | AD-gebruikersreferenties account | Domeingebruiker | De AD-gebruikersaccount waarvan referenties zijn opgegeven wordt als de aanmeldingsaccount van de AD FS-service gebruikt.

### <a name="create-the-ad-ds-account"></a>De AD DS-account maken
Wanneer u Azure AD Connect installeert, wordt het account dat u opgeeft op de pagina **verbinding maken met uw mappen** aanwezig is in Active Directory en machtigingen zijn vereist. De machtigingen en eventuele problemen zijn alleen gevonden tijdens de synchronisatie wordt niet gecontroleerd door de installatiewizard.

De machtigingen die u nodig hebt, is afhankelijk van de optionele functies inschakelen u. Als u meerdere domeinen hebt, moeten u de machtigingen voor alle domeinen in de bos krijgen. Als u een van deze functies niet inschakelt, zijn de standaardmachtigingen voor het **Domeingebruiker** voldoende.

Functie | Machtigingen
------ | ------
Wachtwoordsynchronisatie | <li>Directorywijzigingen repliceren</li>  <li>Repliceren Directory alle gewijzigd
Hybride implementatie van Exchange | Schrijfmachtigingen voor de kenmerken die wordt beschreven in [Exchange hybride write-backs](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) voor gebruikers, groepen en contactpersonen.
Wachtwoord write-backs | Schrijfmachtigingen voor de kenmerken die wordt beschreven in [aan de slag met wachtwoordbeheer](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) voor gebruikers.
Apparaat write-backs | Machtigingen met een PowerShell-script zoals is beschreven in [apparaat write-backs](../active-directory-aadconnect-feature-device-writeback.md).
Groep write-backs | Lezen, maken, bijwerken en verwijderen van de groepsobjecten in de organisatie-eenheid waar de distributiegroepen moeten zich bevinden.

## <a name="upgrade"></a>Upgrade
Wanneer u een upgrade vanuit één versie van Azure AD Connect naar een nieuwe versie uitvoert, moet u de volgende machtigingen:

Principal | Machtigingen vereist | Wordt gebruikt voor
---- | ---- | ----
Gebruiker die de installatiewizard | Beheerder van de lokale server | Binaire bestanden worden bijgewerkt.
Gebruiker die de installatiewizard | Lid van ADSyncAdmins | Breng wijzigingen aan synchronisatie regels en andere configuratie.
Gebruiker die de installatiewizard | Als u een volledige SQL server gebruikt: DBO (of soortgelijke) van de synchronisatie-engine-database | Wijzigingen aanbrengen, database niveau, zoals tabellen bijwerken met nieuwe kolommen.

## <a name="more-about-the-created-accounts"></a>Meer informatie over de gemaakte accounts

### <a name="active-directory-account"></a>Active Directory-account
Als u express-instellingen gebruikt, is klikt u vervolgens een account gemaakt in Active Directory die wordt gebruikt voor synchronisatie. De account bevindt zich in het hoofddomein in de map gebruikers en heeft de naam ervan voorafgegaan door **MSOL_**. Het account is gemaakt met een lang complexe wachtwoord dat niet verloopt. Als u een wachtwoordbeleid in uw domein hebt, breng weet u zeker lange en complexe wachtwoorden mogen voor dit account.

![AD-account](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Azure AD Connect sync service-accounts
Een lokale serviceaccount wordt gemaakt door de installatiewizard (tenzij u opgeeft dat het account in aangepaste instellingen gebruiken). Het account is voorafgegaan **AAD_** en gebruikt voor de service werkelijke synchroniseren om uit te voeren als. Als u op een domeincontroller Azure AD Connect hebt geïnstalleerd, wordt het account is gemaakt in het domein. Als u een externe server met SQL server of als u een proxy die is verificatie vereist gebruikt, moet de serviceaccount **AAD_** zich bevinden in het domein.

![Synchronisatie-serviceaccount](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Het account is gemaakt met een lang complexe wachtwoord dat niet verloopt.

Dit account wordt gebruikt voor de opslag van de wachtwoorden opnieuw instellen voor de andere accounts in een veilige manier. Deze wachtwoorden van andere accounts zijn opgeslagen in de database is versleuteld. De persoonlijke sleutels voor de versleuteling zijn beveiligd met de versleuteling van cryptografische services geheim-toets met Windows gegevens beveiliging API (DPAPI). U moet het wachtwoord voor de serviceaccount aangezien Windows wordt vervolgens de toetsen versleuteling veiligheidsredenen destroy niet opnieuw instellen.

Als u een volledige SQL Server gebruikt, is de serviceaccount de DBO van de gemaakte database voor de synchronisatie-engine. De service werkt niet zoals bedoeld met een andere machtigingen. Een SQL-aanmelding wordt ook gemaakt.

Het account is ook machtigingen naar bestanden, registersleutels en andere objecten die betrekking hebben op de synchronisatie-Engine.

### <a name="azure-ad-service-account"></a>Azure AD-serviceaccount
Een account in Azure AD is voor de synchronisatieservice gemaakt. Dit account kan worden geïdentificeerd door de naam weer te geven.

![AD-account](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

De naam van de server die op het account wordt gebruikt, kan worden geïdentificeerd in het tweede deel van de gebruikersnaam in te voeren. In de afbeelding is de naam van de server FABRIKAMCON. Als u de tijdelijke servers hebt, heeft elke server een eigen account. Er is een limiet van 10 sync service-accounts in Azure AD.

De serviceaccount is gemaakt met een lang complexe wachtwoord dat niet verloopt. Deze is een speciale rol **Directory-synchronisatie-Accounts** die alleen machtigingen voor het uitvoeren van taken voor directory-synchronisatie heeft verleend. Deze speciale ingebouwde rol kan niet worden toegekend buiten de wizard Azure AD Connect en de portal van Azure dit account met de rol **gebruiker**wordt weergegeven.

![AD-account rol](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](../active-directory-aadconnect.md).
