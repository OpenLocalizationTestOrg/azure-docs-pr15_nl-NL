<properties
    pageTitle="Wat is Microsoft Azure Active Directory licenties? | Microsoft Azure"
    description="Beschrijving van licenties voor Microsoft Azure Active Directory, hoe dit werkt, hoe u aan de slag en aanbevolen procedures, inclusief Office 365, Microsoft Intune en Azure Active Directory Premium en eenvoudige edities"
    services="active-directory"
      keywords="Azure AD-licenties"
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

# <a name="what-is-microsoft-azure-active-directory-licensing"></a>Wat is Microsoft Azure Active Directory licenties?

##<a name="description"></a>Beschrijving
Azure Active Directory (Azure AD) is Microsoft identiteit als een oplossing van Service (IDaaS) en de platform. Azure AD wordt aangeboden in een aantal functionele en technische-versies die variëren van Azure AD-vrij, zodat de is beschikbaar met een Microsoft-service, zoals Office 365, Dynamics, Microsoft Intune en Azure (Azure AD genereert geen eventuele kosten verbruik in deze modus), Azure AD uitgekeerde versies zoals Enterprise mobiliteit Suite (EMS), Azure AD Premium en Basic, evenals Azure meervoudige verificatie MFA (). Meest Azure AD dat is betaald versies worden aangeboden via per gebruiker rechten zoals veel van Microsoft online services, zoals in Office 365, Microsoft Intune en Azure AD. In dat geval de aankoop van de service wordt aangeduid met een of meer abonnementen en elk abonnement bevat een oude aankoop aantal licenties in de tenant. Per gebruiker rechten worden bereikt via licentie toewijzen, een koppeling tussen de gebruiker en het product, zodat de onderdelen van de service voor de gebruiker en gebruik een van de vooruitbetaalde licenties maken.

