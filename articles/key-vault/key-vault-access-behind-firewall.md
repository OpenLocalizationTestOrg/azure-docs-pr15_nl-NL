<properties
    pageTitle="Toegang tot sleutel kluis achter de firewall | Microsoft Azure"
    description="Informatie over het openen van sleutel kluis van een toepassing achter een firewall"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Toegang krijgen tot toets kluis achter de firewall
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>V: Mijn clienttoepassing belangrijke kluis moet zich achter een firewall, welke poorten/hosts/IP-adressen moet ik openen voor toegang tot belangrijke kluis?

Toegang tot een belangrijke kluis moet de clienttoepassing belangrijke kluis steeds toegang tot meerdere eindpunten voor verschillende functies.

- Verificatie (via Azure Active Directory)
- Beheer van de sleutel-kluis (waaronder maken/gelezen/bijwerken/verwijderen, en ook clienttoegangsbeleid instelling) tot en met Azure resourcemanager
- Openen en beheren van objecten (toetsen en geheimen) die zijn opgeslagen in belangrijke kluis zelf, gaat door het belangrijkste kluis specifieke-eindpunt (bijvoorbeeld [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Afhankelijk van uw configuratie en omgeving, zijn er enkele variaties.   

## <a name="ports"></a>Poorten

Al het verkeer naar belangrijke kluis voor alle 3 functies (access met vlak voor verificatie, beheer en gegevens) is gewijd aan HTTPS: poort 443. Echter voor CRL, wordt er af en toe HTTP (poort 80)-verkeer is toegestaan. Klanten die ondersteuning bieden voor OCSP CRL niet mag worden bereikt, maar mogelijk af en toe [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl)hebt bereikt.  

## <a name="authentication"></a>Verificatie

Toets kluis clienttoepassing moet voor toegang tot de eindpunten van de Azure Active Directory voor verificatie. Het eindpunt gebruikt is afhankelijk van de configuratie van de tenant AAD en het type principal--UPN, service hoofdsom en het type account, bijvoorbeeld Microsoft-Account of organisatie-ID.  

| Hoofdsom Type | Eindpunt: poort |
|----------------|---------------|
| Gebruiker met Microsoft-Account<br> (bijvoorbeelduser@hotmail.com) | **Globale:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure Amerikaanse overheid:**<br> Login-us.microsoftonline.com:443<br><br>**Azure Duitsland:**<br> Login.microsoftonline.de:443<br><br> en <br>Login.live.com:443   |
| Gebruiker/Service principal met organisatie ID AAD (bijvoorbeelduser@contoso.com) | **Globale:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure Amerikaanse overheid:**<br> Login-us.microsoftonline.com:443<br><br>**Azure Duitsland:**<br> Login.microsoftonline.de:443 |
| Gebruiker/Service principal organisatie -ID + ADFS of andere federatieve eindpunt (bijvoorbeeld gebruikenuser@contoso.com) | De bovenstaande eindpunten voor organisatie ID plus ADFS of andere federatieve eindpunten |

Er zijn andere mogelijke complexe scenario's. Raadpleeg [Azure Active Directory verificatie Flow](/documentation/articles/active-directory-authentication-scenarios/), [Toepassingen integreren met Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) en [Protocollen voor Active Directory-verificatie](https://msdn.microsoft.com/library/azure/dn151124.aspx) voor meer informatie.  

## <a name="key-vault-management"></a>Belangrijke kluis Management

Voor sleutel kluis Management (CRUD- en -beleid instelling), moet de clienttoepassing belangrijke kluis toegang tot de resourcemanager Azure-eindpunt.  

| Type bewerking | Eindpunt: poort |
|----------------|---------------|
| Toets kluis besturingselement vlak bewerkingen<br> via Azure resourcemanager | **Globale:**<br> Management.Azure.com:443<br><br> **Azure China:**<br> Management.chinacloudapi.CN:443<br><br> **Azure Amerikaanse overheid:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Duitsland:**<br> Management.microsoftazure.de:443 |
| Azure Active Directory Graph API | **Globale:**<br> Graph.Windows.NET:443<br><br> **Azure China:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure Amerikaanse overheid:**<br> Graph.Windows.NET:443<br><br> **Azure Duitsland:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Belangrijke kluis bewerkingen

Belangrijke kluis client moet toegang tot het eindpunt van de belangrijkste kluis voor alle belangrijke kluis object (toetsen en geheimen) management en cryptografische bewerkingen. Afhankelijk van de locatie van uw sleutel kluis verschilt het achtervoegsel eindpunt. Het eindpunt van de sleutel kluis is van de indeling: < kluis-naam >. < regio-specifieke-dns-achtervoegsel > zoals is beschreven in de onderstaande tabel.  

| Type bewerking | Eindpunt: poort |
|----------------|---------------|
| Toets kluis bewerkingen, zoals cryptografische bewerkingen op sleutels, gemaakt/gelezen/bijwerken/verwijderen sleutels en geheimen, set/get labels en andere kenmerken op belangrijke kluis objecten (toetsen/geheimen)     | **Globale:**<br> &lt;kluis-naam&gt;. vault.azure.net:443<br><br> **Azure China:**<br> &lt;kluis-naam&gt;. vault.azure.cn:443<br><br> **Azure Amerikaanse overheid:**<br> &lt;kluis-naam&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;kluis-naam&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-adresbereiken

Toets kluis service weer gebruikmaakt van een andere Azure resources zoals PaaS infrastructuur, dus is het niet mogelijk een specifiek bereik van IP-adressen die belangrijke kluis service eindpunten op elk gewenst moment. Echter als uw firewall biedt alleen ondersteuning voor IP-adres bereiken vervolgens Raadpleeg het document [Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653) .   Voor verificatie en -identiteit (Azure Active Directory) moet uw toepassing kunnen verbinding maken met de eindpunten die worden beschreven in de [verificatie en identiteit adressen](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Volgende stappen

- Als u vragen over toets kluis hebt, bezoekt u de [Azure toets kluis Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
