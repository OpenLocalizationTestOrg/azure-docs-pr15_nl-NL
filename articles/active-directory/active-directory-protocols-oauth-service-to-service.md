<properties
    pageTitle="Azure AD Services Auth met OAuth2.0 | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe met de HTTP-berichten implementeren Services verificatie op basis van de stroom OAuth2.0 client referenties verlenen."
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

# <a name="service-to-service-calls-using-client-credentials"></a>Service aan service gesprekken met de clientreferenties van de

Het OAuth 2.0-Client referenties verlenen Flow toestemming geeft dat een webservice (een *vertrouwelijke client*) verifiëren bij het aanroepen van een andere webservice, in plaats van zich voordoen als een gebruiker met een eigen referenties. In dit scenario de client is meestal een middelste laag webservice, een daemon-service of website.

## <a name="client-credentials-grant-flow-diagram"></a>Clientreferenties verlenen gegevensstroom-diagram

In het volgende diagram wordt uitgelegd hoe de stroom werkt in Azure Active Directory (Azure AD) voor het verlenen van de referenties van de client.

![OAuth2.0 Client referenties verlenen stroom](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. De clienttoepassing wordt geverifieerd door naar het eindpunt van de token uitgifte Azure AD en vraagt een toegangstoken.
2. Het eindpunt van de token uitgifte Azure AD problemen het toegangstoken.
3. Het toegangstoken wordt gebruikt om de beveiligde resource te verifiëren.
4. Gegevens uit de beveiligde resource is geretourneerd naar de webtoepassing.

## <a name="register-the-services-in-azure-ad"></a>De Services registreren in Azure AD

Zowel de bellen service en de ontvangst-service registreren in Azure Active Directory (Azure AD). Zie voor gedetailleerde informatie kunt [toevoegen, bijwerken, en een App verwijderen](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Een toegangstoken aanvragen

Als u een toegangstoken, gebruikt u een HTTP POST aan de tenant / regiospecifieke Azure AD-eindpunt.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Service-naar-service token toegangsaanvraag

Een service-naar-service token toegangsaanvraag bevat de volgende parameters.

| Parameter | | Beschrijving |
|-----------|------|------------|
| response_type | Vereist | Geeft het antwoordtype gevraagde. De waarde moet in een stroom Client referenties verlenen **client_credentials**.|
| client_id | Vereist | Hiermee geeft u de Azure AD-client-id van de bellen webservice. Als u wilt zoeken van de bellen toepassing client-ID, in de beheerportal Azure, klik op **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**.|
| client_secret | Vereist |  Voer een code die zijn geregistreerd voor de bellen webservice in Azure AD in. Als u wilt een sleutel in de beheerportal Azure maken, klikt u op **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**. |
| resource | Vereist | Voer de App-ID-URI van de ontvangst webservice. Als u de App-ID-URI, in de beheerportal Azure zoekt, klikt u op **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**. |

## <a name="example"></a>Voorbeeld

Het volgende HTTP-bericht om een toegangstoken voor de webservice https://service.contoso.com/ vraagt. De `client_id` de webservice waarin het toegangstoken aanduidt.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Service-naar-Service Access Token antwoord

Een antwoord success bevat een reactie JSON OAuth 2.0 met de volgende parameters.

| Parameter   | Beschrijving |
|-------------|-------------|
|access_token |De gevraagde toegangstoken. De bellen webservice kunt u deze token gebruiken om te verifiëren bij de ontvangst webservice. |
|access_type  | Geeft de waarde type token. Het enige type die ondersteuning biedt voor Azure AD is **vruchtdragende**. Zie voor meer informatie over vruchtdragende tokens, de [OAuth 2.0 autorisatie Framework: vruchtdragende Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Hoe lang het toegangstoken geldt (in seconden).|
|expires_on   |Het tijdstip waarop het toegangstoken verloopt. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC totdat de verlooptijd. Deze waarde wordt gebruikt om te bepalen de levensduur van in de cache tokens. |
|resource     | De App-ID-URI van de ontvangst webservice. |

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld ziet u een reactie success op een verzoek om een toegangstoken bij een webservice.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Zie ook

* [OAuth 2.0 in Azure AD](active-directory-protocols-oauth-code.md)
