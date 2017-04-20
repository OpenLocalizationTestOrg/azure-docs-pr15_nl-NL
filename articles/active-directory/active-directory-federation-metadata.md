<properties
    pageTitle="Azure AD Federatie metagegevens | Microsoft Azure"
    description="In dit artikel worden de Federatie metagegevensdocument waarmee Azure Active Directory worden gepubliceerd voor services die Azure Active Directory-tokens accepteren."
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


# <a name="federation-metadata"></a>Federatie metagegevens

Azure Active Directory (Azure AD) geeft een document Federatie metagegevens voor services die geconfigureerd voor het accepteren van beveiligingstokens Azure AD problemen. De documentindeling Federatie metagegevens wordt beschreven in de [Web Services Federatie Language (WS-Federatie) versie 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), die een uitbreiding van [metagegevens voor de v2.0 OASIS beveiliging bevestiging Markup Language (SAML)](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Tenant / regiospecifieke en Tenant ongeacht metagegevens eindpunten

Azure AD publiceert-tenant / regiospecifieke en tenant ongeacht eindpunten.

Tenant / regiospecifieke eindpunten zijn ontworpen voor een bepaalde tenant. De metagegevens van de tenant / regiospecifieke Federatie bevat informatie over de tenant, inclusief tenant / regiospecifieke uitgever- en -eindpunt. Toepassingen die toegang tot een enkel tenant beperken gebruik tenant / regiospecifieke eindpunten.

Tenant-onafhankelijke eindpunten bevatten informatie die geldt voor alle Azure AD-tenants. Deze informatie is van toepassing op tenants gehost bij *login.microsoftonline.com* en worden verdeeld over tenants. Tenant-onafhankelijke eindpunten worden aanbevolen voor meerdere tenant-toepassingen, omdat ze niet gekoppeld aan een bepaalde tenant.

## <a name="federation-metadata-endpoints"></a>Federatie metagegevens eindpunten

Azure AD publiceert Federatie metagegevens op `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Voor de **tenant-specifieke eindpunten**, de `TenantDomainName` kan een van de volgende typen zijn:

- Een geregistreerde domeinnaam van een Azure AD-tenant, zoals: `contoso.onmicrosoft.com`.
- De onveranderlijke-ID van het domein, zoals tenant `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Voor de **tenant-onafhankelijke eindpunten**, de `TenantDomainName` is `common`. In dit document bevat alleen de Federatie metagegevens-elementen die gelden voor alle Azure AD-tenants die worden gehost op login.microsoftonline.com.

Bijvoorbeeld, een tenant-specifieke-eindpunt mogelijk `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Het eindpunt van de tenant-onafhankelijke is [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). U kunt het Federatie metagegevensdocument weergeven door deze URL in een browser te typen.

## <a name="contents-of-federation-metadata"></a>Inhoud van Federatie metagegevens

Het volgende gedeelte vindt u informatie nodig zijn voor services die de tokens uitgegeven door Azure AD gebruiken.

### <a name="entity-id"></a>Entiteit-ID

De `EntityDescriptor` element bevat een `EntityID` kenmerk. De waarde van de `EntityID` kenmerk de uitgever, dat wil zeggen de beveiliging token-service (STS) dat het token uitgegeven vertegenwoordigt. Het is belangrijk is voor het valideren van de uitgever wanneer u een token ontvangt.

De volgende metagegevens ziet u een steekproef tenant / regiospecifieke `EntityDescriptor` element met een `EntityID` element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
U kunt de tenant-ID in het eindpunt van de tenant-onafhankelijke vervangen door uw tenant-ID te maken van een tenant-specifieke `EntityID` waarde. De resulterende waarde is hetzelfde als de token uitgever. De strategie kan een toepassing meerdere tenant voor het valideren van de uitgever voor een bepaald tenant.

De volgende metagegevens ziet u een steekproef tenant ongeacht `EntityID` element. Houd er rekening mee dat het `{tenant}` is een letterlijke waarde, niet een tijdelijke aanduiding.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Certificaten voor token-ondertekend

Wanneer een service een token dat is uitgegeven door een Azure AD-tenant ontvangt, moet de handtekening van het token worden gevalideerd met een ondertekend sleutel die in het document van de metagegevens Federatie is gepubliceerd. De metagegevens Federatie bevat het openbare gedeelte van de certificaten die de tenants voor token-ondertekening gebruiken. Het certificaat onbewerkte bytes worden weergegeven in de `KeyDescriptor` element. Het handtekeningcertificaat token geldt voor het ondertekenen van alleen wanneer de waarde van de `use` kenmerk is `signing`.

Federatie metagegevens uitgegeven door Azure AD kunt u er meerdere ondertekend toetsen, zoals wanneer Azure AD bereidt bijwerken van het handtekeningcertificaat. Wanneer u een Federatie metagegevensdocument bevat meer dan één certificaat, moet een service die is de tokens valideren ondersteuning voor alle certificaten in het document.

Een voorbeeld ziet u de volgende metagegevens `KeyDescriptor` element met een ondertekend sleutel.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

De `KeyDescriptor` element wordt weergegeven op twee plaatsen in het document van Federatie metagegevens. in de sectie WS-Federatie-specifieke en de sectie SAML / regiospecifieke. De certificaten die zijn gepubliceerd in beide secties worden niet hetzelfde zijn.

Klik in de sectie WS-Federatie-specifieke leest een WS-Federation metagegevens lezer de certificaten van een `RoleDescriptor` element met de `SecurityTokenServiceType` type.

Een voorbeeld ziet u de volgende metagegevens `RoleDescriptor` element.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

In de sectie SAML / regiospecifieke leest een WS-Federation metagegevens lezer de certificaten van een `IDPSSODescriptor` element.

Een voorbeeld ziet u de volgende metagegevens `IDPSSODescriptor` element.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Er zijn geen verschillen in de notatie van tenant / regiospecifieke en tenant ongeacht certificaten.

### <a name="ws-federation-endpoint-url"></a>WS-Federation eindpunt URL

De metagegevens Federatie bevat de URL die Azure AD worden gebruikt voor eenmalige aanmelding en één afmelden in WS-Federation-protocol. Dit eindpunt wordt weergegeven in de `PassiveRequestorEndpoint` element.

Een voorbeeld ziet u de volgende metagegevens `PassiveRequestorEndpoint` element voor een tenant-specifieke-eindpunt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Voor het eindpunt van de tenant-onafhankelijke, de WS-Federation-URL wordt weergegeven in het eindpunt WS-Federatie, zoals wordt weergegeven in het onderstaande voorbeeld.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>Op SAML protocol eindpunt URL

De metagegevens Federatie bevat de URL die Azure AD voor eenmalige aanmelding en één afmelden in het vak SAML 2.0-protocol wordt gebruikt. Deze eindpunten worden weergegeven de `IDPSSODescriptor` element.

Het aanmelden en afmelden URL's worden weergegeven in de `SingleSignOnService` en `SingleLogoutService` elementen.

Een voorbeeld ziet u de volgende metagegevens `PassiveResistorEndpoint` voor een tenant-specifieke-eindpunt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

De eindpunten voor de algemene SAML 2.0-protocoleindpunten worden op dezelfde manier zijn gepubliceerd in de tenant-onafhankelijke Federatie metagegevens, zoals wordt weergegeven in het onderstaande voorbeeld.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
