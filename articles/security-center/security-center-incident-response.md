<properties
   pageTitle="Met behulp van Azure Beveiligingscentrum voor een incident antwoord wordt verzonden | Microsoft Azure"
   description="In dit document wordt uitgelegd hoe Azure Beveiligingscentrum gebruiken voor een mogelijkheid incident antwoord."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Met behulp van Azure Beveiligingscentrum voor een incident antwoord wordt verzonden
Veel organisaties informatie over het reageren op beveiligingsincidenten alleen nadat ze zijn een aanval. Als u wilt beperken kosten en schade, is het belangrijk dat een incident antwoord wordt verzonden in plaats van plan bent voordat een aanval plaatsvindt. U kunt Azure Beveiligingscentrum gebruiken in verschillende fasen van een incident antwoord wordt verzonden.

## <a name="incident-response-planning"></a>Incident antwoord plannen

Een effectieve-abonnement, is afhankelijk van drie core mogelijkheden: niet alleen beveiligen, detecteren in en reageren op threats. Bescherming tegen is over het voorkomen van incidenten, detectie is over het identificeren van threats vroeg en antwoord is over het verwijderen van de hacker en systemen om te beperken van de effecten van een inbreuk op herstellen.

In dit artikel worden de beveiliging incident antwoord fasen van het artikel [Microsoft Azure beveiliging antwoord in de Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) gebruikt, zoals in het volgende diagram:

![Levenscyclus van incident antwoord](./media/security-center-incident-response/security-center-incident-response-fig1.png)

U kunt Beveiligingscentrum gebruiken tijdens de analyse, schatten en Diagnose fasen. Hier ziet voorbeelden van hoe Beveiligingscentrum handig tijdens fase aanvankelijke incident antwoord is:

- **Bepalen**: Bekijk de eerste vermelding van een gebeurtenis onderzoek.
    - Voorbeeld: controleren op de eerste verificatie dat een hoge prioriteit beveiligingswaarschuwing in het dashboard Beveiligingscentrum is verheven.
- **Evaluatie**: de eerste beoordeling voor meer informatie over het verdachte activiteiten uitvoeren.
    - Voorbeeld: meer informatie over de Beveiligingsmelding van verkrijgen.
- **Diagnose**: een technische onderzoek leiden en insluiting, risicobeperking en tijdelijke oplossing strategieën identificeren.
    - Voorbeeld: wordt de remediation stappen die door Beveiligingscentrum worden beschreven in de Beveiligingsmelding van dat bepaalde.

