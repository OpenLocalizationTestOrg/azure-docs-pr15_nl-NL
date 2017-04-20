<properties
   pageTitle="Azure Active Directory-rapportage - preview | Microsoft Azure"
   description="Overzicht van de verschillende beschikbare rapporten voor Azure Active Directory preview"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory-rapportage - voorbeeld

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-reporting-azure-portal.md)
- [Azure klassieke portal](active-directory-reporting-guide.md)

*Deze documentatie maakt deel uit van de [Azure Active Directory rapportage Guide](active-directory-reporting-guide.md).*

Met rapporten in de voorbeeldversie Azure Active Directory, krijgt u alle gegevens die u nodig hebt om te bepalen hoe uw omgeving doet. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md)

Er zijn twee hoofdgebieden van rapporten:

- **Aanmeldingsproblemen activiteiten** – informatie over het gebruik van beheerde toepassingen en gebruiker aanmeldingsproblemen activiteiten

- **Controlelogboeken bijhouden** - systeem activiteit informatie over gebruikers en groepsbeheer, uw beheerde toepassingen en directory activiteiten

Afhankelijk van het bereik van de gegevens die u zoekt, kunt u door te klikken op **gebruikers en groepen** of **bedrijfstoepassingen** in de lijst met services in de [portal van Azure](https://portal.azure.com)deze rapporten openen.

## <a name="sign-in-activities"></a>Aanmeldingsproblemen activiteiten

### <a name="user-sign-in-activities"></a>Activiteiten van aanmeldingsproblemen gebruikers

Met de informatie die is verstrekt door de gebruiker aanmeldingsproblemen rapport, antwoorden u op vragen, zoals:

- Wat is het patroon aanmelden van een gebruiker?
- Hoeveel gebruikers hebben gebruikers aangemeld gedurende een week?
- Wat is de status van deze aanmeldingen?

Uw ingangspunt naar deze gegevens is de gebruiker aanmeldingsproblemen grafiek in de sectie **Overzicht** onder **gebruikers en groepen**.

 ![Rapportage] (./media/active-directory-reporting-azure-portal/05.png "Rapportage")

De gebruiker aanmeldingsproblemen grafiek worden weergegeven wekelijkse aggregaties van Aanmeldingsfout ins voor alle gebruikers in een bepaalde periode. De standaardindeling voor de periode is 30 dagen.

![Rapportage] (./media/active-directory-reporting-azure-portal/02.png "Rapportage")

Als u op een dag in de grafiek aanmelden klikt, krijgt u een gedetailleerd overzicht van de activiteiten aanmelden.

![Rapportage] (./media/active-directory-reporting-azure-portal/03.png "Rapportage")

Elke rij in de lijst aanmelden activiteiten kunt u de gedetailleerde informatie over de geselecteerde aanmeldingsproblemen, zoals:

- Wie heeft aangemeld?

- Wat is de gerelateerde UPN?

- Welke toepassing is het doel van het aanmelden?

- Wat is het IP-adres van het aanmelden?

- Wat is de status van het aanmelden?

### <a name="usage-of-managed-applications"></a>Gebruik van beheerde toepassingen

Met een toepassing centraal weergave van uw gegevens aanmelden, kunt u, zoals vragen beantwoorden:

- Wie is mijn toepassingen via?

- Wat zijn de bovenste 3 toepassingen in uw organisatie?

- Ik hebt onlangs een toepassing geïmplementeerd. Hoe doet dit?


Uw ingangspunt naar deze gegevens is de bovenste 3 toepassingen in uw organisatie in de laatste 30 dagen-rapport in de sectie **Overzicht** onder **bedrijfstoepassingen**.

 ![Rapportage] (./media/active-directory-reporting-azure-portal/06.png "Rapportage")


De app gebruik graph wekelijkse aggregaties van aanmelden ins voor uw toepassingen top 3 een bepaalde periode. De standaardindeling voor de periode is 30 dagen.

![Rapportage] (./media/active-directory-reporting-azure-portal/78.png "Rapportage")

Als u wilt, kunt u de focus instellen op een specifieke toepassing.

![Rapportage] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Rapportage")


Als u op een dag in de app gebruik grafiek klikt, krijgt u een gedetailleerd overzicht van de activiteiten aanmelden.


![Rapportage] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Rapportage")



De optie **aanmeldingen** biedt u een volledig overzicht van alle gebeurtenissen aanmelden tot uw toepassingen.

![Rapportage] (./media/active-directory-reporting-azure-portal/85.png "Rapportage")

Met behulp van de kolomkiezer, kunt u de gegevensvelden die u wilt weergeven.

![Rapportage] (./media/active-directory-reporting-azure-portal/column_chooser.png "Rapportage")



### <a name="filtering-sign-ins"></a>Aanmeldingen filteren

U kunt aanmeldingen filteren op een tijdsinterval om de hoeveelheid weergegeven gegevens te beperken.

![Rapportage] (./media/active-directory-reporting-azure-portal/927.png "Rapportage")


Er is een andere methode voor het posten van de activiteiten aanmeldingsproblemen filteren om te zoeken naar specifieke vermeldingen.
De zoekmethode kunt u uw Aanmeldingsadres invoegtoepassingen voor rond specifieke **gebruikers**, **groepen** of **toepassingen**bereik.


![Rapportage] (./media/active-directory-reporting-azure-portal/84.png "Rapportage")

## <a name="audit-logs"></a>Controlelogboeken bijhouden

De controle Logboeken in Azure Active Directory bieden records van systeemactiviteiten voor naleving.

Er zijn drie belangrijkste categorieën voor gerelateerde activiteiten in de portal van Azure controle:

- Gebruikers en groepen   

- Toepassingen

- Directory   


Raadpleeg de [lijst met gebeurtenissen controleren-rapport](active-directory-reporting-audit-events.md#list-of-audit-report-events)voor een volledige lijst van controle rapport activiteiten.


Uw ingangspunt alle controle gegevens is **controlelogboeken bijhouden** in de sectie **activiteit** van **Azure Active Directory**.


![Controle] (./media/active-directory-reporting-azure-portal/61.png "Controle")


Een controlelogboek heeft een lijstweergave waarin de betrokkenen (die), de activiteiten (wat) en de doelen.


![Controle] (./media/active-directory-reporting-azure-portal/345.png "Controle")


Door te klikken op een item in de lijstweergave, kunt u meer informatie erover te verkrijgen.

![Controle] (./media/active-directory-reporting-azure-portal/873.png "Controle")




### <a name="users-and-groups-audit-logs"></a>Gebruikers en groepen controlelogboeken bijhouden


Met de gebruikers en groepen gebaseerde controlerapporten, vindt u antwoorden op vragen, zoals:

- Wat typen updates zijn toegepast van de gebruikers?

- Hoeveel gebruikers zijn gewijzigd?

- Hoeveel wachtwoorden zijn gewijzigd?

- Wat is een beheerder uitgevoerd in een map?

- Wat zijn de groepen die zijn toegevoegd?

- Zijn er groepen met lidmaatschapswijzigingen?

- De eigenaren van groep zijn gewijzigd?

- Welke licenties aan een groep of een gebruiker zijn toegewezen?


Als u alleen controleren van de gegevens die zijn gerelateerd aan gebruikers en groepen bekijken wilt, kunt u een gefilterde weergave onder **controlelogboeken** vinden in de sectie **activiteit** van **gebruikers en groepen**.


![Controle] (./media/active-directory-reporting-azure-portal/93.png "Controle")


### <a name="application-audit-logs"></a>Toepassing controlelogboeken bijhouden

Met de toepassing gebaseerde controle rapporten, vindt u antwoorden op vragen zoals:

- Wat zijn de toepassingen die zijn toegevoegd of bijgewerkt?

- Wat zijn de toepassingen die zijn verwijderd?

- Is een service-principe voor een toepassing gewijzigd?

- De namen van toepassingen zijn gewijzigd?

- Wie toestemming hebt gegeven aan een toepassing?


Als u alleen controleren van de gegevens die zijn gerelateerd aan toepassingen bekijken wilt, kunt u een gefilterde weergave onder **controlelogboeken** vinden in de sectie **activiteit** van **zakelijke toepassingen**.


![Controle] (./media/active-directory-reporting-azure-portal/134.png "Controle")


### <a name="filtering-audit-logs"></a>Filteren controlelogboeken bijhouden

U kunt een controlerapport filteren op een tijdsinterval om de hoeveelheid weergegeven gegevens te beperken.

![Controle] (./media/active-directory-reporting-azure-portal/324.png "Controle")

Er is een andere methode voor het posten van een controlelogboek filteren om te zoeken naar specifieke vermeldingen.

![Controle] (./media/active-directory-reporting-azure-portal/237.png "Controle")

## <a name="next-steps"></a>Volgende stappen

Zie de [Azure Active Directory rapportage-handleiding](active-directory-reporting-guide.md).
