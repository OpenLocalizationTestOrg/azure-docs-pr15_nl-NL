<properties
    pageTitle="Uw aangepaste domeinnaam toevoegen aan Azure Active Directory preview | Microsoft Azure"
    description="Het toevoegen van uw bedrijf domeinnamen aan Azure Active Directory, en hoe om te controleren of de domeinnaam."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Een aangepaste domeinnaam toevoegen aan Azure Active Directory-voorbeeld

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-domains-add-azure-portal.md)
- [Azure klassieke portal](active-directory-add-domain.md)

U kunt een of meer domeinnamen die uw organisatie gebruikmaakt van zakendoet en uw gebruikers zich aanmelden met uw bedrijfsnetwerk met de naam van uw bedrijfsdomein hebt geïmporteerd. Voorbeeld van de Azure Active Directory (Azure AD) gebruikt, kunt u de naam van uw bedrijfsdomein toevoegen aan Azure AD ook. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) Hiermee kunt u toewijzen gebruikersnamen in de adreslijst waarmee, zoals uw gebruikers bekend zijn ‘alice@contoso.com.’ het proces is heel eenvoudig:

1. De aangepaste domeinnaam toevoegen aan uw adreslijst
2. Een DNS-vermelding toevoegen voor de domeinnaam voor de domeinnaamregistrar
3. Controleer of de aangepaste domeinnaam in Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Hoe voeg ik een domeinnaam?

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2.  Selecteer **meer services**, **Azure Active Directory** invoeren in het tekstvak en druk vervolgens op **Enter**.

    ![Gebruikersbeheer openen](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Selecteer op het blad ***mapnaam*** **Domain names**.

4. Selecteer de opdracht **toevoegen** op het blad ** *directory-naam* - Domain names** .

  ![De opdracht toevoegen selecteren](./media/active-directory-domains-add-azure-portal/add-command.png)

5. Klik op het blad **domeinnaam** , voer de naam van uw aangepaste domein in het vak, zoals 'contoso.com', en selecteer **Domein toevoegen**. Zorg ervoor dat de .com, .net of andere op het hoogste niveau extensie opnemen.

6. Klik op het blad ***domeinnaam*** (dat wil zeggen het blad dat wordt geopend met de naam van uw nieuwe domein in de titel), kunt u de DNS-vermelding-gegevens die Azure AD om te bevestigen dat uw organisatie eigenaar is van de aangepaste domeinnaam wilt gebruiken.

  ![DNS-vermeldingsgegevens ophalen](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

U kunt de naam van het domein hebt toegevoegd, moet Azure AD controleren dat uw organisatie eigenaar is van de domeinnaam. Voordat Azure AD deze controle uitvoeren kan, moet u een DNS-vermelding toevoegen in het DNS-zonebestand voor de domeinnaam. Deze taak wordt uitgevoerd op de website van voor de domeinnaamregistrar voor de domeinnaam.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>De DNS-vermelding toevoegen voor de domeinnaamregistrar voor het domein

De volgende stap voor het gebruik van uw aangepaste domeinnaam met Azure AD is bij de DNS-zonebestand voor het domein. Hiermee worden Azure AD om te bevestigen dat uw organisatie eigenaar is van de aangepaste domeinnaam.

1.  Meld u aan bij de domeinnaamregistrar voor het domein. Als u geen toegang tot het bijwerken van de DNS-vermelding, vraagt u de persoon of een team met deze toegang voor stap 2 en zodat u weet wanneer deze is voltooid.

2.  Werk de DNS-zonebestand voor het domein door toe te voegen van de DNS-vermelding die u hebt gekregen Azure AD. Deze DNS-vermelding kunt Azure AD om te controleren of het eigendom van het domein. De DNS-vermelding wordt elke gedrag zoals e-mailroutering of webhosting niet gewijzigd.

Voor hulp bij deze de DNS-vermelding toevoegen, Lees [de instructies voor het toevoegen van een DNS-vermelding voor populaire DNS-Registrar](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Controleer of de naam van het domein met Azure AD

Zodra u de DNS-vermelding hebt toegevoegd, bent u gereed om te controleren of de naam van het domein met Azure AD.

Een domeinnaam kan worden gecontroleerd alleen nadat de DNS-records hebt doorgegeven. Deze doorgifte vaak alleen seconden duurt, maar het kan soms een uur of langer duren. Als verificatie niet de eerste keer niet werkt, probeer het later opnieuw.

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2.  Selecteer **Bladeren**, Gebruikersbeheer invoeren in het tekstvak en druk vervolgens op **Enter**.

    ![Gebruikersbeheer openen](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Selecteer op het blad **Gebruikersbeheer - Domain names** de niet-geverifieerde domeinnaam die u wilt controleren.

4. Selecteer op het blad ***domeinnaam*** (dat wil zeggen het blad dat wordt geopend met de naam van uw nieuwe domein in de titel) **verifiëren** de verificatie voltooien.

Nu kunt u [dat uw aangepaste domeinnaam opnemen gebruikersnamen toewijzen](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Problemen oplossen

Als u een aangepaste domeinnaam niet worden geverifieerd, probeert u het volgende. We beginnen met de meest voorkomende en naar beneden af op het kleinste gemene werken.

1.  **Een uur wachten voordat**. DNS-records moeten doorgeven voordat Azure AD kunt controleren of het domein. Dit kan een uur of langer duren.

2.  **Zorg ervoor dat de DNS-record hebt ingevoerd, en of deze juist is**. Deze stap op de website uitvoeren voor de domeinnaamregistrar voor het domein. Azure AD niet worden de domeinnaam geverifieerd, als de DNS-vermelding niet aanwezig zijn in het DNS-zonebestand is, of als het is niet exact overeen met de DNS-vermelding die Azure AD geleverd, kunt u. Als u geen toegang tot het bijwerken van DNS-records voor het domein dat voor de domeinnaamregistrar, de DNS-vermelding delen met de persoon of een team bij uw organisatie die deze toegang heeft en vraag of ze kunnen de DNS-vermelding toevoegen.

3.  **De naam van het domein vanuit een andere map in Azure AD verwijderen**. Een domeinnaam kunt in slechts één map worden geverifieerd. Als een domeinnaam eerder is gecontroleerd in een andere map, moet deze worden verwijderd er voordat deze kan worden geverifieerd in uw nieuwe map. Lees meer over het verwijderen van domeinnamen, [aangepaste domeinnamen die beheren](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Meer aangepaste domeinnamen toevoegen

Als uw organisatie gebruikmaakt van meerdere namen van de aangepaste domein, zoals 'contoso.com' en 'contosobank.com', kunt u deze toevoegen met een maximum van 900 domeinnamen. Gebruik dezelfde stappen in dit artikel om toe te voegen elk van de domeinnamen van uw.

## <a name="next-steps"></a>Volgende stappen

[Aangepaste domeinnamen beheren](active-directory-domains-manage-azure-portal.md)
