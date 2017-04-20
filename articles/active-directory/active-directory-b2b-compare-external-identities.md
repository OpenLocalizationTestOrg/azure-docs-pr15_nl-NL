<properties
   pageTitle="Vergelijking van functies voor het beheer van externe identiteiten met Azure Active Directory | Microsoft Azure"
   description="Verschilt van Azure Active Directory B2B samenwerking, B2C en meerdere tenant App ter ondersteuning van verificatie en machtiging voor externe identiteiten"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Vergelijking van functies voor het beheer van externe identiteiten met Azure Active Directory

Naast het mobiele werknemers toegang tot SaaS apps beheren, kan Azure Active Directory (Azure AD) ervoor zorgen dat uw organisatie resources delen met zakenpartners en toepassingen op consumenten en bedrijven kunt overbrengen.

## <a name="developing-applications-for-businesses"></a>Ontwikkelen van toepassingen voor bedrijven

Biedt u een service of toepassing, zoals een service salarisadministratie voor bedrijven? Azure AD biedt de identiteit-platform waarmee u toepassingen maken die naadloos integreren met miljoenen organisaties die Azure AD al hebt geconfigureerd als onderdeel van de Office 365 of andere enterprise-services.

**Voorbeeld:** Een farmaceutische distributeur biedt medisch voorraad en informatiesystemen voor de gezondheidszorg. Ze nodig voor het veld van een toepassing analytics medisch procedures en gewenste klanten hun eigen identiteiten beheren. Dit bedrijf kiezen Azure AD als het platform identiteit voor de oefening management-toepassing en Azure AD identiteiten bieden aan hun klanten apenstaartje omhoog, indien nodig. Zie [Azure Active Directory developer's guide](active-directory-developers-guide.md)voor meer informatie.

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Zakelijke partnertoegang bieden tot uw zakelijke resources

Hebt u zakenpartners of aan anderen buiten uw organisatie die moeten toegang hebben tot uw ondernemingsresources, zoals een SharePoint-site of uw ERP-systeem? Azure AD kan beheerders aan externe gebruikers (die mogelijk of bestaat niet in Azure AD) eenmalige toegang tot bedrijfstoepassingen te verlenen. Dit verbetert de beveiliging zoals gebruikers toegang verliezen wanneer ze de partnerorganisatie, verlaten, terwijl u clienttoegangsbeleid binnen uw organisatie regelt. Dit ook maakt eenvoudiger beheer terwijl u niet nodig hebt voor het beheren van een externe partner directory of per partner Federatie relaties.

**Voorbeeld:** Een afbeeldingen bedrijf winkels biedt met foto imaging services en het grootste detailhandel-netwerk van afdrukken kiosk werkt. Ze kiezen Azure AD om in te schakelen duizenden gebruikers aan hun zakenpartners detailhandel hun eigen referenties gebruiken downloaden van de meest recente partner marketingmaterialen en opnieuw ordenen foto processing voorraad van het bedrijf producent extranet. Zie [Azure AD B2B samenwerking](active-directory-b2b-what-is-azure-ad-b2b.md)voor meer informatie.

## <a name="developing-applications-for-consumers"></a>Ontwikkelen van toepassingen voor consumenten

Moet u veilig en voordelig online toepassingen, zoals een detailhandel store voorgrond, naar miljoenen consumenten publiceren? Azure AD biedt een platform inschakelen sociale evenals aanmeldingsproblemen gebruikersnaam en wachtwoord van uw huisstijl toepassen zelf te kunnen registreren en selfservice voor wachtwoordherstel voor consumenten van uw toepassing. Hierdoor wordt gemak vergroot voor uw gebruikers te verlagen belasting van uw ontwikkelaars.

**Voorbeeld:** De \#1 Sport franchise ter wereld nodig rechtstreeks koppelen aan de 450 miljoen ventilatoren. Klik hiertoe ingebouwd ze een mobiele app met behulp van Azure AD voor verificatie en profiel opslag van de gebruiker. Ventilatoren vereenvoudigde registratie ophalen en aanmelden door gebruik van de accounts van sociale zoals Facebook, of ze kunnen traditionele gebruikersnaam/wachtwoorden gebruiken voor een aantrekkelijker in iOS, Android en Windows telefoons. Maken op de bestaande aangepaste code Azure AD-platform aanzienlijk verkleind tijdens het geven van de winkel aangepaste huisstijl en bezwaren over beveiliging, gegevens inbreuk op en schaalbaarheid verlichten. Zie [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)voor meer informatie.

## <a name="comparison-of-azure-ad-capabilities"></a>Vergelijking van Azure AD-mogelijkheden

De scenario's die al in dit artikel wordt beschreven is gericht door mogelijkheden binnen Azure AD. In deze tabel moet u verduidelijken welke mogelijkheden meest relevant zijn voor u zijn:

| **Houd rekening met dit product...**       | [Azure AD meerdere tenant SaaS-app](active-directory-developers-guide.md)    | [Azure AD B2B samenwerking](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Als ik moet opgeven...** | een service voor bedrijven | partnertoegang tot mijn apps  | een service voor consumenten |
| **En ik ben strekking...**  | Pharma distributeur      | Installatiekopieën bedrijf            | Sport franchise       |
| **Een app voor implementeren...**  | Beheer van oefening     | Leverancier extranet          | Voetbal ventilatoren            |
| **Doelgroepen...**        | Arts kantoren        | Goedgekeurde zakenpartners | Iedereen met e-mail      |
| **Toegankelijke wanneer...**      | Klant beheerder toestemming | Mijn uitnodigingen voor beheerder           | De consument zich registreert      |
