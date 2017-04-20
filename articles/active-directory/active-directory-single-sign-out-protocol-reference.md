<properties
    pageTitle="Azure één afmelden SAML Protocol | Microsoft Azure"
    description="In dit artikel worden de één Sign-Out SAML-Protocol in Azure Active Directory"
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


# <a name="single-sign-out-saml-protocol"></a>Eén afmelden SAML-Protocol

Azure Active Directory (Azure AD) ondersteunt de SAML 2.0 web één afmelden profiel van de browser. Voor één afmelden om correct, moet Azure AD registreren de URL van de metagegevens tijdens de registratie van toepassing. Azure AD krijgt de URL Meld u af en de bijbehorende sleutel van de cloudservice uit de metagegevens. Azure AD wordt de bijbehorende sleutel gebruikt om te controleren of de handtekening in de binnenkomende LogoutRequest en wordt de LogoutURL gebruikt om gebruikers te leiden nadat ze bent afgemeld.

Als de cloudservice biedt geen ondersteuning voor een eindpunt metagegevens, nadat de toepassing is geregistreerd, moet de ontwikkelaar contact opnemen met Microsoft ondersteuning bieden van de URL van de Meld u af en de bijbehorende sleutel.

In dit diagram ziet u de werkstroom van het proces Azure AD één afmelden.

![Eén afmelden werkstroom](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

De cloud-service verzendt een `LogoutRequest` bericht Azure AD om aan te geven dat een sessie is beëindigd. Het volgende fragment toont een voorbeeld `LogoutRequest` element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

De `LogoutRequest` verzonden naar Azure AD element is vereist voor de volgende kenmerken:

- `ID`: Hiermee wordt het afmelden verzoek geïdentificeerd. De waarde van `ID` niet beginnen met een getal. De normale manier is om de **id** toevoegen aan de tekenreeksweergave van een GUID.

- `Version`: Stel de waarde van dit element naar **2.0**. Deze waarde is vereist.

- `IssueInstant`: Dit is een `DateTime` tekenreeks met een waarde coördineren Universal Time (UTC) en [heen indeling ('o'")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD verwacht een waarde van dit type, maar deze niet wordt toepassen.

- De `Consent`, `Destination`, `NotOnOrAfter` en `Reason` kenmerken worden genegeerd als ze zijn opgenomen een `LogoutRequest` element.

### <a name="issuer"></a>Uitgever

De `Issuer` element in een `LogoutRequest` moet precies overeenkomen met een van de **ServicePrincipalNames** in de cloudservice in Azure AD. Dit is meestal ingesteld met de **App-ID-URI** die is opgegeven tijdens de registratie van toepassing.

### <a name="nameid"></a>NameID

De waarde van de `NameID` element moet precies overeenkomen met de `NameID` van de gebruiker die wordt ondertekend af.
## <a name="logoutresponse"></a>LogoutResponse

Azure AD verzendt een `LogoutResponse` in antwoord op een `LogoutRequest` element. Het volgende fragment toont een voorbeeld `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure AD-sets de `ID`, `Version` en `IssueInstant` -waarden in de `LogoutResponse` element. Ook wordt de `InResponseTo` element dat moet worden de waarde van de `ID` kenmerk van de `LogoutRequest` die opgewekt door het antwoord.

### <a name="issuer"></a>Uitgever

Azure AD deze waarde ingesteld op `https://login.microsoftonline.com/<TenantIdGUID>/` waar <TenantIdGUID> is de ID van de tenant van de Azure AD-tenant.

Berekend van de waarde van de `Issuer` element, gebruik de waarde van de **App-ID-URI** tijdens de registratie van toepassing opgegeven.

### <a name="status"></a>Status

Azure AD-gebruik de `StatusCode` element in de `Status` element om aan te geven het slagen of mislukken van afmelden. Wanneer het afmelden niet lukt, de `StatusCode` element kan ook aangepaste foutberichten bevatten.
