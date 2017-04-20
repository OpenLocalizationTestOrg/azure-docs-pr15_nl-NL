<properties
    pageTitle="Synchronisatie van Azure AD Connect: kenmerken worden gesynchroniseerd met Azure Active Directory | Microsoft Azure"
    description="Zijn de kenmerken die zijn gesynchroniseerd met Azure Active Directory."
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
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Synchronisatie van Azure AD Connect: kenmerken worden gesynchroniseerd met Azure Active Directory
In dit onderwerp ziet u de kenmerken die Azure AD Connect-synchronisatie worden gesynchroniseerd.  
De kenmerken die zijn gegroepeerd op de gerelateerde Azure AD-app.

## <a name="attributes-to-synchronize"></a>Kenmerken om te synchroniseren
Een vraag is *Wat is de lijst met minimale kenmerken om te synchroniseren*. De standaard- en aanbevolen benadering is de standaardattributen houden zodat een volledige GAL (algemene adreslijst) kan worden gemaakt in de cloud en alle functies in Office 365-werkbelastingen. In sommige gevallen zijn er enkele kenmerken die uw organisatie wil niet dat in de cloud gesynchroniseerde omdat deze kenmerken bevatten gevoelige of PII (persoonlijke gegevens) gegevens, zoals in dit voorbeeld:  
![ongeldige kenmerken](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

In dit geval beginnen met de lijst met kenmerken in dit onderwerp en de kenmerken die gevoelige of PII gegevens bevatten zou en kunnen niet worden gesynchroniseerd identificeren. Deselecteer die kenmerken tijdens de installatie met [Azure AD-app en kenmerk filteren](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

>[AZURE.WARNING] Wanneer als u de kenmerken uitschakelt, moet u voorzichtig zijn en schakel alleen uit die kenmerken absoluut niet mogelijk om te synchroniseren. Andere kenmerken in tegenstelling, mogelijk een negatieve invloed hebben op functies.

## <a name="office-365-proplus"></a>Office 365 ProPlus

| Kenmerknaam| Gebruiker| Opmerking |
| --- | :-: | --- |
| accountEnabled| X| Hiermee definieert u als een account is ingeschakeld.|
| CN| X|  |
| Weergavenaam| X|  |
| objectSID| X| mechanisch eigenschap. AD gebruikers-id gebruikt voor het behoud van synchronisatie tussen Azure AD en AD.|
| pwdLastSet| X| mechanisch eigenschap. Wordt gebruikt om te weten wanneer om ongeldig al gepubliceerde tokens te maken. Gebruikt door Wachtwoordsynchronisatie en Federatie.|
| sourceAnchor| X| mechanisch eigenschap. Onveranderlijke id waarmee u het behoud van de relatie tussen Hiermee en Azure AD.|
| usageLocation| X| mechanisch eigenschap. Land van de gebruiker. Wordt gebruikt voor toewijzing aan een licentie.|
| userPrincipalName| X| UPN is de aanmeldings-ID voor de gebruiker. Meestal hetzelfde als [mail] resultaat.|

## <a name="exchange-online"></a>Exchange Online

| Kenmerknaam| Gebruiker| Contactpersoon| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Hiermee definieert u als een account is ingeschakeld.|
| assistent| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| CO| X| X|  |  |
| bedrijf| X| X|  |  |
| countryCode| X| X|  |  |
| afdeling| X| X|  |  |
| Beschrijving| X| X| X|  |
| Weergavenaam| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| TelefoonPrivé| X| X|  |  |
| Info| X| X| X| Dit kenmerk is momenteel niet worden gebruikt voor groepen.|
| Initialen| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| Manager| X| X|  |  |
| lid|  |  | X|  |
| mobiele| X| X|  |  |
| msDS-HABSeniorityIndex| X| X| X|  |
| msDS-PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute2| X| X| X| Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute3| X| X| X| Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute4| X| X| X| Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute5| X| X| X| Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg-IsOrganizational|  |  | X|  |
| objectSID| X|  | X| mechanisch eigenschap. AD gebruikers-id gebruikt voor het behoud van synchronisatie tussen Azure AD en AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| pager hebben| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postcode| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | mechanisch eigenschap. Wordt gebruikt om te weten wanneer om ongeldig al gepubliceerde tokens te maken. Gebruikt door Wachtwoordsynchronisatie en Federatie.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Afgeleid van groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanisch eigenschap. Onveranderlijke id waarmee u het behoud van de relatie tussen Hiermee en Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| titel| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | mechanisch eigenschap. Land van de gebruiker. Wordt gebruikt voor toewijzing aan een licentie.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. Meestal hetzelfde als [mail] resultaat.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint Online

| Kenmerknaam| Gebruiker| Contactpersoon| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Hiermee definieert u als een account is ingeschakeld.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| CO| X| X|  |  |
| bedrijf| X| X|  |  |
| countryCode| X| X|  |  |
| afdeling| X| X|  |  |
| Beschrijving| X| X| X|  |
| Weergavenaam| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| hideDLMembership|  |  | X|  |
| TelefoonPrivé| X| X|  |  |
| Info| X| X| X|  |
| initialen| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| e-mail| X| X| X|  |
| mailnickname| X| X| X|  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| lid|  |  | X|  |
| Afzonderlijk| X| X|  |  |
| mobiele| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| mechanisch eigenschap. AD gebruikers-id gebruikt voor het behoud van synchronisatie tussen Azure AD en AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| pager hebben| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postcode| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanisch eigenschap. Wordt gebruikt om te weten wanneer om ongeldig al gepubliceerde tokens te maken. Gebruikt door Wachtwoordsynchronisatie en Federatie.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Afgeleid van groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanisch eigenschap. Onveranderlijke id waarmee u het behoud van de relatie tussen Hiermee en Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| titel| X| X|  |  |
| unauthOrig| X| X| X|  |
| URL| X| X|  |  |
| usageLocation| X|  |  | mechanisch eigenschap. Land van de gebruiker. Wordt gebruikt voor toewijzing aan een licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. Meestal hetzelfde als [mail] resultaat.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync Online

| Kenmerknaam| Gebruiker| Contactpersoon| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Hiermee definieert u als een account is ingeschakeld.|
| c| X| X|  |  |
| CN| X|  | X|  |
| CO| X| X|  |  |
| bedrijf| X| X|  |  |
| afdeling| X| X|  |  |
| Beschrijving| X| X| X|  |
| Weergavenaam| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| givenName| X| X|  |  |
| TelefoonPrivé| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| e-mail| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| lid|  |  | X|  |
| mobiele| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| msRTCSIP-ApplicationOptions| X|  |  |  |
| msRTCSIP-DeploymentLocator| X| X|  |  |
| msRTCSIP-lijn| X| X|  |  |
| msRTCSIP-OptionFlags| X| X|  |  |
| msRTCSIP-OwnerUrn| X|  |  |  |
| msRTCSIP-PrimaryUserAddress| X| X|  |  |
| msRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| mechanisch eigenschap. AD gebruikers-id gebruikt voor het behoud van synchronisatie tussen Azure AD en AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postcode| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanisch eigenschap. Wordt gebruikt om te weten wanneer om ongeldig al gepubliceerde tokens te maken. Gebruikt door Wachtwoordsynchronisatie en Federatie.|
| securityEnabled|  |  | X| Afgeleid van groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanisch eigenschap. Onveranderlijke id waarmee u het behoud van de relatie tussen Hiermee en Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| titel| X| X|  |  |
| usageLocation| X|  |  | mechanisch eigenschap. Land van de gebruiker. Wordt gebruikt voor toewijzing aan een licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. Meestal hetzelfde als [mail] resultaat.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| Kenmerknaam| Gebruiker| Contactpersoon| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Hiermee definieert u als een account is ingeschakeld.|
| CN| X|  | X| Algemene naam of alias. Meestal het voorvoegsel van [e-mail] waarde.|
| Weergavenaam| X| X| X| Een tekenreeks met de naam die vaak worden weergegeven als de beschrijvende naam (voornaam achternaam).|
| e-mail| X| X| X| volledige e-mailadres.|
| lid|  |  | X|  |
| objectSID| X|  | X| mechanisch eigenschap. AD gebruikers-id gebruikt voor het behoud van synchronisatie tussen Azure AD en AD.|
| proxyAddresses| X| X| X| mechanisch eigenschap. Gebruikt door Azure AD. Bevat alle secundaire e-mailadressen voor de gebruiker.|
| pwdLastSet| X|  |  | mechanisch eigenschap. Wordt gebruikt om te weten wanneer om ongeldig al gepubliceerde tokens te maken.|
| securityEnabled|  |  | X| Afgeleid van groupType.|
| sourceAnchor| X| X| X| mechanisch eigenschap. Onveranderlijke id waarmee u het behoud van de relatie tussen Hiermee en Azure AD.|
| usageLocation| X|  |  | mechanisch eigenschap. Land van de gebruiker. Wordt gebruikt voor toewijzing aan een licentie.|
| userPrincipalName| X|  |  | Deze UPN is de aanmeldings-ID voor de gebruiker. Meestal hetzelfde als [mail] resultaat.|

## <a name="intune"></a>Intune

| Kenmerknaam| Gebruiker| Contactpersoon| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Hiermee definieert u als een account is ingeschakeld.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Beschrijving| X| X| X|  |
| Weergavenaam| X| X| X|  |
| e-mail| X| X| X|  |
| mailnickname| X| X| X|  |
| lid|  |  | X|  |
| objectSID| X|  | X| mechanisch eigenschap. AD gebruikers-id gebruikt voor het behoud van synchronisatie tussen Azure AD en AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanisch eigenschap. Wordt gebruikt om te weten wanneer om ongeldig al gepubliceerde tokens te maken. Gebruikt door Wachtwoordsynchronisatie en Federatie.|
| securityEnabled|  |  | X| Afgeleid van groupType|
| sourceAnchor| X| X| X| mechanisch eigenschap. Onveranderlijke id waarmee u het behoud van de relatie tussen Hiermee en Azure AD.|
| usageLocation| X|  |  | mechanisch eigenschap. Land van de gebruiker. Wordt gebruikt voor toewijzing aan een licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. Meestal hetzelfde als [mail] resultaat.|

## <a name="dynamics-crm"></a>Dynamics CRM

| Kenmerknaam| Gebruiker| Contactpersoon| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Hiermee definieert u als een account is ingeschakeld.|
| c| X| X|  |  |
| CN| X|  | X|  |
| CO| X| X|  |  |
| bedrijf| X| X|  |  |
| countryCode| X| X|  |  |
| Beschrijving| X| X| X|  |
| Weergavenaam| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| lid|  |  | X|  |
| mobiele| X| X|  |  |
| objectSID| X|  | X| mechanisch eigenschap. AD gebruikers-id gebruikt voor het behoud van synchronisatie tussen Azure AD en AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| Postcode| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | mechanisch eigenschap. Wordt gebruikt om te weten wanneer om ongeldig al gepubliceerde tokens te maken. Gebruikt door Wachtwoordsynchronisatie en Federatie.|
| securityEnabled|  |  | X| Afgeleid van groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanisch eigenschap. Onveranderlijke id waarmee u het behoud van de relatie tussen Hiermee en Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| titel| X| X|  |  |
| usageLocation| X|  |  | mechanisch eigenschap. Land van de gebruiker. Wordt gebruikt voor toewijzing aan een licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. Meestal hetzelfde als [mail] resultaat.|

## <a name="3rd-party-applications"></a>3e toepassingen van derden
Deze groep is een verzameling kenmerken die worden gebruikt als de minimale kenmerken die u nodig hebt voor een algemene werkbelasting of toepassing. Dit kan worden gebruikt voor een werkbelasting niet wordt vermeld in een andere sectie of voor een niet-Microsoft-app. Expliciet gebruiken voor het volgende:

- Yammer (alleen gebruiker wordt verbruikt)
- [Hybride Business-to-Business (B2B) cross-organisatie samenwerking scenario's aangeboden door alle resources zoals SharePoint](http://go.microsoft.com/fwlink/?LinkId=747036)

Deze groep is een verzameling kenmerken die kunnen worden gebruikt als de map Azure AD niet wordt gebruikt voor de ondersteuning van Office 365, Dynamics of Intune. Er wordt een kleine set core kenmerken.

| Kenmerknaam| Gebruiker| Contactpersoon| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Hiermee definieert u als een account is ingeschakeld.|
| CN| X|  | X|  |
| Weergavenaam| X| X| X|  |
| givenName| X| X|  |  |
| e-mail| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| lid|  |  | X|  |
| objectSID| X|  |  | mechanisch eigenschap. AD gebruikers-id gebruikt voor het behoud van synchronisatie tussen Azure AD en AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanisch eigenschap. Wordt gebruikt om te weten wanneer om ongeldig al gepubliceerde tokens te maken. Gebruikt door Wachtwoordsynchronisatie en Federatie.|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanisch eigenschap. Onveranderlijke id waarmee u het behoud van de relatie tussen Hiermee en Azure AD.|
| usageLocation| X|  |  | mechanisch eigenschap. Land van de gebruiker. Wordt gebruikt voor toewijzing aan een licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. Meestal hetzelfde als [mail] resultaat.|

## <a name="windows-10"></a>Windows 10
Een domein behoren computer(device) voor Windows 10 worden sommige kenmerken naar Azure AD gesynchroniseerd. Zie [verbinding maken met een domein behoren apparaten om over te Azure AD voor Windows 10-ervaringen](active-directory-azureadjoin-devices-group-policy.md)voor meer informatie over de scenario's. Deze kenmerken altijd te synchroniseren en Windows 10 niet wordt weergegeven als een app die kunt u bijvoorbeeld deselecteren. Een domein behoren computer voor Windows 10 wordt aangeduid met het kenmerk Eigenschapsspecifiek gevuld met.

| Kenmerknaam| Apparaat| Opmerking |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Vastgelegde waarde voor een domein behoren computers. |
| Weergavenaam | X| |
| MS-DS-CreatorSID | X| Een afkorting registeredOwnerReference.|
| objectGUID | X| Een afkorting deviceID.|
| objectSID | X| Een afkorting onPremisesSecurityIdentifier.|
| Besturingssysteem | X| Een afkorting deviceOSType.|
| operatingSystemVersion | X| Een afkorting deviceOSVersion.|
| userCertificate | X| |

Deze kenmerken voor **gebruiker** zijn naast de andere apps die u hebt geselecteerd.  

| Kenmerknaam| Gebruiker| Opmerking |
| --- | :-: | --- |
| domainFQDN| X| Een afkorting DNS-domeinnaam. Bijvoorbeeld contoso.com.|
| domainNetBios| X| Een afkorting NetBIOS-naam. Bijvoorbeeld CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Exchange hybride write-backs
Deze kenmerken worden geschreven terug van Azure AD met lokale Active Directory wanneer u deze optie selecteren om **hybride implementatie van Exchange**. Afhankelijk van uw Exchange-versie, mogelijk minder kenmerken worden gesynchroniseerd.

| Kenmerknaam| Gebruiker| Contactpersoon| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| msDS-ExternalDirectoryObjectID| X|  |  | Afgeleid van cloudAnchor in Azure AD. Dit kenmerk is nieuw in Exchange 2016.|
| msExchArchiveStatus| X|  |  | Onlinearchief: Kunnen klanten archiveren van e-mail.|
| msExchBlockedSendersHash| X|  |  | Filteren: Gegevens terug worden geschreven on-premises filteren en online veilige en geblokkeerde afzender gegevens van clients.|
| msExchSafeRecipientsHash| X|  |  | Filteren: Gegevens terug worden geschreven on-premises filteren en online veilige en geblokkeerde afzender gegevens van clients.|
| msExchSafeSendersHash| X|  |  | Filteren: Gegevens terug worden geschreven on-premises filteren en online veilige en geblokkeerde afzender gegevens van clients.|
| msExchUCVoiceMailSettings| X|  |  | Unified Messaging (UM) - Online voicemail inschakelen: gebruikt door Microsoft Lync Server integratie om aan te geven met Lync Server on-premises implementatie dat de gebruiker voicemail in de online services heeft.|
| msExchUserHoldPolicies| X|  |  | Juridische bewaring: Hiermee cloudservices om te bepalen welke gebruikers zijn onder procesvoering houdt.|
| proxyAddresses| X| X| X| Alleen de x500 adres vanaf Exchange Online wordt ingevoegd.|

## <a name="device-writeback"></a>Apparaat write-backs
Apparaatobjecten zijn gemaakt in Active Directory. Deze objecten kunnen worden verbonden met Azure AD apparaten of computers met Windows 10 domein behoren.

| Kenmerknaam| Apparaat| Opmerking |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| Weergavenaam | X| |
| DN-naam | X| |
| msDS-CloudAnchor | X| |
| msDS-DeviceID | X| |
| msDS-DeviceObjectVersion | X| |
| msDS-DeviceOSType | X| |
| msDS-DeviceOSVersion | X| |
| msDS-DevicePhysicalIDs | X| |
| msDS-KeyCredentialLink | X| Alleen voor gebruik met Windows Server 2016 AD schema |
| msDS-IsCompliant | X| |
| msDS-IsEnabled | X| |
| msDS-IsManaged | X| |
| msDS-RegisteredOwner | X| |


## <a name="notes"></a>Notities

- Als u een alternatieve ID gebruikt, wordt de on-premises implementatie kenmerk userPrincipalName wordt gesynchroniseerd met de onPremisesUserPrincipalName Azure AD-kenmerk. Het kenmerk alternatieve ID voor voorbeeld e-mail wordt gesynchroniseerd met de Azure AD-kenmerk userPrincipalName.
- In de lijsten hierboven, is het objecttype **gebruiker** ook van toepassing op het object type **iNetOrgPerson**.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de configuratie [Azure AD Connect synchroniseren](active-directory-aadconnectsync-whatis.md) .

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
