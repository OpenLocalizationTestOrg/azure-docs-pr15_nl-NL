<properties
    pageTitle="Azure Active Directory-B2C: Selfservice voor wachtwoordherstel | Microsoft Azure"
    description="Een onderwerp wilt zien waarin het instellen van selfservice-wachtwoord opnieuw instellen voor uw gebruikers in Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory-B2C: Selfservice-wachtwoord opnieuw instellen voor uw gebruikers instellen

Uw consumenten (die zich hebben aangemeld voor lokale accounts) kunnen hun wachtwoorden zelf opnieuw instellen met de functie zelf opnieuw instellen. Dit vermindert de belasting van uw ondersteuningsmedewerkers, met name als uw toepassing miljoenen consumenten via deze regelmatig bevat. Op dit moment alleen ondersteund met een gecontroleerde e-mailadres als een methode voor het herstellen. We wordt extra herstelmethoden (gecontroleerde telefoonnummer, vragen over beveiliging, enzovoort) toevoegen in de toekomst.

> [AZURE.NOTE]
In dit artikel is van toepassing op selfservice-wachtwoord opnieuw instellen die worden gebruikt in de context van een beleid aanmelden. Als u nodig hebt volledig aanpasbaar wachtwoordherstel beleidsregels aangeroepen vanuit de app, raadpleegt u [in dit artikel](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy).

Standaard uw adreslijst hebben geen selfservice-wachtwoord opnieuw instellen is ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/) als de beheerder van abonnement. Dit is de dezelfde werk- of schoolaccount of hetzelfde Microsoft-account waarmee u uw adreslijst gemaakt.
2. Navigeer naar de Active Directory-extensie op de navigatiebalk aan de linkerkant.
3. Zoek uw adreslijst onder het tabblad **map** en klikt u erop.
4. Klik op het tabblad **configureren** .
5. Schuif omlaag naar de sectie **gebruikerswachtwoord opnieuw instellen van beleid** en schakel de optie **gebruikers die zijn ingeschakeld voor het wachtwoord opnieuw instellen** op **Ja**. Zoals u ziet dat de optie **Alternatieve e-mailadres** is ingeschakeld; Laat deze ongewijzigd.

    ![Selfservice-wachtwoorden opnieuw instellen](./media/active-directory-b2c-reference-sspr/sspr.png)

6. Klik op **Opslaan** onder aan de pagina. U bent klaar!

Als u wilt testen, gebruik u de functie "Nu uitvoeren" op elke beleid aanmelden met lokale accounts worden weergegeven als een identiteitsprovider. Klik op de lokale account aanmelden pagina (waar u een e-mailadres en wachtwoord, of een gebruikersnaam en wachtwoord), klikt u op **geen toegang tot uw account?** om te controleren of de consumenten-ervaring.

> [AZURE.NOTE]
De selfservice-wachtwoord opnieuw instellen pagina's kunnen worden aangepast met de [bestaande huisstijl functie bedrijf](../active-directory/active-directory-add-company-branding.md).
