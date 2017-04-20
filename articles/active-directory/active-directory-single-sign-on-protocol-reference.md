<properties
    pageTitle="Azure eenmalige aanmelding SAML Protocol | Microsoft Azure"
    description="In dit artikel worden de eenmalige aanmelding op SAML-protocol in Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>

# <a name="single-sign-on-saml-protocol"></a>Eenmalige aanmelding SAML-protocol

In dit artikel heeft betrekking op het SAML 2.0 verificatie vergaderverzoeken en antwoorden die ondersteuning biedt voor eenmalige aanmelding voor Azure Active Directory (Azure AD).

De onderstaande protocol-diagram geeft de volgorde voor eenmalige aanmelding. De cloudservice (de serviceprovider) een HTTP-omleiding binding gebruikt om te geven een `AuthnRequest` (verificatieaanvraag) element dat moet worden Azure AD (de identiteitsprovider). Azure AD vervolgens gebruikt een HTTP-bericht binden posten een `Response` element dat moet worden de cloudservice.

![Eenmalige aanmelding werkstroom](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Als u een gebruikersverificatie, cloud services verzenden een `AuthnRequest` element dat moet worden Azure AD. Een steekproef SAML 2.0 `AuthnRequest` kan er als volgt uit:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| ID | Vereist | Azure AD dit kenmerk gebruikt om te vullen de `InResponseTo` kenmerk van het geretourneerde antwoord. ID moet niet beginnen met een getal, een gemeenschappelijke strategie wordt Voeg een tekenreeks als "-id" op de afbeelding tekenreeks van een GUID. Bijvoorbeeld `id6c1c178c166d486687be4aaf5e482730` is een geldige-ID. |
| Versie | Vereist | Dit moet **2.0**.|
| IssueInstant | Vereist | Dit is een DateTime-tekenreeks met een UTC-waarde en [heen indeling ('o'")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD verwacht een DateTime-waarde van dit type, maar niet beoordelen of gebruik de waarde. |
| AssertionConsumerServiceUrl | optionele | Als biedt, moet deze overeenkomen met de `RedirectUri` van de cloudservice in Azure AD. |
| ForceAuthn | optionele | Als u opgeeft, is dit moet ONWAAR. Een andere waarde treedt een fout.|
| IsPassive | optionele | Als u opgeeft, is dit moet ONWAAR. Een andere waarde treedt een fout. |  

Alle andere `AuthnRequest` kenmerken, zoals toestemming, bestemming, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex en ProviderName worden **genegeerd**.

Azure AD ook worden genegeerd de `Conditions` element in `AuthnRequest`.

### <a name="issuer"></a>Uitgever

De `Issuer` element in een `AuthnRequest` moet precies overeenkomen met een van de **ServicePrincipalNames** in de cloudservice in Azure AD. Dit is meestal ingesteld met de **App-ID-URI** die is opgegeven tijdens de registratie van toepassing.

Een voorbeeld SAML fragment met de `Issuer` element ziet er zo uit:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Met dit element aanvraagt van de indeling van een bepaalde naam-ID in het antwoord en is optioneel in `AuthnRequest` elementen naar Azure AD verzonden.

Een steekproef `NameIdPolicy` element ziet er zo uit:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Als `NameIDPolicy` wordt geleverd, kunt u ook de optionele `Format` kenmerk. De `Format` kenmerk kan hebben slechts één van de volgende waarden; een andere waarde levert een fout.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: De claim NameID problemen azure Active Directory als paarsgewijze identificatie.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory oplossen de claim NameID in de e-mailadres.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Met deze waarde toestemming geeft dat Azure Active Directory om de claim-indeling te selecteren. Azure Active Directory oplossen de NameID als paarsgewijze identificatie.

Neem niet de `SPNameQualifer` kenmerk. Azure AD worden genegeerd de `AllowCreate` kenmerk.

### <a name="requestauthncontext"></a>RequestAuthnContext

De `RequestedAuthnContext` element Hiermee geeft u de gewenste verificatiemethoden. Dit is optioneel in `AuthnRequest` elementen naar Azure AD verzonden. Azure AD ondersteunt slechts één `AuthnContextClassRef` waarde: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Een bereik instellen

De `Scoping` element, een lijst met een identiteitsprovider bevat, is optioneel in `AuthnRequest` elementen naar Azure AD verzonden.

Als biedt, neem niet de `ProxyCount` kenmerk, `IDPListOption` of `RequesterID` element, zoals ze worden niet ondersteund.

### <a name="signature"></a>Handtekening

Neem niet op een `Signature` element in `AuthnRequest` elementen, zoals Azure AD biedt geen ondersteuning voor ondertekend verificatieaanvragen.

### <a name="subject"></a>Onderwerp

Azure AD worden genegeerd de `Subject` element van `AuthnRequest` elementen.

## <a name="response"></a>Antwoord

Wanneer een gevraagde aanmelding voltooid is, berichten Azure AD een antwoord op de cloudservice. Een voorbeeld-antwoord op een geslaagde poging aanmelding ziet er zo uit:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Antwoord

De `Response` element het resultaat van de autorisatieaanvraag bevat. Azure AD-sets de `ID`, `Version` en `IssueInstant` -waarden in de `Response` element. Dit wordt ook de volgende kenmerken:

- `Destination`: Bij aanmelding voltooid is, dit is ingesteld op de `RedirectUri` van de serviceprovider (cloudservice).
- `InResponseTo`: Dit is ingesteld op de `ID` kenmerk van de `AuthnRequest` element die het antwoord wordt gestart.

### <a name="issuer"></a>Uitgever

Azure AD-sets de `Issuer` element dat moet worden `https://login.microsoftonline.com/<TenantIDGUID>/` waar <TenantIDGUID> is de ID van de tenant van de Azure AD-tenant.

Een antwoord van de steekproef met uitgever element kan bijvoorbeeld er zo uit:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Status

De `Status` element het slagen of mislukken van eenmalige aanmelding worden weergegeven. Het bevat de `StatusCode` element, een code of een reeks geneste codes die u kunt de status van de aanvraag bevat. Bevat ook de `StatusMessage` element, aangepaste foutberichten die worden gegenereerd tijdens het proces aanmelding bevat.

<!-- TODO: Add a authentication protocol error reference -->

Hierna volgt een SAML-antwoord op een mislukte poging aanmelding.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Bevestiging

Naast de `ID`, `IssueInstant` en `Version`, Azure AD Hiermee stelt u de volgende elementen in de `Assertion` element van het antwoord.

#### <a name="issuer"></a>Uitgever

Dit is ingesteld op `https://sts.windows.net/<TenantIDGUID>/`waar <TenantIDGUID> is de ID van de Tenant van de Azure AD-tenant.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Handtekening

Azure AD ondertekent het bevestiging in antwoord op een succesvolle aanmelding. De `Signature` element bevat een digitale handtekening die de cloudservice gebruiken kunt om te verifiëren van de bron om te controleren of de integriteit van de bevestiging.

Als u wilt deze digitale handtekening genereren, Azure AD gebruikmaakt van de bijbehorende sleutel in de `IDPSSODescriptor` element van de metagegevensdocument.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Onderwerp

Hiermee geeft u de hoofdsom die is het onderwerp van de instructies in de bevestiging. Bevat een `NameID` element, waarmee de geverifieerde gebruiker. De `NameID` waarde is een gerichte id die is gericht alleen aan de serviceprovider die de doelgroep voor het token is. Het is permanente: deze kan worden ingetrokken, maar nooit opnieuw is toegewezen. Het is ook ondoorzichtig, omdat het niet alles over de gebruiker te geven en kan niet worden gebruikt als id voor kenmerk query's.

De `Method` kenmerk van de `SubjectConfirmation` element altijd is ingesteld op `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Voorwaarden

Met dit element opgeven voorwaarden waaraan de gebruiksregels van SAML bevestigingen definiëren.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

De `NotBefore` en `NotOnOrAfter` kenmerken Geef het interval waarin de bevestiging geldig is.

- De waarde van de `NotBefore` kenmerk is gelijk aan of iets (minder dan een tweede) later dan de waarde van `IssueInstant` kenmerk van de `Assertion` element. Azure AD wordt geen rekening gehouden eventuele tijdverschil tussen zichzelf en door de cloudservice (serviceprovider) en een buffer wordt niet worden toegevoegd aan dit moment.
- De waarde van de `NotOnOrAfter` kenmerk is 70 minuten later dan de waarde van de `NotBefore` kenmerk.

#### <a name="audience"></a>Publiek

Dit document bevat een URI die aangeeft van een bepaalde doelgroep. Azure AD Hiermee stelt u de waarde van dit element op de waarde van `Issuer` element van de `AuthnRequest` die de aanmelding hebt gestart. Voor de evaluatie van de `Audience` waarde, gebruik de waarde van de `App ID URI` die is opgegeven tijdens de registratie van toepassing.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Als de `Issuer` waarde, de `Audience` waarde moet precies overeenkomen met een van de belangrijkste servicenamen dat de cloudservice in Azure AD voorstelt. Echter als de waarde van de `Issuer` element een URI waarde is, de `Audience` waarde in het antwoord is de `Issuer` waarde voorafgegaan door `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Bevat deze optie claims over het onderwerp of de gebruiker. Het volgende fragment bevat een steekproef `AttributeStatement` element. Het beletselteken wordt aangegeven dat het element meerdere kenmerken en kenmerkwaarden bevatten kan.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **De naam van Claim** : de waarde van de `Name` kenmerk (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) is de UPN van de geverifieerde gebruiker, zoals `testuser@managedtenant.com`.
- **ObjectIdentifier claimen** : de waarde van de `ObjectIdentifier` kenmerk (`http://schemas.microsoft.com/identity/claims/objectidentifier`) is de `ObjectId` van het directoryobject dat de geverifieerde gebruiker in Azure AD voorstelt. `ObjectId`een onveranderlijke uniek en opnieuw gebruiken veilige id van de geverifieerde gebruiker.

#### <a name="authnstatement"></a>AuthnStatement

Met dit element beschouwt dat het onderwerp bevestiging is geverifieerd door een bepaalde manier op een bepaald moment.

- De `AuthnInstant` kenmerk Hiermee geeft u de tijd waarop de gebruiker worden geverifieerd met Azure AD.
- De `AuthnContext` element geeft de verificatie-context gebruikt om de gebruiker te verifiëren.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
