<properties
    pageTitle="Beheer voor bedrijfs-apps in de voorbeeldversie Azure Active Directory van gebruikers | Microsoft Azure"
    description="Informatie over het beheren van de gebruiker account inrichten voor gebruik van de Azure Active Directory-preview bedrijfs-apps"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Voorbeeld: Gebruikersaccount inrichting voor bedrijfs-apps in de nieuwe Azure-portal beheren

In dit artikel wordt beschreven hoe de [Azure-portal](https://portal.azure.com) gebruiken voor het beheren van automatische gebruikersaccount inrichting en verwijderen van gegevens voor toepassingen die ondersteuning bieden voor deze, met name lijsten die zijn toegevoegd in de categorie 'aanbevolen' in de [galerie met Azure Active Directory-toepassing](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Dit probleem management in de nieuwe portal van Azure is momenteel in de proefversie van openbare en in dit artikel worden de nieuwe functies, evenals een paar tijdelijke beperkingen die tijdens de proefperiode in plaats. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md)

Meer informatie over de inrichting van automatische gebruiker-account en hoe deze werkt, raadpleegt u de [inrichting van gebruiker automatiseren en Deprovisioning naar SaaS toepassingen met Azure Active Directory](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Uw apps zoeken in de nieuwe portal

Vanaf September 2016, kunnen alle toepassingen die zijn geconfigureerd voor één aanmelding in een map, door de beheerder van een map met de [galerie met Azure Active Directory-toepassing](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) in de [klassieke Azure-portal](https://manage.windowsazure.com), nu worden bekeken en beheerd in de nieuwe Azure-portal.

Deze toepassingen vindt u in de sectie **Bedrijfstoepassingen** van de nieuwe Azure-portal die kan worden geraadpleegd via het menu **Meer Services** in het linker navigatiegedeelte gebied. Bedrijfs-apps zijn apps die zijn geïmplementeerd en worden gebruikt door gebruikers binnen uw organisatie.

![Bedrijfstoepassingen blade][0]

De koppeling **alle toepassingen** aan de linkerkant te selecteren, ziet u een lijst met alle apps die zijn geconfigureerd, met inbegrip van apps die is toegevoegd vanuit de galerie. Een app selecteren, wordt het blad resource voor deze app, waar rapporten kunnen worden bekeken voor deze app en een aantal instellingen kan worden beheerd geladen.

Instellingen voor het inrichten gebruikersaccount kan worden beheerd door in te schakelen **Provisioning** aan de linkerkant.

![Toepassing resource blade][1]


##<a name="provisioning-modes"></a>Modi inrichting

Het blad **Provisioning** begint met een menu **modus** , waarin wordt getoond welke inrichten modi worden ondersteund voor een enterprise-toepassing, en kan ze worden geconfigureerd. De beschikbare opties omvatten:

* **Automatische** - deze optie wordt weergegeven als Azure AD ondersteunt automatische API gebaseerde inrichting en/of het opheffen van provisioning van gebruikersaccounts aan deze toepassing. Deze modus selecteren wordt weergegeven voor een interface dat netwerkbeheerders configureren van Azure AD verbinding maken met een van de toepassing Gebruikersbeheer API, Accounttoewijzingen en werkstromen die bepalen hoe gegevens van gebruikersaccounts moet stromen tussen Azure AD maken en de app en beheren van de Azure AD inrichting van service.

* **Handmatig** - met deze optie wordt weergegeven als Azure AD biedt geen ondersteuning voor automatische inrichting van gebruikersaccounts aan deze toepassing. Deze optie houdt in dat gebruiker accountrecords die zijn opgeslagen in de toepassing moeten worden beheerd via een extern proces, op basis van de gebruiker management en inrichten van mogelijkheden van die toepassing (waaronder kunt SAML JIT inrichting).


##<a name="configuring-automatic-user-account-provisioning"></a>Inrichting van automatische gebruiker-account configureren

Een scherm die in vier secties is verdeeld selecteren de optie **automatisch** worden weergegeven:

###<a name="admin-credentials"></a>Beheerdersreferenties
Dit is waar de referenties vereist voor Azure AD verbinding maken met een van de toepassing Gebruikersbeheer API zijn ingevoerd. De invoer vereist varieert afhankelijk van de toepassing. Meer informatie over de vereisten voor specifieke toepassingen en referentietypen, raadpleegt u de [zelfstudie voor de configuratie voor specifieke toepassing](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

De knop **Verbinding testen** te selecteren, kunt u testen van de referenties doordat Azure AD poging tot verbinding met de app de inrichting van app met de opgegeven referenties.

###<a name="mappings"></a>Toewijzingen
Dit is waar beheerders kunnen bekijken en bewerken van de stroom van welke gebruiker kenmerken tussen Azure AD en de doeltoepassing, wanneer gebruikersaccounts worden deze is ingericht of bijgewerkt.

Er is een vooraf geconfigureerde set toewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten elke SaaS-app. Sommige apps beheren andere typen objecten, zoals groepen of contactpersonen. Selecteer een van deze toewijzingen in de tabel ziet u de toewijzing editor naar rechts, waar ze kunnen worden bekeken en aangepast.

![Toepassing resource blade][2]

Ondersteunde aanpassingen in de eerste voorbeeldweergave opnemen:

* In- en uitschakelen van toewijzingen voor specifieke objecten, zoals het gebruikersobject Azure AD naar gebruikersobject de SaaS-app.

* Bewerken welke kenmerken flow uit het gebruikersobject Azure AD naar gebruikersobject van de app. Zie voor meer informatie over de toewijzing van kenmerken, [lidmaatschap kenmerk toewijzing grafiektypen](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Welke inrichten filteracties Azure AD moet uitvoeren op de doeltoepassing instellen, een nieuwe functie in de portal van Azure. In plaats van Azure AD volledig synchroniseren objecten, kunt u de acties uitgevoerd beperken. Bijvoorbeeld door alleen **Update**, alleen updates van Azure AD bestaande gebruiker accounts voor in een toepassing en maakt geen nieuwe bestanden selecteren. Als u alleen **maken**, Azure alleen Hiermee maakt u nieuwe gebruikersaccounts maar worden niet bijgewerkt bestaande ideeën. Deze functie kunnen beheerders verschillende toewijzingen voor het maken van een account maken en bijwerken van werkstromen. De volledige mogelijkheid voor het maken van meerdere toewijzingen per app is gepland voor later in de preview-periode.

###<a name="settings"></a>Instellingen
Deze sectie kunt beheerders starten en stoppen de Azure AD inrichting van service voor de geselecteerde toepassing, evenals (optioneel) het inrichten cache wissen en opnieuw starten van de service.

Als de inrichting is worden ingeschakeld voor de eerste keer voor een toepassing, schakelt u de service door de **Inrichting van Status** te **op**wijzigen. Hierdoor de Azure AD inrichting van service voor het uitvoeren van een eerste synchronisatie waar de gebruikers die is toegewezen in de sectie **gebruikers en groepen** worden gelezen, query's van de doeltoepassing instellen voor hen en voert de inrichten acties die zijn gedefinieerd in de sectie Azure AD **toewijzingen** . Tijdens dit proces het inrichten worden opgeslagen in de cache opgeslagen gegevens over welke gebruikersaccounts dit beheert, zodat het niet-beheerde accounts binnen de doeltoepassingen die nooit in het bereik voor de toewijzing waren worden niet beïnvloed door het opheffen van provisioning bewerkingen. Na de eerste synchronisatie, de inrichten service worden automatisch gesynchroniseerd gebruiker en objecten groeperen op een interval tien minuten.

Wijzigen van de **Inrichting van Status** naar **uitschakelen** , onderbreekt u gewoon de inrichten service. In deze status Azure niet maken, bijwerken of verwijderen van een gebruiker of groepsobjecten in de app. De staat wijzigt naar aan de service om te kiezen waar deze was gebleven.

Het selectievakje **huidige staat en start opnieuw synchronisatie** te selecteren en op te slaan drukt, stopt de inrichten service, dumps de in de cache opgeslagen gegevens over welke accounts Azure AD wordt beheerd, de services opnieuw worden gestart en voert de eerste synchronisatie opnieuw uit. Deze optie kan beheerders het inrichten van implementatie opnieuw starten.

###<a name="synchronization-details"></a>Synchronisatiedetails
In deze sectie bevat toevoeging informatie over de werking van de inrichten service, met inbegrip van de eerste en laatste tijden die de inrichten service hebt uitgevoerd ten opzichte van de toepassing en hoeveel gebruikers en objecten groeperen worden beheerd.

Koppelingen zijn mits aan het **creëren activiteitsduur rapport**, welke biedt een logboek van alle gebruikers en groepen gemaakt, bijgewerkte en verwijderde tussen Azure AD en het doel toepassing, en aan het **Provisioning foutrapport** vindt u meer gedetailleerde foutberichten voor gebruiker en groepsobjecten die niet kon worden worden gelezen, gemaakt, bijgewerkt of verwijderd. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
