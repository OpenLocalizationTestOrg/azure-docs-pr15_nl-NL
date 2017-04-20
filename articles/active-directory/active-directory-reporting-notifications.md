<properties
    pageTitle="Meldingen voor rapportage van Azure Active Directory"
    description="Het gebruik van de Azure Active Directory rapportage-meldingen voor verdacht ins."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Meldingen voor rapportage van Azure Active Directory

## <a name="what-reports-generate-email-notifications"></a>Welke rapporten genereren e-mailmeldingen

Op dit moment kan alleen de onregelmatig aanmelden activiteit rapport triggers e-mailmeldingen.

## <a name="what-is-an-irregular-sign-in"></a>Wat is een 'onregelmatig Sign in'?

Onregelmatige aanmeldingen zijn die zijn geïdentificeerd door onze machine learning-algoritmen, op basis van een voorwaarde voor een "waardoor reizen" gecombineerd met een afwijkende aanmeldingsproblemen locatie en een apparaat. Dit kan betekenen dat een hacker is geprobeerd aan te melden met dit account.

## <a name="who-receives-the-email-notifications"></a>Wie de e-mailmeldingen ontvangt?

Het e-mailbericht wordt verzonden naar alle globale beheerders die een licentie voor Active Directory-Premium zijn toegewezen. Om ervoor te zorgen dat wordt bezorgd, verzenden we dit naar de beheerders alternatieve e-mailadres als u ook. Beheerders moeten worden opgenomen aad-alerts-noreply@mail.windowsazure.com in hun lijst met veilige afzenders, zodat ze niet het e-mailbericht hebt gemist.

## <a name="how-often-are-these-emails-sent"></a>Hoe vaak worden deze e-mailberichten verzonden?

Het e-mailbericht wordt verzonden als 10 nieuwe onregelmatig aanmeldingsproblemen activiteiten in de afgelopen 30 dagen voorkomen, of omdat het laatste e-mailbericht is verzonden, als dit kleiner is.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Hoe krijg ik toegang tot het rapport dat wordt vermeld in het e-mailbericht?

Als u op de koppeling klikt, wordt u omgeleid naar de rapportpagina binnen de Azure klassieke portal. Toegang tot het rapport, moet u beide:

- Een beheerder of co-beheerder van uw Azure-abonnement

- Een globale beheerder in de map, en een Active Directory-Premium-licentie toegewezen. Zie [Azure Active Directory-edities](active-directory-editions.md)voor meer informatie.

## <a name="can-i-turn-off-these-emails"></a>Kan ik deze e-mailberichten uitschakelen?

Ja, om de meldingen die betrekking hebben op afwijkende aanmeldingen binnen de Azure klassieke portal uitschakelt, klikt u op **configureren**en selecteer vervolgens **Uitgeschakelde** onder de sectie **meldingen** .

## <a name="whats-next"></a>Volgende stappen
- Meer wilt weten over welke rapporten beveiliging, controle en activiteit beschikbaar zijn? [Azure AD-beveiliging, controle, en rapporten van activiteiten](active-directory-view-access-usage-reports.md) uitchecken
- [Aan de slag met Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Bedrijf huisstijl aan uw zich hebt aangemeld en een Access-deelvenster pagina's toevoegen](active-directory-add-company-branding.md)