Het scenario die volgt ziet u hoe om te profiteren van Beveiligingscentrum tijdens de analyse, schatten en Diagnose/reageren fasen van een beveiligingsincident. In Beveiligingscentrum is een [beveiligingsincident](security-center-incident.md) een samenvoeging van alle waarschuwingen voor een resource die met patronen [ketting beëindigen overeenkomen](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Incidenten worden weergegeven in de tegel [beveiligingsmeldingen](security-center-managing-and-responding-alerts.md) en blade. Een incident klikt, worden de lijst met verwante waarschuwingen, waarmee u meer informatie over elk exemplaar te verkrijgen. Beveiligingscentrum bevat ook zelfstandige beveiligingsmeldingen die kunnen ook worden gebruikt om te achterhalen een verdachte activiteiten.

## <a name="scenario"></a>Scenario

Contoso gemigreerd aantal hun lokale bronnen onlangs naar Azure, inclusief enkele VM gebaseerde LOB-werkbelasting en SQL-databases. Van Contoso Core Computer beveiliging Incident antwoord Team (CSIRT) heeft momenteel een probleem wordt onderzocht beveiligingskwesties vanwege beveiliging intelligence niet wordt geïntegreerd met hun huidige incident antwoord's. Het gebrek aan integratie maakt u kennis met een probleem in de analyse-fase (te veel onrechte), alsmede tijdens de evaluatie en Diagnose fasen. Als onderdeel van deze migratie, die ze willen deelnemen voor Beveiligingscentrum te helpen dit probleem.

De eerste fase van deze migratie is voltooid nadat ze onboarded alle resources en alle aanbevelingen voor de beveiliging van Beveiligingscentrum gericht. Contoso CSIRT moet het centrale punt voor de verwerking van computer-incidenten. Het team bestaat uit een groep personen met verantwoordelijkheden ter omgaan met een beveiligingsincident. De teamleden hebben rechten om ervoor te zorgen dat er geen gebied van het antwoord resteert duidelijk omschreven ongedekte.

Voor dit scenario gaan we focus op de rollen van de volgende personas die deel uitmaken van Contoso CSIRT:

![Levenscyclus van incident antwoord](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Jacolien bevindt zich op beveiligingsbewerkingen. Onder andere haar verantwoordelijkheden:

- Cmdlets voor controle en beveiligingsrisico's servicepakket beantwoorden.
- Escaleren naar de cloud werkbelasting eigenaar of de beveiliging analisten naar wens.

SAM is een beveiliging analisten en zijn verantwoordelijkheden opnemen:

- Wordt onderzocht aanvallen.
- Knop meldingen.
- Werken met werkbelasting eigenaars die u wilt bepalen en beperkingen toe te passen.

Zoals u ziet, Jacolien en Sam hebt verschillende verantwoordelijkheden en ze moeten samenwerken om Beveiligingscentrum informatie te delen.

## <a name="recommended-solution"></a>Aanbevolen oplossing

Aangezien Jacolien en Sam hebt verschillende rollen, voortaan ze verschillende gebieden van Beveiligingscentrum gebruikt om relevante informatie voor hun dagelijkse werkzaamheden te verkrijgen. Jacolien wordt **beveiligingsmeldingen** gebruiken als onderdeel van haar dagelijkse controle.

![Beveiligingsmeldingen](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Jacolien wordt gebruikt in beveiligingsmeldingen tijdens de analyse en schatten fasen. Nadat Jacolien is het eerste onderzoek is voltooid, kan ze het probleem naar Sam omzetten als extra onderzoek vereist is. Op dit moment Sam gebruikt de gegevens die is verstrekt door Beveiligingscentrum, soms in combinatie met andere gegevensbronnen, naar de fase Diagnose verplaatsen.


## <a name="how-to-implement-this-solution"></a>Hoe u deze oplossing implementeert

Als u wilt zien hoe u Azure Beveiligingscentrum in een scenario incident antwoord wilt gebruiken, we Jacolien van stappen in de analyse en schatten fasen en raadpleegt u wat Sam betekent diagnose stellen bij het probleem.

### <a name="detect-and-assess-incident-response-stages"></a>Detecteren in en incident antwoord fasen beoordelen

Jacolien aangemeld bij de portal van Azure en werkt in de Beveiligingscentrum-console. Als onderdeel van haar dagelijks monitoring activiteiten, gestart zij reviseren van hoge prioriteit beveiligingsmeldingen door de volgende stappen uit te voeren:

1. Klik op de tegel **beveiligingsmeldingen** en toegang tot het blad **beveiligingsmeldingen** .
    ![Waarschuwing blade beveiliging](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Voor dit scenario dient Jacolien een beoordeling uitvoeren op de melding voor een activiteit schadelijke SQL, zoals gezien in de bovenstaande afbeelding.
2. Klik op de melding voor een **activiteit schadelijke SQL** en controleer de aangevallen resources in het blad **schadelijke SQL activiteit** :  ![Incident details](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    In dit blade, Jacolien notities kunt maken met betrekking tot de aangevallen bronnen, hoe vaak deze aanval is er gebeurd en wanneer deze is gedetecteerd.
3. Klik op de **resource aanval** voor meer informatie over deze aanval.

Lees de beschrijving en Jacolien ervan overtuigd dat dit niet een fout-positief is en dat zij deze zaak naar Sam moet omzetten.

### <a name="diagnose-incident-response-stage"></a>Een diagnose stellen bij incident antwoord fase

SAM ontvangt van de hoofdletters/kleine letters van Jacolien en wordt begonnen reviseren van de remediation stappen die Beveiligingscentrum voorgesteld.

![Levenscyclus van incident antwoord](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Aanvullende informatie

Het team incident antwoord kunt ook profiteren van de [Beveiliging Center Power BI](security-center-powerbi.md) -mogelijkheden om verschillende soorten rapporten weer te geven. Deze rapporten kunnen helpen bij verdere onderzoek kunt visualiseren, analyseren en aanbevelingen en beveiligingsmeldingen filteren. Voor bedrijven die hun beveiligingsinformatie en de gebeurtenis beheeroplossing (SIEM) tijdens het onderzoek gebruiken, kunnen ze ook [Beveiligingscentrum met hun oplossing integreren](security-center-integrating-alerts-with-log-integration.md). U kunt ook Azure controlelogboeken bijhouden en virtuele machine (VM) beveiligingsgebeurtenissen integreren met behulp van het [hulpprogramma Azure log](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/). Als u wilt onderzoeken een aanval, kunt u deze gegevens gebruiken in combinatie met de informatie die Beveiligingscentrum vindt.


## <a name="conclusion"></a>Sluiten

Een team samenstellen voordat een incident voordoet is belangrijk dat uw organisatie en hoe incidenten worden afgehandeld positief worden beïnvloed. De juiste hulpmiddelen om te controleren bronnen kunt dit team stappen ondernemen om nauwkeurig te verhelpen incidenten. Beveiligingscentrum [detectiemogelijkheden](security-center-detection-capabilities.md) kunt helpen IT kunt u snel reageren op beveiligingsincidenten en beveiligingsproblemen verhelpen.
