<properties
    pageTitle="Active Directory Federation Services Management en aanpassingen met Azure AD Connect | Microsoft Azure"
    description="AD FS-beheer bij Azure AD Connect en aanpassingen van AD FS aanmeldingsproblemen gebruikerservaring met Azure AD Connect en PowerShell."
    keywords="AD FS, ADFS, AD FS beheer AAD verbinding kunt maken, verbinding te maken, aanmelden, AD FS aanpassing, vertrouwen, O365, Federatie, gebruikmakende partij herstellen"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Active Directory Federation Services management en aanpassingen met Azure AD Connect

In dit artikel details taken verband met Active Directory Federation Services (AD FS) die kunnen worden uitgevoerd met behulp van Microsoft Azure Active Directory-verbinding, plus andere algemene AD FS-taken die nodig zijn voor een volledige configuratie van een AD FS-farm.

| Onderwerp | Wat deze bedekt |
|:------|:-------------|
|**AD FS-management.**|
|[De optie herstellen](#repairthetrust)| De optie Federatie met Office 365 herstellen |
|[Een AD FS-server toevoegen](#addadfsserver) | Gegevensniveaus uitvouwen de AD FS-farm met een extra AD FS-server|
|[Een AD FS op het web proxy toepassingsserver toevoegen](#addwapserver) | Gegevensniveaus uitvouwen de AD FS-farm met een extra WAP-server|
|[Een gefedereerd domein toevoegen](#addfeddomain)| Een gefedereerd domein toevoegen|
| **AD FS aanpassing**|
|[Een aangepaste bedrijfslogo of afbeelding toevoegen](#customlogo)| Een AD FS-aanmeldingspagina met een bedrijfslogo en de afbeelding aanpassen |
|[Een beschrijving aanmeldingsproblemen toevoegen](#addsignindescription) | Een beschrijving van de aanmeldingspagina toevoegen |
|[Regels voor het claimen van AD FS wijzigen](#modclaims) | AD FS vorderingen die voortvloeien uit verschillende Federatie-scenario's wijzigen |

## <a name="ad-fs-management"></a>AD FS-management.

Azure AD Connect vindt u verschillende taken met betrekking tot AD FS die kan worden uitgevoerd met behulp van de wizard Azure AD Connect met minimale tussenkomst. Nadat u klaar bent met het installeren van Azure AD Connect door de wizard uit te voeren, kunt u de wizard opnieuw uitvoeren van extra taken kunt uitvoeren.

### De optie herstellen<a name=repairthetrust></a>

Azure AD Connect kunt controleren op de huidige status van de vertrouwde AD FS en Azure Active Directory en passende acties de optie herstellen uitvoeren. Volg deze stappen om uw Azure AD herstellen en AD FS vertrouwen.

1. Selecteer **herstellen AAD en ADFS vertrouwen** in de lijst met extra taken.
![AAD en ADFS repareren vertrouwen](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. Klik op de pagina **verbinding maken met Azure AD** uw referenties globale beheerder voor Azure AD, en klik op **volgende**.
![Verbinding maken met Azure AD](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Voer de referenties voor de beheerder van het domein op de pagina **RAS-referenties** .
![RAS-referenties](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Nadat u op **volgende**hebt geklikt, wordt Azure AD Connect controleren op certificaat gezondheid en weergeven van eventuele problemen.

    ![Status van certificaten](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    De pagina **gereed is voor het configureren van** wordt de lijst met acties die worden uitgevoerd om te herstellen van het Vertrouwenscentrum weergegeven.

    ![Klaar om te configureren](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Klik op **installeren** om te herstellen van het Vertrouwenscentrum.

>[AZURE.NOTE] Azure AD Connect kunnen alleen reparatie of act op certificaten die zelf-ondertekend. Azure AD Connect niet certificaten van derden worden hersteld.

### Een AD FS-server toevoegen<a name=addadfsserver></a>

> [AZURE.NOTE] Azure AD Connect is vereist voor het PFX-certificaatbestand om toe te voegen een AD FS-server. Alleen als u de AD FS-farm met behulp van Azure AD Connect hebt geconfigureerd, kunt u dus deze bewerking uitvoeren.

1. **Een extra federatieserver Deploy** en klik op **volgende**.
![Aanvullende federatieserver](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. Klik op de pagina **verbinding maken met Azure AD** Geef uw referenties globale beheerder voor Azure AD en klik op **volgende**.
![Verbinding maken met Azure AD](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Geef het domein beheerdersreferenties.
![Domeinbeheerdersreferenties](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure AD Connect wordt gevraagd het wachtwoord van het PFX-bestand dat u hebt opgegeven bij het configureren van uw nieuwe AD FS-farm met Azure AD Connect. Klik op **Wachtwoord invoeren** om te bieden van het wachtwoord voor het PFX-bestand.
![Certificaatwachtwoord](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![SSL-certificaat opgeven](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Voer de servernaam of IP-adres moet worden toegevoegd aan de AD FS-farm op de pagina **AD FS-Servers** .
![AD FS-servers](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Klik op **volgende** en leest u over de laatste pagina van de **configureren** . Nadat Azure AD Connect is uitgevoerd met de servers toevoegen aan de AD FS-farm, krijgt u de optie om te controleren of de connectiviteit.
![Klaar om te configureren](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Installatie voltooid](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Een AD FS op het web proxy toepassingsserver toevoegen<a name=addwapserver></a>

> [AZURE.NOTE] Azure AD Connect is vereist voor het PFX-certificaatbestand om toe te voegen een web application-proxyserver. Daarom kunnen u voor deze bewerking alleen als u de AD FS-farm met behulp van Azure AD Connect hebt geconfigureerd.

1. Selecteer **Web toepassingsproxy implementeren** in de lijst met beschikbare taken.
![Web toepassingsproxy implementeren](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Geef de referenties Azure globale beheerder.
![Verbinding maken met Azure AD](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Klik op de pagina **opgeven SSL-certificaat** moet u het wachtwoord opgeven voor het PFX-bestand dat u hebt opgegeven toen de AD FS-farm configureren met Azure AD Connect.
![Certificaatwachtwoord](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![SSL-certificaat opgeven](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. De server moet worden toegevoegd als een web-toepassingsproxy toevoegen. Omdat de proxyserver van de web-toepassing kan niet worden toegevoegd aan het domein, vraagt de wizard voor beheerdersreferenties op de server wordt toegevoegd.
![Beheerserver referenties](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. Klik op de pagina **Proxy vertrouwen referenties** uw beheerdersreferenties opgeven om de proxy-vertrouwensrelatie configureren en te krijgen tot de primaire server in de AD FS-farm.
![Proxyverificatiegegevens vertrouwen](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. Klik op de pagina **gereed is voor het configureren van** ziet de wizard u de lijst met acties die wordt uitgevoerd.
![Klaar om te configureren](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Klik op **installeren** om te voltooien van de configuratie. Wanneer de configuratie voltooid is, kunt u in de wizard de optie om te controleren of de verbinding met de servers. Klik op **verifiëren** als u wilt controleren connectivity.
![Installatie voltooid](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Een gefedereerd domein toevoegen<a name=addfeddomain></a>

Het is eenvoudig een domein worden federatieve met Azure AD met behulp van Azure AD Connect toe te voegen. Azure AD Connect wordt toegevoegd het domein voor Federatie en de regels voor het claimen overeenkomen met de uitgever als er meerdere domeinen die zijn gekoppeld aan Azure AD wijzigt.

1. Als u wilt een federatieve domein hebt toegevoegd, selecteert u de taak **toevoegen een extra domein Azure AD**.
![Aanvullende Azure AD-domein](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Klik op de volgende pagina van de wizard Azure AD voorzien de referenties van de globale beheerder.
![Verbinding maken met Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. Geef het domein beheerdersreferenties op de pagina **RAS-referenties** .
![RAS-referenties](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. Klik op de volgende pagina, de wizard vindt u een overzicht van Azure AD-domeinen waarmee u uw on-premises adreslijst kan communiceren. Kies het domein in de lijst.
![Azure AD-domein](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Nadat u het domein hebt gekozen, krijgt de wizard u met de benodigde informatie aangaande verder acties die de wizard en de invloed van de configuratie. In sommige gevallen, als u een domein dat nog niet is geverifieerd in Azure AD, selecteert u krijgt de wizard u informatie om te helpen u Verifieer het domein. Zie [toevoegen de naam van uw aangepaste domein met Azure Active Directory](active-directory-add-domain.md) voor meer informatie.

5. Klik op **volgende**en de pagina **gereed is voor het configureren van** de lijst met acties die Azure AD Connect uitvoert wordt weergegeven. Klik op **installeren** om te voltooien van de configuratie.
![Klaar om te configureren](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS aanpassing

De volgende secties vindt meer informatie over een aantal algemene taken die u u moet mogelijk bij het aanpassen van de aanmeldingspagina van AD FS.

### Een aangepaste bedrijfslogo of afbeelding toevoegen<a name=customlogo></a>

Als u wilt wijzigen van het logo van het bedrijf dat wordt weergegeven op de pagina **aanmelden** , gebruik de volgende Windows PowerShell-cmdlet- en syntaxisfouten.

> [AZURE.NOTE] De aanbevolen afmetingen voor het logo zijn 260 x 35 @ 96 dpi met een bestandsgrootte niet groter is dan 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] De parameter *doelnaam* is vereist. Het standaardthema die is uitgebracht met AD FS heet standaard.


### Een beschrijving aanmeldingsproblemen toevoegen<a name=addsignindescription></a>

Voeg een beschrijving van de aanmeldingspagina naar de **aanmeldingspagina**, via de volgende Windows PowerShell-cmdlet- en syntaxisfouten.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### Regels voor het claimen van AD FS wijzigen<a name=modclaims></a>

AD FS ondersteunt een uitgebreide claimen taal die u gebruiken kunt om aangepaste claim regels te maken. Zie [De functie van de taal van de regel claimen](https://technet.microsoft.com/library/dd807118.aspx)voor meer informatie.

De volgende gedeelten wordt beschreven hoe u aangepaste regels voor bepaalde scenario's die betrekking hebben op Azure AD kunt schrijven en AD FS-federatie.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Onveranderlijke-ID van een waarde die wordt in het kenmerk voorwaardelijke

Azure AD Connect kunt u een kenmerk moet worden gebruikt als een bron-anker wanneer objecten zijn gesynchroniseerd met Azure AD opgeven. Als de waarde in het aangepaste kenmerk niet leeg is, wilt u mogelijk een onveranderlijke ID claimen actie. U mogelijk bijvoorbeeld **ms-ds-consistencyguid** selecteert als het kenmerk voor het anker voor de bron en wilt verlenen **ImmutableID** als **ms-ds-consistencyguid** geval het kenmerk heeft waarde tegen. Als er geen waarde ten opzichte van het kenmerk, actie- **objectGuid** als de onveranderlijke-ID.  U kunt de set aangepaste claim regels zoals is beschreven in de volgende sectie maken.

**Regel 1: Querykenmerken**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

U kunt de waarden van **ms-ds-consistencyguid** en **objectGuid** voor de gebruiker in Active Directory wilt zoeken in deze regel. Wijzig de naam van de store in de naam van een juiste store in de AD FS-implementatie. Het type claims ook wijzigen op het type BEGINLETTERS claims voor uw Federatie zoals gedefinieerd voor **objectGuid** en **ms-ds-consistencyguid**.

Ook via **toevoegen** en geen **probleem**, u voorkomen dat een uitgaande probleem voor de entiteit toevoegen en de waarden kunt gebruiken als tussenliggende waarden. U verleent de claim in een regel hoger nadat u tot stand welke waarde brengen wilt gebruiken als de onveranderlijke-ID.

**Regel 2: Controleren of ms-ds-consistencyguid aanwezig is voor de gebruiker**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Deze regel definieert een tijdelijke vlag met de naam van **idflag** die is ingesteld op **useguid** als er geen **ms-ds-concistencyguid** voor de gebruiker zijn ingevuld. De logica achter dit is het feit dat AD FS kan geen lege claims. Dus wanneer u claims http://contoso.com/ws/2016/02/identity/claims/objectguid en http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid in regel 1 toevoegt, u uiteindelijk met een **msdsconsistencyguid** claimen alleen als de waarde voor de gebruiker wordt gevuld. Als dit niet is gevuld, ziet de AD FS dat deze een lege waarde heeft en deze direct te worden. Alle objecten heeft **objectGuid**, zodat deze claim blijft bestaan nadat regel 1 is uitgevoerd.

**Regel 3: Ms-ds-consistencyguid als onveranderlijke ID actie als dit aanwezig is**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Dit is een impliciete **bestaan** controle. Als de waarde voor het claimen bestaat, klikt u vervolgens probleem dat als de onveranderlijke-ID. Het vorige voorbeeld wordt de claim **nameidentifier** gebruikt. U moet dit claimtype wijzigen in de juiste voor onveranderlijke-ID in uw omgeving.

**Regel 4: Actie als onveranderlijke ID-objectGuid als ms-ds-consistencyGuid ontbreekt**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

In deze regel controleert u gewoon het tijdelijke vlag **idflag**. U beslist of u de actie het claimen op basis van de waarde.

> [AZURE.NOTE] De volgorde van deze regels is belangrijk.

#### <a name="sso-with-a-subdomain-upn"></a>Eenmalige aanmelding met een subdomein UPN

U kunt meerdere domeinen worden federatieve met behulp van Azure AD Connect, zoals is beschreven in het [toevoegen van een nieuwe gefedereerd domein](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain)toevoegen. U moet de UPN-claim wijzigen zodat de uitgever-ID komt met het hoofddomein en niet het subdomein overeen, omdat het federatieve hoofddomein ook de onderliggende behandelt.

De regel claimen voor ID van de uitgever is standaard ingesteld als:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Standaard uitgever-ID claimen](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

De standaardregel gewoon de UPN-achtervoegsel duurt en wordt deze gebruikt in de uitgever-ID claimen. Bijvoorbeeld, John is een gebruiker in sub.contoso.com en contoso.com wordt gekoppeld aan Azure AD. John voert john@sub.contoso.com als de naam van de gebruiker tijdens het aanmelden bij Azure AD, en de regel in AD FS selectiegrepen deze in de volgende wijze claimen uitgever-ID.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Claimen waarde:** http://sub.contoso.com/adfs/services/trust/

Als u wilt dat alleen het hoofddomein in de waarde van de uitgever claim, wijzig de regel claimen zodat deze overeenkomen met de volgende.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [aanmelden gebruikersopties](active-directory-aadconnect-user-signin.md).
