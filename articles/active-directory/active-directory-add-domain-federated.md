<properties
    pageTitle="Voeg uw aangepaste domeinnaam en stel van federatieve aanmelding met Azure Active Directory | Microsoft Azure"
    description="Het toevoegen van uw bedrijf domeinnamen aan Azure Active Directory en hoe instellen federatieve aanmelding tussen Azure Active Directory en uw on-premises implementatie Federatie-oplossing."
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
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Uw aangepaste domeinnaam toevoegen aan Azure Active Directory

U kunt een aangepaste domeinnaam, zoals 'contoso.com,' configureren zodat gebruikers op contoso.com beschikken over een federatieve eenmalige aanmelding ervaring met uw bedrijfsnetwerk. Als u al Active Directory Federation Services (AD FS) of een ander federatieserver waarop uw bedrijfsnetwerk, kunt u Azure AD als de naam van uw aangepaste domein met het hulpprogramma Azure AD Connect wilt gebruiken. U kunt ook Azure AD Connect gebruiken om te implementeren van een nieuwe AD FS-omgeving en configureren die voor federatieve eenmalige aanmelding naar Azure AD.

Als u geen hebt en niet van plan bent om te implementeren AD FS of een ander federatieserver, volgt u deze instructies: [een aangepaste domeinnaam met Azure Active Directory toevoegen](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Een aangepaste domeinnaam toevoegen aan uw adreslijst

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/) met een gebruikersaccount waarmee een globale beheerder van de Azure AD-map.

2. In **Active Directory**, open de map en selecteer het tabblad **Domains** .

3. Klik op de balk met opdrachten, selecteert u **toevoegen**en voer vervolgens de naam van uw aangepaste domein, zoals 'contoso.com'. Zorg ervoor dat de .com, .net of andere op het hoogste niveau extensie opnemen.

4. Schakel het selectievakje **ik van plan bent om dit domein voor eenmalige aanmelding met mijn lokale Active Directory te configureren** .

5. Selecteer **toevoegen**.

Het hulpprogramma Azure AD Connect als u het DNS-fragment dat Azure AD wordt gebruikt om te controleren of het domein wilt uitvoeren. Hier ziet u de DNS-vermelding in de stap **Azure AD-domein** in de wizard. U kunt zien wat die stap in de wizard eruitziet [in deze instructies](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Als u het hulpprogramma Azure AD Connect niet hebt, kunt u [deze hier downloaden](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>De DNS-vermelding toevoegen voor de domeinnaamregistrar voor het domein

De volgende stap voor het gebruik van uw aangepaste domeinnaam met Azure AD is bij de DNS-zonebestand voor het domein. Hiermee worden Azure AD om te bevestigen dat uw organisatie eigenaar is van de aangepaste domeinnaam.

1. Meld u aan bij de website van voor de naam van de domeinregistrar voor uw domeinnaam. Als u geen toegang tot hiervoor hebt, vraagt u de persoon of een team in uw organisatie die deze toegang heeft tot het uitvoeren van stap 2 en zodat u weet wanneer deze is voltooid.

2. Werk de DNS-zonebestand voor het domein door toe te voegen van de DNS-vermelding die u hebt gekregen Azure AD. Deze DNS-vermelding kunt Azure AD om te controleren of het eigendom van het domein. De DNS-vermelding wordt elke gedrag zoals e-mailroutering of webhosting niet gewijzigd.

Voor hulp bij deze stap, Lees [de instructies voor het toevoegen van een DNS-vermelding voor populaire DNS-Registrar](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Controleer of de naam van het domein met Azure AD

Zodra u de DNS-vermelding hebt toegevoegd, bent u gereed om te controleren of de naam van het domein met Azure AD.

Als u wilt controleren of het domein, selecteert u **volgende** op de stap **Azure AD domein** van de wizard Azure AD Connect. Azure AD ziet er voor de DNS-vermelding in het DNS-zonebestand voor het domein. De domeinnaam Azure AD alleen verifiëren als de DNS-records hebt doorgegeven. Vaak doorgeven alleen seconden duurt, maar het kan soms een uur of langer duren. Als verificatie niet de eerste keer niet werkt, probeer het later opnieuw.

Vervolgens gaat u verder met de overige stappen in de wizard Azure AD Connect. Hiermee worden gebruikers van uw Windows-Server AD Azure AD gesynchroniseerd. Gesynchroniseerde gebruikers in het domein dat u hebt geconfigureerd voor Federatie is mogelijk om een federatieve eenmalige aanmelding ervaring te Azure AD uit uw bedrijfsnetwerk.

## <a name="troubleshooting"></a>Problemen oplossen

Als u een aangepaste domeinnaam niet worden geverifieerd, probeert u het volgende. We beginnen met de meest voorkomende en naar beneden af op het kleinste gemene werken.

1.  **Een uur wachten voordat**. DNS-records moeten doorgeven voordat Azure AD kunt controleren of het domein. Dit kan een uur of langer duren.

2.  **Zorg ervoor dat de DNS-record hebt ingevoerd, en of deze juist is**. Deze stap op de website uitvoeren voor de domeinnaamregistrar voor het domein. Azure AD niet worden de domeinnaam geverifieerd, als de DNS-vermelding niet aanwezig zijn in het DNS-zonebestand is, of als het is niet exact overeen met de DNS-vermelding die Azure AD geleverd, kunt u. Als u geen toegang tot het bijwerken van DNS-records voor het domein dat voor de domeinnaamregistrar, de DNS-vermelding delen met de persoon of een team bij uw organisatie die deze toegang heeft en vraag of ze kunnen de DNS-vermelding toevoegen.

3.  **De naam van het domein vanuit een andere map in Azure AD verwijderen**. Een domeinnaam kunt in slechts één map worden geverifieerd. Als een domeinnaam eerder is gecontroleerd in een andere map, moet deze worden verwijderd er voordat deze kan worden geverifieerd in uw nieuwe map. Lees meer over het verwijderen van domeinnamen, [aangepaste domeinnamen die beheren](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Meer aangepaste domeinnamen toevoegen

Als uw organisatie gebruikmaakt van meerdere namen van de aangepaste domein, zoals 'contoso.com' en 'contosobank.com', kunt u deze toevoegen met een maximum van 900 domeinnamen. Gebruik dezelfde stappen in dit artikel om toe te voegen elk van de domeinnamen van uw.

## <a name="next-steps"></a>Volgende stappen

-   [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md)
-   [Meer informatie over de basisbeginselen over het beheer van domein in Azure AD](active-directory-add-domain-concepts.md)
-   [Weergeven van de huisstijl van uw bedrijf wanneer uw gebruikers zich aanmelden](active-directory-add-company-branding.md)
-   [PowerShell gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
