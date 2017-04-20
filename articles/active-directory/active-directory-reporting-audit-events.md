<properties
   pageTitle="Rapport gebeurtenissen van Azure Active Directory controleren | Microsoft Azure"
   description="Gebeurtenissen die beschikbaar zijn voor het weergeven en downloaden uit uw Azure Active Directory gecontroleerd"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Rapport gebeurtenissen van Azure Active Directory controleren

*Deze documentatie maakt deel uit van de [Azure Active Directory rapportage Guide] (actief-directory-rapportage-guide.md).*

Het Auditlograpport Azure Active Directory helpt klanten bevoegdheden acties die zijn aangebracht in hun Azure Active Directory identificeren. Bevoegdheden acties opnemen leiden tot onrechtmatige uitbreiding wijzigingen (bijvoorbeeld rol maken of opnieuw instellen van wachtwoorden), veranderende beleidsconfiguraties (bijvoorbeeld wachtwoordbeleidsregels) of wijzigingen in de configuratie van de directory (bijvoorbeeld wijzigingen in domeininstellingen Federatie). De rapporten bevatten de controlerecord voor de naam van de gebeurtenis, de acteur die de actie, de doel-resource beïnvloed door de wijziging en de datum en tijd (in UTC) heeft uitgevoerd. Klanten kunnen ophalen van de lijst met gebeurtenissen controleren voor hun Azure Active Directory via de [Portal van Azure](https://portal.azure.com/), zoals beschreven in de [weergave uw logboeken controleren](active-directory-reporting-azure-portal.md).


## <a name="list-of-audit-report-events"></a>Lijst met gebeurtenissen van rapport controleren
<!--- audit event descriptions should be in the past tense --->

Gebeurtenissen                               | Gebeurtenisbeschrijving
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Gebruikersgebeurtenissen**                      |
Gebruiker toevoegen                             | Een gebruiker toegevoegd aan de map.
Gebruiker verwijderen                          | Een gebruiker verwijderd uit de adreslijst.
Eigenschappen van de licentie instellen               | Stel de eigenschappen van de licentie voor een gebruiker in de adreslijst.
Gebruikerswachtwoord opnieuw instellen                  | Het wachtwoord voor een gebruiker in de map opnieuw instellen.
Wachtwoord van de gebruiker wijzigen                 | Het wachtwoord voor een gebruiker in de map hebt gewijzigd.
Gebruikerslicentie wijzigen                  | De licentie is toegewezen aan een gebruiker in de adreslijst gewijzigd. Als u wilt zien welke licenties zijn bijgewerkt, kijkt u naar de onderstaande [Update gebruiker](#update-user-attributes) -eigenschappen
Gebruiker bijwerken                          | Een gebruiker in de map bijgewerkt. [Zie hieronder](#update-user-attributes) voor kenmerken die kunnen worden bijgewerkt.
Gebruikerswachtwoord van set dwingen wijzigen       | Stel de eigenschap die forceert u een gebruiker wijzigen van hun wachtwoord op login.
Gebruikersreferenties bijwerken        | Gebruiker het wachtwoord gewijzigd  
**Groepsgebeurtenissen**                     |
Groep toevoegen                            | Een groep hebt gemaakt in de adreslijst.
Groep bijwerken                         | Een groep in de map bijgewerkt. Als u wilt zien welke eigenschappen voor groep zijn bijgewerkt, verwijzen naar de [Groep eigenschappen gecontroleerd](#update-group-attributes) in het gedeelte hierna
Groep verwijderen                         | Een groep verwijderd uit de adreslijst.
Lid toevoegen aan groep            | Een lid aan een groep in de directory toegevoegd.
Lid verwijderen uit groep         | Een lid verwijderen uit een groep in de adreslijst.
CreateGroupSettings              | Groepsinstellingen
UpdateGroupSettings          | Groepsinstellingen bijgewerkt. Als u wilt zien welke groepsinstellingen zijn bijgewerkt, verwijzen naar de [Groep eigenschappen gecontroleerd](#update-group-attributes) in het gedeelte hierna
DeleteGroupSettings            | Verwijderde groepsinstellingen
SetGroupLicense                  | Groepslicentie instellen.
SetGroupManagedBy              | Groep worden beheerd door gebruiker instellen
AddGroupMember               | Lid is toegevoegd aan groep
RemoveGroupMember            | Lid verwijderen uit groep
AddGroupOwner                | Toegevoegde eigenaar aan groep
RemoveGroupOwner                 | De eigenaar van de verwijderd uit de groep
**Toepassingsgebeurtenissen**               |
Service principal toevoegen                | Een service principal toegevoegd aan de map.
Service principal verwijderen             | Een service principal verwijderd uit de adreslijst.
Service principal referenties toevoegen    | Toegevoegde referenties naar een service principal.
Service principal referenties verwijderen | Verwijderde referenties van een service principal.
Delegeren-fragment toevoegen                 | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) gemaakt in de map.
Set delegeren invoer                 | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) in de map bijgewerkt.
Delegeren vermelding verwijderen              | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) in de adreslijst verwijderd.
**Rol gebeurtenissen**                      |
Rollid toevoegen aan rol              | Een gebruiker toegevoegd aan een rol directory.
Rollid verwijderen uit de rol         | Een gebruiker verwijderd uit een rol directory.
Bedrijfsgegevens instellen      | Stel contactvoorkeuren niveau van het bedrijf. Dit geldt ook voor e-mailadressen voor marketing, evenals technische meldingen ontvangen over Microsoft Online Services.
Delegeren-fragment toevoegen                 | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) gemaakt in de map.
Set delegeren invoer                 | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) in de map bijgewerkt.
Delegeren vermelding verwijderen              | Een [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) in de adreslijst verwijderd.
AddSevicePrincipalOwner          | Toegevoegde eigenaar op de hoofdsom van de service.
RemoveSevicePrincipalOwner   | Eigenaar van de service principal verwijderd.
AddApplication  | Toepassing toevoegen.
UpdateApplication | Toepassing bijwerken. Als u wilt zien welke app-instellingen zijn bijgewerkt, raadpleegt u moeten [Gecontroleerd van toepassing-eigenschappen](#update-application-attributes) in het gedeelte hierna
DeleteApplication | Toepassing verwijderen.
RestoreApplication|Toepassing herstellen.
AddApplicationOwner   |     Eigenaar toevoegen aan toepassing.
RemoveApplicationOwner    |     Verwijder de eigenaar van toepassing.
**Rol gebeurtenissen**                         |
Rollid toevoegen aan rol                 | Een gebruiker toegevoegd aan een rol directory.
Rollid verwijderen uit de rol            | Een gebruiker verwijderd uit een rol directory.
AddRoleDefinition               | Toegevoegde roldefinitie.
UpdateRoleDefinition                | Bijgewerkte roldefinitie. Als u wilt zien welke rolinstellingen zijn bijgewerkt, raadpleegt u moeten [Rol definitie eigenschappen gecontroleerd](#update-role-definition-attributes) in het gedeelte hierna
DeleteRoleDefinition                | Verwijderde roldefinitie.
AddRoleAssignmentToRoleDefinition | Toegevoegde roltoewijzing aan de roldefinitie van de.
RemoveRoleAssignmentFromRoleDefinition | Verwijderde roltoewijzing van roldefinitie.
AddRoleFromTemplate     | Toegevoegde rol van sjabloon.
UpdateRole  | Bijgewerkte rol.
AddRoleScopeMemberToRole    | Bereik lid is toegevoegd aan rol.
RemoveRoleScopedMemberFromRole  | Bereik van lid verwijderd uit rol.
**Apparaatgebeurtenissen (alle nieuwe gebeurtenissen)**                    |
AddDevice               | Toegevoegde apparaat.
UpdateDevice            | Bijgewerkte apparaat. Als u wilt zien welke apparaateigenschappen zijn bijgewerkt, raadpleegt u [Apparaateigenschappen Audited](#update-device-attributes) in het gedeelte hierna
DeleteDevice            | Verwijderde apparaat.
AddDeviceConfiguration      | Apparaatconfiguratie van de toegevoegde.
UpdateDeviceConfiguration   | Apparaatconfiguratie van de bijgewerkte. Als u wilt zien welke apparaateigenschappen configuratie zijn bijgewerkt, raadpleegt u [configuratie apparaateigenschappen Audited](#update-device-configuration-attributes) in het gedeelte hierna
DeleteDeviceConfiguration   | Apparaatconfiguratie van de verwijderde.
AddRegisteredOwner     | Toegevoegde geregistreerde eigenaar naar apparaat.
AddRegisteredUsers     | Geregistreerde gebruikers toegevoegd aan apparaat.
RemoveRegisteredOwner    | Geregistreerde eigenaar van apparaat verwijderen.
RemoveRegisteredUsers    | Geregistreerde gebruikers verwijderen van apparaat.
RemoveDeviceCredentials  | Apparaat referenties verwijderd.
**B2B gebeurtenissen**                       |
Uitnodigingen voor batch geüpload.              | Een beheerder heeft een bestand met anderen uitnodigen om aan gebruikers van de partner die zijn geüpload.
Uitnodigingen voor batch verwerkt.             | Een bestand met uitnodigingen voor gebruikers van de partner is verwerkt.
Externe gebruikers uitnodigen.                | De map is een externe gebruiker uitgenodigd.
Externe gebruikers uitnodigen inwisselen.         | Een externe gebruiker heeft de uitnodiging voor de adreslijst hebt ingewisseld.
Externe gebruikers toevoegen aan groep.          | Een externe gebruiker is lidmaatschap toegewezen aan een groep in de adreslijst.
Externe gebruiker toewijzen aan de toepassing. | Een externe gebruiker is directe toegang toegewezen aan een toepassing.
Virus tenant maken.               | Een nieuwe tenant is gemaakt in Azure AD met de uitnodiging aflossingsprijs.
Virus gebruiker maken.                 | Een gebruiker is gemaakt in een bestaande tenant in Azure AD door de uitnodiging aflossingsprijs.
**Administratieve eenheden (alle nieuwe gebeurtenissen)**             |
AddAdministrativeUnit   |   Administratieve eenheid toevoegen.
UpdateAdministrativeUnit|   Administratieve eenheid bijwerken. Als u wilt zien welke eigenschappen voor administratieve eenheid zijn bijgewerkt, raadpleegt u moeten [administratieve eenheidseigenschappen gecontroleerd](#update-administrative-unit-attributes) in het gedeelte hierna
DeleteAdministrativeUnit|   Administratieve eenheid verwijderen.
AddMemberToAdministrativeUnit|  Lid toevoegen aan administratieve eenheid.
RemoveMemberFromAdministrativeUnit|     Lid verwijderen uit de administratieve eenheid.
**Directory gebeurtenissen**                 |
Partner bedrijf toevoegen               | Een partner toegevoegd aan de map.
Partner bedrijf verwijderen          | Een partner verwijderd uit de adreslijst.
DemotePartner   |   Partner verlagen.
Domein hebt toegevoegd aan bedrijf                | Een domein toegevoegd aan de map.
Domein verwijderen van bedrijf           | Een domein verwijderd uit de adreslijst.
Domein bijwerken                        | Een domein op de map bijgewerkt. Als u wilt zien welke eigenschappen voor het domein zijn bijgewerkt, raadpleegt u moeten [Domeineigenschappen gecontroleerd](#update-domain-attributes) in het gedeelte hierna
Domeinverificatie instellen            | De standaardinstelling voor domein voor het bedrijf gewijzigd.
Bedrijfsgegevens instellen      | Stel contactvoorkeuren niveau van het bedrijf. Dit geldt ook voor e-mailadressen voor marketing, evenals technische meldingen ontvangen over Microsoft Online Services.
Federatie instellingen instellen voor domein    | De federatie-instellingen voor een domein bijgewerkt.
Domein verifiëren                        | Een domein op de map gecontroleerd.
Controleer of gecontroleerde e-maildomein         | Een domein op de map met e-mailverificatie gecontroleerd.
DirSyncEnabled vlag op bedrijf instellen   | Stel de eigenschap waarmee een map voor Azure AD-synchronisatie.
Wachtwoordbeleid instellen                  | Stel de lengte en teken voorwaarden voor de wachtwoorden van gebruikers.
Bedrijfsgegevens instellen              | Het niveau van het bedrijf informatie worden bijgewerkt. Zie de PowerShell-cmdlet [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) voor meer informatie.
SetCompanyAllowedDataLocation  |    Set bedrijf toegestaan gegevenslocatie.
SetCompanyDirSyncEnabled    |   Stel DirSyncEnabled vlag.
SetCompanyDirSyncFeature |  Stel DirSync-functie.
SetCompanyInformation |   Bedrijfsgegevens van instellen.
SetCompanyMultiNationalEnabled    |     Stel bedrijf meertalige functie is ingeschakeld.
SetDirectoryFeatureOnTenant   |     Stel directory-functie op tenantniveau.
SetTenantLicenseProperties  |   Tenant licentie eigenschappen instellen.
CreateCompanySettings   |   Instellingen van het bedrijf maken
UpdateCompanySettings   |   Werk de instellingen van het bedrijf. Als u wilt zien welke eigenschappen van het bedrijf zijn bijgewerkt, verwijzen naar [de eigenschappen van het bedrijf gecontroleerd](#update-company-attributes) in het gedeelte hierna
DeleteCompanySettings   |   Instellingen van het bedrijf verwijderen
SetAccidentalDeletionThreshold   |  Stel de drempelwaarde voor per ongeluk wordt verwijderd.
SetRightsManagementProperties   |   Rights management-eigenschappen instellen.
PurgeRightsManagementProperties     |   Rights management eigenschappen verwijderen.
UpdateExternalSecrets   |   Externe geheimen bijwerken
**Voor Beleidsgebeurtenissen (alle nieuwe gebeurtenissen)**           |
AddPolicy   |   Beleid toevoegen.
UpdatePolicy    |   Beleid bijwerken.
DeletePolicy    |   Beleid verwijderen.
AddDefaultPolicyApplication     |   Beleid toevoegen aan toepassing.
AddDefaultPolicyServicePrincipal    |   Beleid toevoegen aan de service-principal.
RemoveDefaultPolicyApplication  |   Verwijder beleid van toepassing.
RemoveDefaultPolicyServicePrincipal     |   Beleid van service principal verwijderen.
RemovePolicyCredentials     |   Beleid referenties verwijderd.

## <a name="audit-report-retention"></a>Rapport bewaarbeleid controleren
Gebeurtenissen in het auditlograpport van Azure AD worden voor 180 dagen bewaard. Zie voor meer informatie over bewaarbeleid op rapporten en [Azure Active Directory rapport bewaarbeleid](active-directory-reporting-retention.md).

Voor klanten wilt opslaan van hun gebeurtenissen controleren langer bewaarbeleid, kan de API rapportage worden gebruikt om op te halen regelmatig gebeurtenissen controleren in een afzonderlijk gegevensopslag. Zie [Aan de slag met de API voor rapportage](active-directory-reporting-api-getting-started.md) voor meer informatie.

## <a name="properties-included-with-each-audit-event"></a>Eigenschappen die wordt geleverd bij elke controlegebeurtenis

Eigenschap      | Beschrijving
------------- | --------------------------------------------------------------
Datum en tijd | De datum en tijd waarop de controlegebeurtenis heeft plaatsgevonden
Acteur         | De gebruiker of service hoofdsom die de actie wordt uitgevoerd
Actie        | De actie die is uitgevoerd
Doel        | De gebruiker of de service-principal die de actie is uitgevoerd op


## <a name="update-user-attributes"></a>Kenmerken "Gebruiker bijwerken"
De gebeurtenis van de controle "Update gebruiker" bevat extra informatie over welke gebruikerskenmerken van de zijn bijgewerkt. Voor elk kenmerk, zijn zowel de vorige waarde als de nieuwe waarde is opgenomen.

Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | De gebruiker kan mij aanmelden.
AssignedLicense                     | Alle licenties die zijn toegewezen aan de gebruiker.
AssignedPlan                        | Service-abonnementen die voortvloeien uit de licenties die zijn toegewezen aan de gebruiker.
LicenseAssignmentDetail             | Als u meer informatie over de licenties die zijn toegewezen aan de gebruiker. Als op basis van een groep licensing was betrokken, zou dit bijvoorbeeld de groep die de licentie verleend bevatten.
Mobile                              | Van de gebruiker mobiele telefoon.
OtherMail                           | Alternatieve e-mailadres van de gebruiker.
OtherMobile                         | Alternatieve mobiele telefoon van de gebruiker.
StrongAuthenticationMethod          | Een lijst met verificatiemethoden geconfigureerd door de gebruiker voor meervoudige verificatie, zoals telefoongesprek, SMS of verificatie code vanuit een mobiele app.
StrongAuthenticationRequirement     | Meervoudige verificatie wordt afgedwongen, ingeschakeld of uitgeschakeld voor deze gebruiker.
StrongAuthenticationUserDetails     | Getal met een telefoon van de gebruiker, alternatief nummer getal en e-mailadres gebruikt voor verificatie voor meervoudige verificatie en het wachtwoord opnieuw instellen.
StrongAuthenticationPhoneAppDetail  | Details forof telefoon-apps geregistreerd om uit te voeren 2FA login
TelephoneNumber                     | Het telefoonnummer van de gebruiker.
AlternativeSecurityId               | Een alternatief beveiligings-ID voor het object.
CreationType                        |Methode voor het maken van de gebruiker (hetzij uitsluitend op uitnodiging of virussen).
InviteTicket                        |Lijst met tickets van de uitnodiging voor de gebruiker.
InviteReplyUrl                      |Lijst met URL's om te reageren op uitnodiging bevestigen.
InviteResources                     |Lijst met resources waaraan de gebruiker is uitgenodigd.
LastDirSyncTime                     |Laatst het object is bijgewerkt vanwege het synchroniseren van de gezaghebbende directory (klant, on-premises).
MSExchRemoteRecipientType           |Toegewezen aan de geadresseerden typen MSO. Verwijzen naar [MSO geadresseerden typen] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx voor geadresseerden typen
PreferredDataLocation               |De gewenste plaats voor de van de gebruiker, groep wordt bepaald, van contactpersoon, openbare map of van apparaat gegevens.
ProxyAddresses                      |Het adres waarmee een geadresseerde Exchange Server-object wordt herkend in een andere e-mailsysteem.
StsRefreshTokensValidFrom           |Uitvoert vernieuwen token intrekkingsgegevens - eventuele STS vernieuwen tokens uitgegeven voordat deze tijd worden beschouwd als verlopen.
UserPrincipalName                   |De UPN die is de aanmeldingsnaam van een Internet-stijl voor een gebruiker.
UserState                           |Status van de gebruiker PendingApproval/PendingAcceptance/geaccepteerd/PendingVerification.
UserStateChangedOn                  |Tijdstempel van de laatste wijziging van UserState. Wordt gebruikt voor het activeren van de levenscyclus van werkstromen.
UserType                            |Type gebruiker. Lid (0), Gast (1), Viral(2).

## <a name="update-group-attributes"></a>"Groep bijwerken" kenmerken
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Classificatie            | De indeling voor een groep geïntegreerde (HBI, MBI, enzovoort).
Beschrijving               | Leesbare beschrijvende woordgroepen over het object.  
Weergavenaam               |De weergavenaam voor een object
DirSyncEnabled            |Hiermee wordt aangegeven of de synchronisatie vindt plaats van een gezaghebbende (klant, on-premises) directory.
GroupLicenseAssignment    |De toewijzing licentie van een groep
GroupType                 |Type groep, Unified (0)
IsMembershipRuleLocked    |Geeft aan dat de eigenschap MembershipRule is ingesteld in de groep selfservice management-service kan niet worden gewijzigd door gebruikers. Alleen van toepassing op groepen waarbij GroupType GroupType.DynamicMembership bevat
IsPublic                  |De vlag om aan te geven als de groep openbaar/persoonlijk.
LastDirSyncTime           |Laatst het object is bijgewerkt als gevolg van het synchroniseren van de gezaghebbende directory (klant, on-premises).
E-mail                      |Primaire e-mailadres
MailEnabled               |Geeft aan of deze groep e-videomogelijkheden.
MailNickname              |Bijnaam voor een adres adresboek-object, meestal het gedeelte van de voorgaande e-naam de "@" symbool.   
MembershipRule            |Een tekenreeks die de criteria door de selfservice-groep management-service gebruikt om te bepalen welke leden moeten behoren tot deze groep uit te drukken. Zie ook IsMembershipRuleLocked. Alleen van toepassing op groepen waarbij GroupType GroupType.DynamicMembership bevat.
MembershipRuleProcessingState| Een vaste waarde gedefinieerd door de selfservice-groep management-service definiëren de status van lidmaatschap processing voor deze groep. Alleen van toepassing op groepen waarbij GroupType GroupType.DynamicMembership bevat.
ProxyAddresses            |Het adres waarmee een geadresseerde Exchange Server-object wordt herkend in een andere e-mailsysteem.
RenewedDateTime           |De record van de tijdstempel van wanneer u een groep het meest recent hebt verlengd.   
SecurityEnabled           |Geeft aan of lidmaatschap van de groep invloed kan zijn op machtigingen verlenen.
WellKnownObject           |Etiketten, een Active directory-object, erop te wijzen als een van de vooraf gedefinieerde.

## <a name="update-device-attributes"></a>Kenmerken "Apparaat bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Geeft aan of een beveiligings-principal kan verifiëren.
CloudAccountEnabled | Geeft aan of een beveiligings-principal kan verifiëren. Geschreven door InTune wanneer het apparaat on-premises is gemaakt.
CloudDeviceOSType | Type van het apparaat op basis van het besturingssysteem bijvoorbeeld Windows RT, iOS. Als de waarde door een cloudservice (zoals Intune), tijdserver dit kenmerk in de adreslijst voor DeviceOSType.
CloudDeviceOSVersion | Versie van het besturingssysteem. Als de waarde door een cloudservice (zoals Intune), tijdserver dit kenmerk in de adreslijst voor DeviceOSVersion.
CloudDisplayName | Waarde van de weergavenaam LDAP-kenmerk. Als de waarde door een cloudservice (zoals Intune), tijdserver dit kenmerk in de map voor de weergavenaam.
CloudCreated |Geeft aan of het object is gemaakt door cloudservices.
CompliantUntil | Apparaat wordt geacht compatibele tot welk tijdstip.
DeviceMetadata |Aangepaste metagegevens voor het apparaat
DeviceObjectVersion | Dit kenmerk wordt gebruikt om aan te geven van de versie van het apparaat.
DeviceOSType |Type van het apparaat op basis van het besturingssysteem bijvoorbeeld Windows RT, iOS. Geschreven door de registratie-Service en bedoeld om te worden bijgewerkt door de MDM licht management-service of STS management-service.
DeviceOSVersion |Versie van het besturingssysteem. Geschreven door de registratie-Service en bedoeld om te worden bijgewerkt door de MDM licht management-service of STS management-service.
DevicePhysicalIds |Kenmerk met meerdere waarden is bedoeld voor de opslag van identificaties van het apparaat. Dit kan onder andere bestaan BIOS-id's, TPM vingerafdrukken, hardware specifieke id's, enzovoort.
DirSyncEnabled | Hiermee wordt aangegeven of de synchronisatie vindt plaats van een gezaghebbende (klant, on-premises) directory.
Weergavenaam |De weergavenaam voor een object
IsCompliant | Dit kenmerk wordt gebruikt voor het beheren van het mobiele apparaat management status van het apparaat.
IsManaged |Dit kenmerk wordt gebruikt om aan te geven op dat het apparaat dat wordt beheerd door een wolk MDM.
LastDirSyncTime |Laatst het object is bijgewerkt vanwege het synchroniseren van de gezaghebbende directory (klant, on-premises).

## <a name="update-device-configuration-attributes"></a>Kenmerken "apparaatconfiguratie bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Het maximum aantal dagen dat een apparaat inactief voordat deze worden kan wordt beschouwd als verwijderd.
RegistrationQuota   |Het beleid dat wordt gebruikt om het aantal apparaat registraties toegestaan voor één gebruiker te beperken.

## <a name="update-service-principal-configuration-attributes"></a>Kenmerken "Service principal configuratie bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Geeft aan of een beveiligings-principal kan verifiëren.
AppPrincipalId | Externe, toepassingsspecifieke identiteit voor een beveiligings-principal.
Weergavenaam |De weergavenaam voor een object
ServicePrincipalName    | Service principal name, met ' naam/instantie' waar de naam van de waarde van een toepassing class en instantie bevat ten minste die zich hostname [: poort] of "naam" waarmee wordt opgegeven dat een id voor de service-principal.

## <a name="update-app-attributes"></a>Kenmerken "App bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |De set adressen (URL's omleiden) die zijn toegewezen aan een service principal.
Toepassings-id                               |Toepassings-ID van de App   
AppIdentifierUri | Toepassing URI, waarmee de toepassing.  Het is gewoonlijk de URL van de toepassing toegang.
AppLogoUrl |De url voor de logoafbeelding van de toepassing die zijn opgeslagen in een CDN.
AvailableToOtherTenants | Waar de toepassing is meerdere tenant (dat wil zeggen kunnen worden gebruikt door andere tenants).
Weergavenaam | De weergavenaam voor de naam van een toepassing
Recht |Lijst met toepassingen rechten.
ExternalUserAccountDelegationsAllowed |Vlag die aangeeft of toepassing van de resource een vertrouwde is en delegeren vermeldingen voor externe gebruikersaccounts kunt maken.
GroupMembershipClaims |Het lidmaatschap claims Groepsbeleid.
PublicClient | Waar als de client kan niet geheime behouden (dat wil zeggen niet-vertrouwelijke client in OAuth2.0)
RecordConsentConditions | Soorten toestemming voorwaarden voldoen, zoals gedefinieerd door de contractvoorwaarden: geen (0), SilentConsentForPartnerManagedApp(1). Deze waarde in het schema Graph API wordt geëvalueerd en kan alleen worden ingesteld/wijzigen door beheerders van de tenant.
RequiredResourceAccess |XML-inhoud van een waarde van de eigenschap RequiredResourceAccess.
WebApp |Als dit waar is, geeft aan dat deze toepassing een web-app.
WwwHomepage |De primaire pagina met webonderdelen.

## <a name="update-role-attributes"></a>Kenmerken "Rol bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |De set adressen (URL's omleiden) die zijn toegewezen aan een service principal.
BelongsToFirstLoginObjectSet |Als dit waar is, geeft aan dat dit object bij de set gegroepeerde objecten die zijn vereist hoort voor aanmelding van de eerste beheerder van een nieuwe tenant inschakelen.
Ingebouwde |Hiermee wordt aangegeven of de levensduur van een object eigendom is van het systeem.
Beschrijving | Leesbare beschrijvende woordgroepen over het object.  
Weergavenaam |De weergavenaam voor een object
MailNickname | Bijnaam voor een adres adresboek-object, meestal het gedeelte van de voorgaande e-naam de "@" symbool.
RoleDisabled |Hiermee wordt aangegeven of de rol voor toepassing van access controles moet worden genegeerd.
RoleTemplateId | De identiteit van de rolsjabloon.
ServiceInfo |Service-specifieke inrichten informatie die kan worden gebruikt door MOAC en/of andere service-exemplaren (van de servicetypen dezelfde of een andere).
TaskSetScopeReference |Geeft een TaskSet en een reeks die is gekoppeld aan een rol of RoleTemplate bereiken.
ValidationError |Gegevens die zijn gepubliceerd door een federatieve service met een beschrijving van een tijdelijke, service-specifieke fout met betrekking tot de eigenschappen of koppeling van een actie bij object beheerder om op te lossen.
WellKnownObject |Etiketten, een Active directory-object, erop te wijzen als een van de vooraf gedefinieerde.

## <a name="update-role-definition-attributes"></a>Kenmerken "roldefinitie bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |Verzameling autorisatie bereiken die kan worden verwezen wanneer deze RoleDefinition toewijzen aan een beveiligings-principal.
Weergavenaam     |De weergavenaam voor een object
GrantedPermissions  |Machtigingen verleend door deze RoleDefinition.


## <a name="update-administrative-unit-attributes"></a>Kenmerken "Administratieve eenheid bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Beschrijving |   Deze eigenschap wordt bijgewerkt wanneer u de beschrijving van een administratieve eenheid wijzigen.
Weergavenaam |   Deze eigenschap wordt bijgewerkt wanneer u de naam van een administratieve eenheid wijzigen.

## <a name="update-company-attributes"></a>"Bedrijf bijwerken" kenmerken
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Een locatie waar gebruikers van het bedrijf kunnen worden deze is ingericht.
AuthorizedServiceInstance   | Namen van de service-exemplaren waaraan een abonnement kan worden geïmplementeerd.
DirSyncEnabled |Hiermee wordt aangegeven of de synchronisatie vindt plaats van een gezaghebbende (klant, on-premises) directory.
DirSyncStatus |Hiermee wordt aangegeven of de synchronisatie adres adresboek objecten in deze context tenant uit een gezaghebbende (klant, on-premises plaatsvindt) directory; een uitbreiding van de eigenschap DirSyncEnabled van bedrijf objecten.
DirSyncFeatures |Bits vlag set ingeschakeld en uitgeschakeld dirsync-functies voor de tenant kunt bijhouden.
DirectoryFeatures |Ingeschakeld/uitgeschakeld directory-functies.
DirSyncConfiguration |Bevat alle DirSync-configuratie die specifiek zijn voor de huidige tenant.
Weergavenaam |De weergavenaam voor een object
IsMnc |Een Boole-vlag is ingesteld op "true" iff die het bedrijf is ingeschakeld voor de functie meertalige bedrijf.
ObjectSettings | Een verzameling instellingen die van toepassing op het bereik van het object.
PartnerCommerceUrl |URL van de Partner commerce-site.
PartnerHelpUrl |URL van de Partner help-site.
PartnerSupportEmail | De URL van de Partner ondersteuning e-mail.
PartnerSupportTelephone |De URL van de Partner ondersteuning telefoonnummer.
PartnerSupportUrl |URL van de Partner ondersteuningssite.
StrongAuthenticationDetails |Informatie over StrongAuthentication.
StrongAuthenticationPolicy |Sterke verificatiebeleid voor het bedrijf.
TechnicalNotificationMail |E-mailadres om te laten technische problemen die betrekking hebben op een bedrijf.
TelephoneNumber |De telefoonnummers die voldoen aan de ITU aanbeveling E.123.
TenantType |Het type van een tenant. Als deze waarde niet is opgegeven, is de tenant een bedrijf. Anders mogelijke waarden zijn: MicrosoftSupport (0), SyndicatePartner (1), BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |Een reeks DNS-domeinnamen die is gebonden aan een bedrijf.

## <a name="update-domain-attributes"></a>Kenmerken "Domein bijwerken"
Kenmerk                           | Beschrijving
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Mogelijkheden    |Bit vlaggen met een beschrijving van de mogelijkheden van het domein, indien van toepassing.
Standaard |Geeft aan of het domein de standaardwaarde is; Stel de standaard UserPrincipalName achtervoegsel wanneer een beheerder een nieuwe gebruiker in MOAC maakt.
Eerste |Geeft aan of het domein dat het oorspronkelijke domein voor het bedrijf, zoals toegewezen door OCP. Het oorspronkelijke domein is een unieke subdomein van een Microsoft Online-domein. e.g.contoso3.microsoftonline.com.
LiveType    |Type van de corresponderende Windows Live naamruimte, indien van toepassing.
Naam    | ID voor het eindpunt.
PasswordNotificationWindowDays  |Het aantal dagen voordat een wachtwoord de gebruiker verloopt ontvangt een melding.
PasswordValidityPeriodDays  | Het aantal dagen dat een wachtwoord is een goed idee voor voordat deze moet worden gewijzigd.

Controlerecords zijn een besturingselement voor een vereist voor veel regelgeving. Het wordt aanbevolen dat de klant een kopie van dit help-onderwerp met de kopie van de geëxporteerde controlerapport van de klant om uit te leggen details van het rapport verzenden voor klanten met behulp van het rapport in de controle Azure Active Directory om te voldoen aan hun regelgeving. Als de revisor wilt begrijpen van de regelgeving Azure momenteel voldoet aan, rechtstreekse de revisor naar de [pagina van de naleving](https://azure.microsoft.com/support/trust-center/compliance/) van de Microsoft Azure Trust Center.
