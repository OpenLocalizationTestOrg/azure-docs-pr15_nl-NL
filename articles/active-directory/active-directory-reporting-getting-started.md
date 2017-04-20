<properties
   pageTitle="Azure Active Directory-rapportage: Aan de slag | Microsoft Azure"
   description="Overzicht van de verschillende beschikbare rapporten in Azure Active Directory-rapportage"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>Aan de slag met Azure Active Directory-rapportage

## <a name="what-it-is"></a>Wat is

Azure Active Directory (Azure AD) bevat beveiliging, activiteit en controlerapporten voor uw adreslijst. Hier volgt een lijst van de rapporten die zijn opgenomen:

### <a name="security-reports"></a>Beveiligingsrapporten

- Aanmeldingen van onbekende bronnen
- Aanmeldingen na meerdere fouten
- Aanmeldingen uit verschillende gebieden
- Aanmeldingen van IP-adressen met verdachte activiteit
- Onregelmatige aanmeldingsproblemen activiteit
- Aanmeldingen vanaf mogelijk geïnfecteerd apparaten
- Gebruikers met een afwijkende aanmeldingsproblemen activiteit

### <a name="activity-reports"></a>Rapporten van activiteiten

- Gebruik van toepassingen: samenvatting
- Gebruik van toepassingen: gedetailleerde
- Dashboard van toepassing
- Account inrichting van fouten
- Individuele Gebruikersapparaten
- Individuele gebruiker activiteit
- Groepen activiteitenrapport
- Wachtwoord opnieuw instellen registratie activiteitenrapport
- Activiteit voor wachtwoordherstel

### <a name="audit-reports"></a>Controlerapporten

- Directory controlerapport

> [AZURE.TIP] Voor meer documentatie op Azure AD-rapportage, raadpleegt u [uw rapporten toegang en gebruik weergeven](active-directory-view-access-usage-reports.md).



## <a name="how-it-works"></a>Werkwijze


### <a name="reporting-pipeline"></a>Rapportage verkooppijplijn

De rapportage pijplijn bestaat uit drie basisstappen. Telkens wanneer een gebruiker zich aanmeldt, of een verificatie wordt uitgevoerd, gebeurt het volgende:

- Eerst de gebruiker wordt geverifieerd (geslaagd of mislukt), en het resultaat is opgeslagen in de Azure Active Directory-service-databases.
- Met regelmatige tussenpozen, alle recente Aanmeldingsadres ins worden verwerkt. In dit stadium onze beveiligings- en afwijkende activiteit algoritmen zoekt alle recente Aanmeldingsadres ins voor verdachte activiteiten.
- Na verwerking, zijn de rapporten geschreven, opgeslagen in de cache en served in de portal van Azure klassieke.

### <a name="report-generation-times"></a>Generatie tijden rapporteren

Vanwege de grote hoeveelheid authenticatie en meld u invoegtoepassingen voor verwerkt door het platform Azure AD, zijn de meest recente aanmeldingen verwerkt, gemiddeld een uur oud. In sommige gevallen kan het verwerken van de meest recente aanmeldingen maximaal acht uur duren.

U vindt de meest recente verwerkte aanmeldingsproblemen aan de hand van de help-informatie aan het begin van elk rapport.

![Help-informatie aan het begin van elk rapport](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Voor meer documentatie op Azure AD-rapportage, raadpleegt u [uw rapporten toegang en gebruik weergeven](active-directory-view-access-usage-reports.md).



## <a name="getting-started"></a>Aan de slag


### <a name="sign-into-the-azure-classic-portal"></a>Meld u aan bij de portal van Azure klassieke

Eerst moet u aan te melden bij de [portal van Azure klassieke](https://manage.windowsazure.com) als een globale of de beheerder van de naleving. U moet ook een abonnement op Azure-service-beheerder of een collega beheerder of gebruikmaken van de 'toegang tot Azure AD"Azure-abonnement.

### <a name="navigate-to-reports"></a>Blader naar de rapporten

Ga naar het tabblad rapporten boven aan het telefoonboek van uw om rapporten te bekijken.

Als dit de eerste keer is de rapporten weergeven, moet u een dialoogvenster accepteren voordat u kunt de rapporten weergeven. Dit is om ervoor te zorgen dat dit is geschikt voor beheerders in uw organisatie deze gegevens, die kan worden geacht privégegevens in sommige landen moeten worden weergegeven.

![Het dialoogvenster](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Elk rapport verkennen

Navigeer in elk rapport om te zien van de gegevens worden verzameld en de aanmeldingen verwerkt. U vindt een [lijst met alle rapporten hier](active-directory-reporting-guide.md).

![Alle rapporten](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>De rapporten als CSV downloaden

Elk rapport kan worden gedownload als een CSV-bestand (door komma's gescheiden waarden). U kunt deze bestanden in Excel, PowerBI of derden analyse programma's voor verdere uw gegevens analyseren.

Als u wilt downloaden van een rapport als een CSV, Ga naar het rapport en klik onderaan op "Download".

![Knop downloaden](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Voor meer documentatie op Azure AD-rapportage, raadpleegt u [uw rapporten toegang en gebruik weergeven](active-directory-view-access-usage-reports.md).





## <a name="next-steps"></a>Volgende stappen

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Waarschuwingen voor afwijkende sign in activiteit aanpassen

Ga naar het tabblad 'Configureren' van de map.

Ga naar het gedeelte "Meldingen".

In- of uitschakelen van de sectie "E-mailmeldingen van afwijkende aanmeldingen".

![De sectie meldingen](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integreren met de Azure AD rapportage-API

Zie [aan de slag met de API rapportage](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Meervoudige verificatie deelnemen voor gebruikers

Selecteer een gebruiker in een rapport.

Klik op de knop 'MFA inschakelen' onderaan in het scherm.

![De knop meervoudige verificatie onderaan in het scherm](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Voor meer documentatie op Azure AD-rapportage, raadpleegt u [uw rapporten toegang en gebruik weergeven](active-directory-view-access-usage-reports.md).




## <a name="learn-more"></a>Meer informatie


### <a name="audit-events"></a>Gebeurtenissen controleren

Meer informatie over welke gebeurtenissen worden gecontroleerd in de map in [Azure Active Directory rapportage gebeurtenissen controleren](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>API-integratie

Zie [aan de slag met de API rapportage](active-directory-reporting-api-getting-started.md) en de [API-documentatie](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Communiceren

E-mail [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) voor feedback of help of vragen hebt.

> [AZURE.TIP] Voor meer documentatie op Azure AD-rapportage, raadpleegt u [uw rapporten toegang en gebruik weergeven](active-directory-view-access-usage-reports.md).
