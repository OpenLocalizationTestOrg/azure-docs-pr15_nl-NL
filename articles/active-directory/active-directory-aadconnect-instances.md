<properties
    pageTitle="Azure AD Connect: Synchroniseren exemplaren van de service | Microsoft Azure"
    description="Deze pagina documenten aandachtspunten voor het Azure AD-exemplaren."
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
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Aandachtspunten voor het exemplaren
Azure AD Connect wordt meestal gebruikt met het exemplaar wereldwijde van Azure AD en Office 365. Maar er zijn ook andere exemplaren en deze hebben verschillende vereisten voor URL's en andere aandachtspunten.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Duitsland
De [Microsoft Cloud Duitsland](http://www.microsoft.de/cloud-deutschland) is een soevereine cloud dat wordt beheerd door een beheerder Duitse gegevens.

Deze cloud is momenteel in de proefversie. Veel van de scenario's die normaal gesproken u door zelf, zoals doen kunt domeinen verifiëren, moeten worden uitgevoerd door de operator. Neem contact op met uw lokale Microsoft-vertegenwoordiger voor meer informatie over het deel te nemen in de Preview-versie.

URL's openen in de proxyserver |
--- |
\*. microsoftonline.de |
\*. windows.net |
+ Certificaatintrekkingslijsten |

Wanneer u zich aanmeldt bij uw adreslijst Azure AD moet u een account in het domein onmicrosoft.de.

Functies die momenteel niet aanwezig zijn in de Microsoft Cloud-Duitsland:

- Azure AD verbinding status is niet beschikbaar.
- Automatische updates is niet beschikbaar.
- Wachtwoord write-backs is niet beschikbaar.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure overheid cloud
De [Microsoft Azure overheid cloud](https://azure.microsoft.com/features/gov/) is een cloud voor Amerikaanse overheid.

Oudere versies van DirSync heeft deze cloud is ondersteund. Uit build 1.1.180 van Azure AD Connect de volgende wordt genereren van de cloud ondersteund. Deze generatie werkt in de VS alleen-lezen op basis eindpunten en hebben een andere lijst met URL's om te openen in uw proxyserver.

URL's openen in de proxyserver |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
+ Certificaatintrekkingslijsten |

Azure AD Connect worden niet automatisch kunnen detecteren dat het telefoonboek van uw Azure AD bevindt zich in de cloud overheid. In plaats daarvan moet u de volgende acties uitvoeren tijdens de installatie van Azure AD Connect.

1. Start de installatie van Azure AD Connect.
2. Zodra u de eerste pagina waar u gewoonlijk om de gebruiksrechtovereenkomst te accepteren ziet, ga niet verder maar laat u de installatiewizard uitgevoerd.
3. Start regedit en de registersleutel wijzigen `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` aan de waarde `2`.
4. Ga terug naar de installatiewizard Azure AD Connect accepteer de gebruiksrechtovereenkomst en doorgaan. Zorg ervoor dat u het **aangepaste configuratie** installatiepad gebruiken (en niet Express installatie) tijdens de installatie. Vervolgens verdergaan met de installatie zoals u gewend bent.

Functies die momenteel niet aanwezig is in de cloud Microsoft Azure overheid:

- Azure AD verbinding status is niet beschikbaar.
- Automatische updates is niet beschikbaar.
- Wachtwoord write-backs is niet beschikbaar.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
