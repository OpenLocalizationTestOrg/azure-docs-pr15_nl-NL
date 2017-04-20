<properties
    pageTitle="Kenmerktoewijzingen aanpassen | Microsoft Azure"
    description="Informatie over het toewijzen van welke kenmerken voor SaaS-apps in Azure Active Directory hoe kunt u deze wijzigen om uw zakelijke behoeften op te lossen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="customizing-attribute-mappings"></a>Kenmerktoewijzingen aanpassen


Microsoft Azure AD biedt ondersteuning voor het inrichten van de gebruiker naar toepassingen van derden SaaS zoals Salesforce, Google Apps en anderen. Als u gebruiker inrichting voor een derde partij SaaS toepassing is ingeschakeld, bepaalt de beheerportal Azure de waarden van de kenmerken in de vorm van een configuratie 'kenmerktoewijzing' genoemd.

Er is een vooraf geconfigureerde set kenmerktoewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten elke SaaS-app. Sommige apps beheren andere soorten objecten, zoals groepen of contactpersonen. <br> 
U kunt de standaard-kenmerktoewijzingen op basis van uw zakelijke behoeften aanpassen. Hierdoor, kunt u wijzigen of maak nieuwe kenmerktoewijzingen of bestaande kenmerktoewijzingen verwijderen.

U kunt deze functie in de Azure AD-portal openen door te klikken op kenmerken in de werkbalk van een toepassing SaaS.

> [AZURE.NOTE] De **kenmerken** -koppeling is alleen beschikbaar als u de inrichting van gebruiker is ingeschakeld voor een toepassing voor SaaS hebt. 


![SalesForce][1] 


Wanneer u kenmerken in de werkbalk, de lijst met huidige toewijzingen die zijn geconfigureerd voor een toepassing voor SaaS klikt.

De volgende schermafbeelding ziet u een voorbeeld hiervoor:



![SalesForce][2]  


In het bovenstaande voorbeeld ziet u dat het kenmerk van de **Voornaam** van een beheerde object in Salesforce wordt gevuld met de waarde **givenName** van de gekoppelde Azure AD-object.

Als u wilt aanpassen van uw kenmerktoewijzingen of als u wilt herstellen, instellingen terug naar de standaardconfiguratie aangepaste, kunt u dit doen door te klikken op de bijbehorende knop op de werkbalk vanaf de onderkant van een toepassing.


![SalesForce][3]  


Kenmerktoewijzingen die nodig zijn voor een toepassing SaaS correct zijn. In de tabel kenmerken hebben de toewijzingen gerelateerde kenmerk **Ja** als de waarde voor het kenmerk **vereist** . Als een kenmerktoewijzing vereist is, kunt u deze niet verwijderen. In dit geval is functie **verwijderen** niet beschikbaar.

Een bestaande kenmerktoewijzing wijzigen, selecteert u de toewijzing en klik vervolgens op **bewerken**. Hiermee opent u een dialoogvenster weer waarmee u voor het wijzigen van de toewijzing van de geselecteerde kenmerken.


![Kenmerktoewijzing bewerken][4]  



## <a name="understanding-attribute-mapping-types"></a>Lidmaatschap kenmerk typen toewijzen


U besturingselement hoe kenmerken worden ingevuld in een derde partij met kenmerktoewijzingen SaaS-toepassing. Er zijn vier verschillende toewijzingstypen die worden ondersteund:

- **Directe** – het kenmerk target wordt gevuld met de waarde van een kenmerk van het gekoppelde object in Azure AD.


- **Constante** – het kenmerk target wordt gevuld met een bepaalde tekenreeks die u hebt opgegeven.


- **Expressie** - met het kenmerk target is ingevuld op basis van het resultaat van een expressie script-achtige. Zie voor meer informatie [Het schrijven van expressies voor het toewijzen van kenmerken in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **Geen** : het kenmerk target is links ongewijzigd. Echter als het kenmerk target ooit leeg is, is deze gevuld met de standaardwaarde die u opgeeft.



Naast deze vier typen in de toewijzing eenvoudige kenmerken ondersteuning voor aangepaste kenmerktoewijzingen het concept van de toewijzing van een **standaardwaarde** . De toewijzing voor de standaard-waarde zorgt ervoor dat een kenmerk target wordt gevuld met een waarde als er geen waarde in Azure AD noch op het doelobject.

Microsoft Azure AD biedt een zeer efficiënte implementatie van een synchronisatieproces. In een geïnitialiseerde omgeving, worden alleen objecten die moet worden bijgewerkt verwerkt tijdens een synchronisatie cyclus. Bijwerken van kenmerktoewijzingen heeft dit gevolgen voor de prestaties van een cyclus van synchronisatie. Dit komt doordat een update van de configuratie van de toewijzing kenmerk is vereist voor alle beheerde objecten om te worden reevaluated. Reden is een aanbevolen aanbevolen wordt om het aantal opeenvolgende wijzigingen aan uw kenmerktoewijzingen ten minste behouden.


##<a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Automatiseren gebruiker inrichting/Deprovisioning aan SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Expressies voor het toewijzen van kenmerken schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Een bereik van Filters voor het inrichten van de gebruiker instellen](active-directory-saas-scoping-filters.md)
- [Gebruikmaken van SCIM waarmee automatisch inrichten van gebruikers en groepen van Azure Active Directory naar toepassingen](active-directory-scim-provisioning.md)
- [Account inrichting van meldingen](active-directory-saas-account-provisioning-notifications.md)
- [Lijst met zelfstudies over het integreren van SaaS Apps](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