[Probeer nu Azure AD premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [AZURE.NOTE] Azure AD beheer van de portal is een onderdeel van de Azure klassieke portal. Tijdens het gebruik van Azure AD, hoeft u niet elke Azure aankopen, toegang tot deze portal is vereist een actieve Azure of een [Azure proefabonnement](https://azure.microsoft.com/pricing/free-trial/).

Zie [Wat Azure AD is](active-directory-whatis.md)voor een globaal overzicht van mogelijkheden voor Azure AD-service.
[Meer informatie over de niveaus van Azure AD-service](https://azure.microsoft.com/support/legal/sla/)

> [AZURE.NOTE]  Azure omslagstelsel abonnementen zijn verschillende: terwijl ook weergegeven in uw adreslijst, deze abonnementen maken van Azure resources inschakelen en koppel deze aan uw betalingsmethode. In dit geval zijn er geen licentie telt dat is gekoppeld aan het abonnement. Koppeling van de gebruikers met het abonnement, de gebruikers toegang tot het beheren van bronnen van abonnement, wordt bereikt door het toekennen van machtigingen voor het werken met Azure resources die zijn toegewezen aan het abonnement.


##<a name="how-does-azure-ad-licensing-work"></a>Hoe Azure AD licensing werkt?

Azure AD (recht-basis) op basis van een licentie services werk door een abonnement in uw Azure AD-tenant adreslijstservice/activeren. Zodra het abonnement actief is kunnen de servicemogelijkheden en worden beheerd door beheerders van de adreslijstservice/gebruikt door gelicentieerde gebruikers.

Wanneer u kopen of Enterprise mobiliteit Suite, Azure AD Premium of Azure AD eenvoudige activeren, wordt uw adreslijst wordt bijgewerkt met het abonnement, waaronder de geldigheidsperiode en vooruitbetaalde licenties. Uw abonnementsgegevens, inclusief status, volgende levenscyclus van de gebeurtenis en het aantal licenties toegewezen of beschikbaar is beschikbaar via de portal van Azure klassieke Klik op het tabblad licenties voor de specifieke map. Dit is ook aan de aanbevolen locatie voor het beheren van uw licentietoewijzingen.

Elk abonnement bevat een of meer service-abonnementen, elk toewijzing het opgenomen functioneel niveau van het servicetype; bijvoorbeeld: Azure AD, Azure MFA, Microsoft Intune, Exchange Online of SharePoint Online. Azure AD-Licentiebeheer hoeft niet niveau servicebeheer abonnement. Dit is anders dan Office 365 die afhankelijk van deze modus geavanceerde configuratie is voor het beheren van toegang tot de bijbehorende services. Azure AD is afhankelijk van in de service te configureren, functies inschakelen en beheren van afzonderlijke machtigingen.

In het algemeen, wordt informatie over Azure AD-abonnement beheerd via de Azure klassieke portal, klik op het tabblad licenties voor de specifieke map. Azure AD-abonnementen, met uitzondering van Azure AD Premium, worden niet weergegeven in de portal van Office.

> [AZURE.IMPORTANT] Azure AD Premium en Basic, evenals Enterprise mobiliteit Suite abonnementen, zijn beperkt tot hun ingerichte map/tenant. Abonnementen kunnen niet worden gesplitst tussen mappen of gebruikt om aan te geven van gebruikers in andere mappen. Een abonnement verplaatsen tussen mappen, is mogelijk, maar is vereist voor het indienen van een ondersteuningsticket of annulering en inkoop als rechtstreeks kopen.

> Bij de aanschaf van Azure AD of Enterprise mobiliteit Suite tot en met de activering van de Volume Licensing-abonnement wordt automatisch plaats wanneer de overeenkomst bevat andere Microsoft Online services, zoals Office 365.

Dat is betaald Azure AD-functies omvatten de breedte van de map. Voorbeelden hiervan zijn:
- Op basis van een groep toewijzing tot aan de toepassingen, die onder de specifieke toepassing die u beheert is ingeschakeld.
- Geavanceerde en selfservice-groep management-mogelijkheden zijn beschikbaar onder de configuratie directory of binnen de specifieke groep.
- Premium beveiligingsrapporten zijn op het tabblad Rapportage
- Cloud-toepassing discovery weergegeven in de portal Azure onder identiteit.

###<a name="assigning-licenses"></a>Het toewijzen van licenties
Terwijl het verkrijgen van een abonnement wordt gevormd door alle die u nodig hebt voor het configureren van betaalde mogelijkheden, uw Azure AD betaald functies gebruikt, is vereist distributie van licenties voor de juiste personen. Elke gebruiker die toegang tot nodig hebt of die wordt beheerd via een Azure AD dat is betaald functie moet in het algemeen is een licentie toegewezen. Een licentie-toewijzing is een toewijzing tussen een gebruiker en een gekochte service, zoals Azure AD Premium, Basic of Enterprise mobiliteit Suite.

Het is eenvoudig beheren welke gebruikers in uw adreslijst een licentie nodig hebt. Dit kan worden uitgevoerd door toewijzen aan een groep maken van toewijzingsregels tot en met de portal voor het beheer van Azure AD of door het toewijzen van licenties rechtstreeks naar de juiste personen via een portal, PowerShell of API's. Bij het toewijzen van licenties aan een groep, wordt alle groepsleden een licentie toegewezen. Als gebruikers worden toegevoegd of verwijderd uit de groep deze worden toegewezen of de juiste licentie verwijderd. Toewijzing aan een groep een groepsbeheer die beschikbaar zijn voor u kan gebruiken en komt overeen met de toewijzing tot aan de toepassingen op basis van een groep. Deze methode gebruikt, kunt u instellen van regels, zodat alle gebruikers in uw adreslijst worden automatisch toegewezen of zelfs delegeren de beschikking naar andere managers in de organisatie, ervoor te zorgen dat iedereen met de juiste functienaam een licentie heeft.

Met de toewijzing van de licentie op basis van een groep, wordt elke gebruiker die een gebruikslocatie ontbreken de maplocatie overnemen tijdens toewijzing. Deze locatie kunt op elk gewenst moment door de beheerder worden gewijzigd. In gevallen waarin de geautomatiseerd toewijzing is mislukt vanwege een fout, worden de gegevens van de gebruiker onder die licentietype die staat doorgevoerd.

##<a name="getting-started-with-azure-ad-licensing"></a>Aan de slag met Azure AD-licenties

Aan de slag met Azure AD gaat heel eenvoudig; u kunt altijd uw adreslijst als onderdeel van zich registreert voor een proefabonnement gratis Azure maken. [Meer informatie over het aanmelden als een organisatie](sign-up-organization.md). Het volgende kunt u ervoor zorgen dat uw adreslijst best wordt uitgelijnd op andere Microsoft-services u mogelijk maken of wilt gebruiken en uw doelstellingen bij het verkrijgen van de service.

Hier volgen een paar aanbevolen procedures:
- Als u al een van de organisatie-services van Microsoft gebruikt, hebt u al een Azure AD-directory. In dit geval moet u blijven gebruiken dezelfde map voor andere services, zodat de identiteitsbeheer core, inclusief inrichting en hybride eenmalige aanmelding, kan worden gebruikt op de services. Uw gebruikers krijgen een eenmalige aanmelding-ervaring en profiteren van de mogelijkheden tussen services. Hierdoor als u wilt kopen van een Azure AD-service voor uw werknemers dat is betaald, raadzaam is dat u dezelfde map gebruikt u dit wilt doen.
- Als u van plan bent Azure AD gebruiken voor een andere set gebruikers (partners, klanten en dergelijke), of als u wilt evalueren Azure AD-services en wilt u dat doen in moeten worden geïsoleerd van uw productieservice, of als u op zoek bent om een sandbox-omgeving voor de services in te stellen, raden we maken u eerst een nieuwe map via het klassieke Azure Azure-portal. [Meer informatie over het maken van een nieuwe Azure AD directory in de portal van Azure klassieke](active-directory-licensing-directory-independence.md). De nieuwe map wordt gemaakt met uw account als een externe gebruiker met machtigingen voor globale beheerders. Als u zich bij de portal van Azure klassieke met dit account aanmelden, is mogelijk om te zien van deze map en alle beheerderstaken voor directory openen. Het is raadzaam dat u een lokale account met de juiste bevoegdheden maken voor het beheren van andere Microsoft-services (die niet toegankelijk is via de portal van Azure klassieke). [Meer informatie over het maken van gebruikersaccounts in Azure AD](active-directory-create-users.md).

> [AZURE.NOTE] Azure AD ondersteunt "externe gebruikers," welke gebruikersaccounts in een exemplaar van Azure AD die zijn gemaakt met een Microsoft-Account (MSA) of een Azure AD-identiteit uit een andere map. Hoewel we bezet worden uitbreiden deze mogelijkheid in alle organisatie-services van Microsoft, nu deze accounts niet ondersteund in een aantal mogelijkheden van de services; bijvoorbeeld: het beheer van de Office 365-portal niet ondersteunt momenteel deze gebruikers. Hierdoor kunnen is externe gebruikers met een Microsoft-account niet mogelijk voor het beheer van de Office 365-portal, toegang tot terwijl u externe gebruikers uit andere mappen Azure AD worden genegeerd. In dat geval wordt alleen de lokale gebruikersaccount, de Azure AD of Office 365 directory waar de gebruiker oorspronkelijk is gemaakt, is toegankelijk is via deze ervaringen.

Zoals aangegeven, heeft Azure AD verschillende betaalde versies. Deze versies hebt kleine verschillen in hun beschikbaarheid aankoop:


| Product   | EA/VOLUMELICENTIES     | Openen  |   CSP |   MPN gebruiksrechten  |   Rechtstreekse aankoop | Proefabonnement |
|---|---|---|---|---|---|---|
| Enterprise mobiliteit Suite |   X | X | X | X |  |      X |
| Azure AD Premium  | X | X | X |   | X | X |
| Azure AD-Basic    | X | X | X | X |  <br /> |  <br />  |

###<a name="select-one-or-more-license-trials"></a>Selecteer een of meer licentie experimenten
 In alle gevallen kunt u een proefabonnement Azure AD Premium of Enterprise mobiliteit Suite activeren door de specifieke proefabonnement dat u op het tabblad licenties in uw adreslijst wilt selecteren. Een proefabonnement bevat een abonnement van 30 dagen met 100 licenties.

![Azure Active Directory-proefabonnement-abonnementen](./media/active-directory-licensing-what-is/trial_plans.png)

![Abonnementen voor Enterprise mobiliteit Suite proefabonnement](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Actieve proefabonnement plannen](./media/active-directory-licensing-what-is/active_license_trials.png)

###<a name="assign-licenses"></a>Licenties toewijzen
Als het abonnement actief is, moet u een licentie toewijzen aan uzelf en vernieuw de browser om ervoor te zorgen ziet u alle functies van uw. De volgende stap is betaald licenties toe te wijzen aan de gebruikers die toegang krijgen tot of worden opgenomen in moet Azure AD-functies. Terwijl we hierboven genoemde in 'Licenties toewijzen', is de beste manier hiervoor aan de groep dat staat voor het gewenste publiek identificeren en toewijzen aan de licentie; op deze manier worden gebruikers die worden toegevoegd of verwijderd uit de groep over de levenscyclus van de worden toegewezen aan of verwijderd uit de licentie.

Als u wilt een licentie toewijst aan een groep of individuele gebruikers, selecteert u de licentie-abonnement dat u wilt toewijzen en klik op **toewijzen** op de opdrachtbalk.

![Actieve proefabonnement plannen](./media/active-directory-licensing-what-is/assign_licenses.png)

Eenmaal in het dialoogvenster toewijzing voor het geselecteerde plan, kunt u gebruikers en deze toe te voegen aan de kolom **toewijzen** aan de rechterkant. U kunt door de lijst met gebruikers bladeren of zoeken naar specifieke personen met de gevonden op de voorgrond glas rechtsboven in het raster gebruiker. Als u wilt toewijzen groepen, selecteer 'Groepen' in het menu **weergeven** en klik op de knop controleren aan de rechterkant te vernieuwen van de toewijzingen die worden weergegeven.

![Licenties toewijzen aan groepen](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

U kunt nu zoeken of blader door de groepen en deze toevoegen aan de kolom **toewijzen** op dezelfde manier. U kunt deze een combinatie van gebruikers en groepen in één bewerking toewijzen. Klik op de knop controleren in de rechterbenedenhoek van de pagina om het toewijzingsproces.

![Licentiebericht toewijzing voortgang weergegeven](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Wanneer een groep is toegewezen, wordt er in de leden de licenties overnemen binnen 30 minuten, maar gewoonlijk binnen 1 en 2 minuten.

Toewijzingsfouten kunnen optreden tijdens de toewijzing van Azure AD-licentie, maar zijn relatief niet vaak voorkomen. Mogelijke toewijzingsfouten zijn beperkt tot:
- Toewijzing conflict - wanneer een gebruiker een licentie niet compatibel is met de huidige licentie was toegewezen. In dit geval het toewijzen van de nieuwe licentie moeten worden de vorige taak verwijderen.
- Beschikbare licenties overschreden - wanneer het aantal gebruikers in toegewezen groepen beschikbare licenties overschrijden, de status van de gebruikers toewijzing worden doorgevoerd een mislukt vanwege ontbrekende licenties toewijzen.

###<a name="view-assigned-licenses"></a>Weergave toegewezen licenties

Een overzichtsweergave van de toegewezen licenties beschikbaar, toegewezen en volgende abonnement levenscyclus van de gebeurtenis inclusief worden weergegeven op het tabblad **licenties** .

![Conceptuele weergave van toegewezen licenties](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Een uitgebreide lijst met toegewezen gebruikers en groepen, inclusief de status van toewijzing en het pad (directe of overgenomen van een of meer groepen) is beschikbaar bij het navigeren in een licentie-indeling.

![Gegevens weergeven voor een abonnement licentie toegewezen licenties](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Licenties verwijderen is net zo eenvoudig als het toe te wijzen. Als de gebruiker direct wordt toegewezen, of voor een toegewezen groep, kunt u de licentie selecteren van het licentietype, selecteert u **verwijderen**, de gebruiker of groep toevoegen aan de lijst verwijderen en de acceptatie van de actie verwijderen. U kunt ook een licentietype openen, selecteert u de specifieke gebruiker of groep en tikt u op de werkbalk op **verwijderen** . U beëindigt de overname van een gebruiker van een licentie van een groep, hoeft de gebruiker van de groep te verwijderen.

###<a name="extending-trials"></a>Experimenten uitbreiden

Proefabonnement uitbreidingen voor klanten die beschikbaar zijn als selfservice via de Office 365-beheerportal. De beheerder van een klant kunt navigeren naar de [Office-portal](https://portal.office.com/#Billing) (toegang is afhankelijk van de machtigingen voor de Office-portal) en selecteert u uw proefabonnement Azure AD Premium. Klik op de koppeling voor **proefabonnement verlengen** en volg de instructies. Moet u een creditcardnummer invoeren, maar deze moet niet betalen.

![Een proefversie licentie in de portal Office uitbreiden](./media/active-directory-licensing-what-is/extend_license_trial.png)

Klanten kunnen ook een proefabonnement wilt verlengen via een aanvraag ondersteuning aanvragen. De beheerder van een klant kunt navigeren naar de portal van Office 365 [ondersteuningspagina](http://aka.ms/extendAADtrial) (toegang is afhankelijk van de machtigingen voor de pagina van de Office-ondersteuning). Op deze pagina selecteert u 'abonnementen en experimenten"onder functies en"Proefabonnement vragen"onder symptoom. Tot slot voert u informatie over de omstandigheden

![Een licentie proefversie met een verzoek voor ondersteuning van uitbreiden](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Volgende stappen

Nu misschien u wilt configureren en gebruiken van enkele Azure AD Premium-functies.

- [Selfservice-wachtwoorden opnieuw instellen](active-directory-manage-passwords.md)
- [Selfservice-groepsbeheer](active-directory-accessmanagement-self-service-group-management.md)
- [Azure AD Connect Health](active-directory-aadconnect-health.md)
- [Toewijzing aan een groep naar toepassingen](active-directory-manage-groups.md)
- [Azure meervoudige verificatie](../multi-factor-authentication/multi-factor-authentication.md)
- [Directe aankoop van Azure AD Premium-licenties](http://aka.ms/buyaadp)
