<properties
   pageTitle="Administratieve eenheden management in Azure Active Directory"
   description="Met behulp van administratieve eenheden voor meer gedetailleerde delegatie van machtigingen in Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Administratieve eenheden management in Azure AD - Public Preview

In dit artikel worden administratieve eenheden – een nieuwe Azure Active Directory-container van resources die kunnen worden gebruikt voor het delegeren van beheerdersmachtigingen via subsets van gebruikers en toepassen beleid naar een subset van gebruikers. Administratieve eenheden kunnen in Azure Active Directory, centraal beheerders machtigingen voor beheerders van regionale delegeren of beleid instellen op een gedetailleerd niveau.

Dit is handig in organisaties met onafhankelijke divisies, bijvoorbeeld een grote universiteit die bestaat uit veel zelfstandige scholen (Business school, technische school, enzovoort) die onafhankelijk van elkaar verschillen. Deze onderverdelingen hebben hun eigen IT-beheerders die toegang, gebruikers beheren en beleid specifiek voor hun deling instellen. Centraal-beheerders kunnen wilt verlenen deze divisie beheerders machtigingen via de gebruikers in hun bepaalde afdelingen. Specifieke taken kunt in dit voorbeeld gebruikt, de beheerder van een centrale, bijvoorbeeld maken een administratieve eenheid voor een bepaalde school (Business school) en deze te vullen met alleen de zakelijke school-gebruikers. Vervolgens de beheerder van een centrale de Business school IT-afdeling kunt toevoegen aan een rol bereik met andere woorden, verlenen de IT-afdeling van bedrijven school beheerdersmachtigingen alleen via de Business school administratieve eenheid.

> [AZURE.IMPORTANT]
> U kunt maken en gebruiken van administratieve eenheden inschakelen als u Azure Active Directory Premium. Zie [aan de slag met Azure AD Premium](active-directory-get-started-premium.md)voor meer informatie.

Vanuit het oogpunt van de beheerder van de siteverzameling is een administratieve eenheid een Active directory-object die kan worden gemaakt en kan worden gevuld met resources. **In deze release, kunnen deze resources alleen gebruikers zijn.** Zodra u hebt gemaakt en ingevuld, kan de administratieve eenheid worden gebruikt als bereik de verleend om machtigingen te beperken alleen via bronnen die zich bevinden in de administratieve eenheid.

## <a name="managing-administrative-units"></a>Administratieve eenheden beheren

U kunt in deze release preview maken en beheren van administratieve eenheden gebruik van de Azure Active Directory-Module voor Windows PowerShell-cmdlets.

Zie voor meer informatie over softwarevereisten en installeren van de Azure AD-module, en voor meer informatie over de cmdlets Azure AD-Module voor het beheren van administratieve eenheden, zoals syntaxis, parameter beschrijvingen en voorbeelden [Manage Azure AD via Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Volgende stappen
[Edities van Azure Active Directory](active-directory-editions.md)
