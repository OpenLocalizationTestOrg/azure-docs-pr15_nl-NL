<properties
    pageTitle="Groepen in Azure Active Directory beheren | Microsoft Azure"
    description="Het maken en beheren van groepen om Azure gebruikers met Azure Active Directory beheren."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Groepen in Azure Active Directory beheren

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-groups-create-azure-portal.md)
- [Azure klassieke portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Een van de functies van Azure Active Directory (Azure AD) Gebruikersbeheer is de mogelijkheid om groepen van gebruikers te maken. Een groep kunt u beheertaken zoals het toewijzen van licenties of machtigingen voor een aantal gebruikers in één keer uitvoeren. U kunt ook groepen gebruiken om te wijzen access

- Resources zoals objecten in de adreslijst
- Resources externe naar de map zoals SaaS-toepassingen, Azure services, SharePoint-sites of on-premises implementatie resources

De eigenaar van een resource kunt bovendien ook toegang toewijzen aan een resource aan een Azure AD-groep eigendom van iemand anders. Deze toewijzing verleent de leden van die groepstoegang tot de resource. Vervolgens beheert de eigenaar van de groep lidmaatschap van de groep. Effectief, de eigenaar van de resource gedelegeerd naar de eigenaar van de groep de machtiging gebruikers toewijzen aan de resource.

## <a name="how-do-i-create-a-group"></a>Hoe maak ik een groep?

Afhankelijk van de services waarvoor uw organisatie zich heeft geabonneerd, kunt u een groep met een van de volgende opties:
- de Azure klassieke portal
- de portal Office 365-account
- de portal voor het account van Windows Intune

Als dat wordt uitgevoerd in de portal van Azure klassieke beschreven taken. Zie [het telefoonboek van uw Azure AD beheren](active-directory-administer.md)voor meer informatie over het gebruik van niet-Azure portals voor het beheren van uw Azure AD-adreslijst.

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com)Selecteer **Active Directory**en selecteer vervolgens de naam van de adreslijst van uw organisatie.

2. Selecteer het tabblad **groepen** .

3. Selecteer de **groep toevoegen**.

4. Geef de naam en de beschrijving van een groep in het venster **Groep toevoegen** .


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Hoe ik toevoegen of verwijderen van individuele gebruikers in een beveiligingsgroep?

**Individuele gebruiker toevoegen aan een groep**

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com)Selecteer **Active Directory**en selecteer vervolgens de naam van de adreslijst van uw organisatie.

2. Selecteer het tabblad **groepen** .

3. Open de groep waaraan u leden wilt toevoegen. Open het tabblad **leden** van de geselecteerde groep als deze niet al wordt weergegeven.

4. Selecteer **leden toevoegen**.

5. Selecteer op de pagina **Leden toevoegen** de naam van de gebruiker of een groep die u wilt toevoegen als lid van deze groep. Zorg ervoor dat deze naam wordt toegevoegd aan het deelvenster **geselecteerde** .


**Individuele gebruiker verwijderen uit een groep**

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com)Selecteer **Active Directory**en selecteer vervolgens de naam van de adreslijst van uw organisatie.

2. Selecteer het tabblad **groepen** .

3. Open de groep waaruit u leden wilt verwijderen.

4. Selecteer het tabblad **leden** , selecteert u de naam van het lid dat u wilt verwijderen uit de groep en klik vervolgens op **verwijderen**.

6. Klik bij de prompt Bevestig dat u wilt deze lid verwijderen uit de groep.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Hoe kan ik het lidmaatschap van een groep dynamisch beheren?

In Azure AD, kunt u heel eenvoudig instellen van een eenvoudige regel om te bepalen welke gebruikers lid zijn van de groep. Een eenvoudige regel is een dat alleen een enkele vergelijking maakt. Bijvoorbeeld als een groep is toegewezen aan een toepassing voor SaaS, u kunt een regel instellen om toe te voegen gebruikers met een functie van "Verkoper". Deze regel verleent vervolgens toegang tot deze toepassing SaaS voor alle gebruikers met deze functie in uw adreslijst.

Wanneer het systeem evalueert eventuele kenmerken van een gebruiker wijzigen, worden alle regels voor dynamische groep in een map om te zien als de wijziging van de gebruiker een groep wilt activeren toegevoegd of verwijderd. Als een gebruiker voldoet aan een regel op een groep, worden ze toegevoegd als lid aan die groep. Als ze niet meer voldoen aan de regel van een groep die ze lid zijn, worden ze verwijderd als een lid uit die groep.

> [AZURE.NOTE] U kunt een regel voor dynamische lidmaatschap op beveiligingsgroepen of Office 365-groepen instellen. Geneste groepslidmaatschappen niet worden momenteel ondersteund voor de toewijzing tot aan de toepassingen op basis van een groep.
>
> Dynamische lidmaatschappen voor groepen vereisen een Azure AD Premium-licentie wilt toewijzen aan
>
> - De beheerder die worden beheerd in de regel op een groep
> - Alle leden van de groep

**Dynamische lidmaatschap voor een groep inschakelen**

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com)Selecteer **Active Directory**en selecteer vervolgens de naam van de adreslijst van uw organisatie.

2. Selecteer het tabblad **groepen** en openen van de groep die u wilt bewerken.

3. Selecteer het tabblad **configureren** en stelt u **Dynamische lidmaatschappen inschakelen** op **Ja**.

4. Een eenvoudige één regel voor de groep om te bepalen hoe dynamische lidmaatschap voor deze groepsfuncties instellen. Zorg ervoor dat de **gebruikers toevoegen waar** optie is geselecteerd en selecteer vervolgens een eigenschap uit de lijst (bijvoorbeeld afdeling, functie, enz.)

5. Selecteer vervolgens een voorwaarde (niet gelijk aan, is gelijk aan, niet begint met, begint met, niet bevat, bevat, niet overeenkomen, vergelijken).

6. Geef een vergelijkingswaarde voor de eigenschap geselecteerde gebruiker.

Zie voor meer informatie over het maken van *Geavanceerde* regels (regels die meerdere vergelijkingen kunnen bevatten) voor dynamische groepslidmaatschap, [werken met kenmerken om geavanceerde regels te maken](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Aanvullende informatie

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Toegang tot resources met Azure Active Directory-groepen beheren](active-directory-manage-groups.md)

* [Azure Active Directory-cmdlets voor het configureren van de groepsinstellingen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)

* [Wat is Azure Active Directory?](active-directory-whatis.md)

* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
