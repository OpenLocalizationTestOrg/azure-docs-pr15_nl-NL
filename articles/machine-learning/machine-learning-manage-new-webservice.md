<properties
    pageTitle="Een webservice met behulp van de portal Azure Machine Learning Web Serivces beheren | Microsoft Azure"
    description="De toegang tot Azure Machine Learning-werkruimten beheren en te implementeren en te beheren ML API-webservices"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Een webservice met behulp van de Azure Machine Learning Web Services-portal beheren

U kunt uw Machine Learning nieuwe en klassieke webservices met behulp van de portal van Microsoft Azure Machine Learning-webservices beheren. Aangezien klassieke WCF-services en nieuwe webservices zijn gebaseerd op verschillende onderliggende technologieën, hebt u iets anders beheermogelijkheden voor elk van deze.

In de portal Machine Learning-webservices, kunt u het volgende doen:

- Controleren hoe de webservice wordt gebruikt.
- De beschrijving van de configureren, het bijwerken van de toetsen service (alleen nieuw) voor het web, uw opslagruimte account key (alleen nieuw), logboekregistratie inschakelen, bijwerken en in- of uitschakelen voorbeeldgegevens.
- Verwijder de webservice.
- Maken, verwijderen of update facturering abonnementen (alleen nieuw).
- Toevoegen en verwijderen van de eindpunten (alleen klassiek)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Nieuwe Web services beheren

Voor het beheren van uw nieuwe Web services:

1.  Meld u aan bij de [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) -portal met uw Microsoft Azure-account: het account dat is gekoppeld aan het abonnement dat Azure gebruiken.
2.  Klik op het menu op **Webservices**.

Er verschijnt een lijst van geïmplementeerd webservices voor uw abonnement. 

Als u wilt beheren een webservice, klikt u op webservices. Vanaf de pagina Web Services kunt u het volgende doen:

- Klik op de webservice als u wilt beheren.
- Klik op het facturering Plan voor de webservice deze bij te werken.
- Een webservice verwijderen.
- Kopieer een webservice en het dashboard implementeren naar een andere regio.

Als u een webservice klikt, wordt de pagina met webonderdelen service Quickstart wordt geopend. De pagina met webonderdelen service Quickstart heeft twee menuopties waarmee u kunt uw webservice beheren:

- **DASHBOARD** - kunt u gebruik van de Web-service wilt bekijken.
- **Configureren** - kunt u beschrijvende tekst toevoegen, bijwerken van de toets voor de opslag-account dat is gekoppeld aan de webservice, en in- of uitschakelen voorbeeldgegevens.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Cmdlets voor controle hoe de webservice wordt gebruikt ###

Klik op het tabblad **DASHBOARD** .

Vanuit het dashboard, kunt u de algehele gebruik van uw webservice weergeven over een tijdsperiode. U kunt de periode om weer te geven in de periode vervolgkeuzelijst in de rechterbovenhoek van de grafieken gebruik selecteren. Het dashboard bevat de volgende informatie:

- Een grafiek stap van het aantal aanvragen weergegeven **Aanvragen verloop van tijd** over de selecteerde tijdsperiode. Hiermee kunt doen als er pieken in gebruik identificeren.
- **Aanvraag en respons aanvragen** geeft het totale aantal aanvraag en respons oproepen die de service heeft ontvangen via de selecteerde tijdsperiode en hoeveel is mislukt.
- **Tijd voor gemiddelde berekenen van aanvraag en respons** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen verzoeken weergegeven.
- **Batch aanvragen** geeft het totale aantal Batch aanvragen voor de service heeft ontvangen over de selecteerde tijdsperiode en hoeveel is mislukt.
- **Latentie van gemiddeld taak** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen verzoeken weergegeven.
- **Fouten** wordt het totale aantal fouten die zijn opgetreden oproepen naar de webservice weergegeven.
- **Services kosten** worden de kosten voor de facturering-abonnement dat is gekoppeld aan de service weergegeven.

### <a name="configuring-the-web-service"></a>De webservice configureren ###

Klik op de menuoptie **configureren** .

U kunt de volgende eigenschappen bijwerken:

* **Beschrijving** , kunt u een beschrijving voor de webservice in te voeren.
* **Titel** kunt u een titel voor de webservice invoeren
* **Toetsen** kunt u uw primaire en secundaire API sleutels draaien.
* **Opslag accountsleutel** kunt u de toets voor de opslag-account dat is gekoppeld aan de Web-servicewijzigingen bijwerken. 
* **Inschakelen voorbeeldgegevens** kunt u voorbeeldgegevens die u gebruiken kunt om te testen van de service verzoek-antwoord geven. Als u de webservice in Machine Learning Studio hebt gemaakt, is de voorbeeldgegevens opgehaald uit de gegevens uw gebruikt voor uw model trainen. Als u de service via een programma hebt gemaakt, worden de gegevens worden opgehaald uit de voorbeeldgegevens die u hebt opgegeven als onderdeel van de JSON-pakket.

### <a name="managing-billing-plans"></a>Facturering abonnementen beheren ###

Klik op de menuoptie **abonnementen** van de pagina met webonderdelen services Quickstart. U kunt ook klikken op het abonnement dat is gekoppeld aan specifieke webservice voor het beheren van dat plan.

* **Nieuw** kunt u een nieuw abonnement maken.
* **Exemplaar toevoegen/verwijderen-abonnement** kunt u 'schaal "een bestaand abonnement capaciteit toe te voegen.
* **Upgrade/DownGrade** kunt u ' om uit te breiden"een bestaand abonnement capaciteit toe te voegen.
* **Verwijderen** , kunt u een abonnement verwijderen.

Klik op een abonnement op een dashboard bekijken. Het dashboard kunt u een momentopname of abonnement gebruik over een geselecteerde tijdsperiode. Klik op de vervolgkeuzelijst **periode** in de rechterbovenhoek van dashboard de periode om weer te geven. 

Het dashboard van abonnement biedt de volgende informatie:

* **Beschrijving van het abonnement** wordt informatie over de kosten en capaciteit die is gekoppeld aan het abonnement.
* **Abonnement gebruik** geeft het getal van transacties en berekeningscluster uren die zijn geheven ten opzichte van het abonnement.
* **Webservices** geeft het getal van webservices dat dit abonnement gebruikt.
* **Begin door aanroepen naar een webservice** geeft de hoogste vier-webservices die zijn kunt bellen die worden berekend met de planning.
* **Top-webservices per uur berekenen** geeft de hoogste vier-webservices dat berekeningscluster resources die in rekening worden gebracht ten opzichte van het abonnement gebruikt.

## <a name="manage-classic-web-services"></a>Klassieke webservices beheren

> [AZURE.NOTE] De procedures in dit gedeelte zijn relevant voor het beheren van webservices klassieke via de portal Azure Machine Learning-webservices. Zie voor informatie over het beheren van klassieke webservices via de Machine Learning-Studio en de portal van Azure klassieke, [beheren een Azure Machine Learning-werkruimte](machine-learning-manage-workspace.md).

Voor het beheren van uw klassieke Web services:

1.  Meld u aan bij de [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) -portal met uw Microsoft Azure-account: het account dat is gekoppeld aan het abonnement dat Azure gebruiken.
2.  Klik op het menu op **Klassieke webservices**.

Als u wilt een klassieke webservice beheren, klikt u op **Klassieke webservices**. Vanaf de pagina klassieke webservices kunt u het volgende doen:

- Klik op de webservice om weer te geven van de bijbehorende eindpunten.
- Een webservice verwijderen.

Wanneer u een klassieke webservice beheert, beheren u elk van de eindpunten afzonderlijk. Als u een webservice in de Web Services-pagina klikt, wordt de lijst met eindpunten die is gekoppeld aan de service wordt geopend. 

U kunt op de pagina klassieke webservice eindpunt toevoegen en verwijderen van de eindpunten van de service. Zie voor meer informatie over het toevoegen van eindpunten [Eindpunten maken](machine-learning-create-endpoint.md).

Klik op een van de eindpunten naar de pagina met webonderdelen service Quickstart openen. Klik op de pagina Quickstart zijn er twee menuopties waarmee u kunt uw webservice beheren:

- **DASHBOARD** - kunt u gebruik van de Web-service wilt bekijken.
- **Configureren** - kunt u beschrijvende tekst toevoegen, Schakel logboekregistratie in- of uitschakelen, update de toets voor de opslag-account dat is gekoppeld aan de webservice, en in- en uitschakelen van voorbeeldgegevens.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Cmdlets voor controle hoe de webservice wordt gebruikt ###

Klik op het tabblad **DASHBOARD** .

Vanuit het dashboard, kunt u de algehele gebruik van uw webservice weergeven over een tijdsperiode. U kunt de periode om weer te geven in de periode vervolgkeuzelijst in de rechterbovenhoek van de grafieken gebruik selecteren. Het dashboard bevat de volgende informatie:

- Een grafiek stap van het aantal aanvragen weergegeven **Aanvragen verloop van tijd** over de selecteerde tijdsperiode. Hiermee kunt doen als er pieken in gebruik identificeren.
- **Aanvraag en respons aanvragen** geeft het totale aantal aanvraag en respons oproepen die de service heeft ontvangen via de selecteerde tijdsperiode en hoeveel is mislukt.
- **Tijd voor gemiddelde berekenen van aanvraag en respons** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen verzoeken weergegeven.
- **Batch aanvragen** geeft het totale aantal Batch aanvragen voor de service heeft ontvangen over de selecteerde tijdsperiode en hoeveel is mislukt.
- **Latentie van gemiddeld taak** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen verzoeken weergegeven.
- **Fouten** wordt het totale aantal fouten die zijn opgetreden oproepen naar de webservice weergegeven.
- **Services kosten** worden de kosten voor de facturering-abonnement dat is gekoppeld aan de service weergegeven.

### <a name="configuring-the-web-service"></a>De webservice configureren ###

Klik op de menuoptie **configureren** .

U kunt de volgende eigenschappen bijwerken:

* **Beschrijving** , kunt u een beschrijving voor de webservice in te voeren. Beschrijving is een vereist veld.
* **Logboekregistratie** , kunt u in- of uitschakelen van fout bij het aanmelden op het eindpunt. Voor meer informatie over logboekregistratie, Zie inschakelen [logboekregistratie voor Machine Learning-webservices](machine-learning-web-services-logging.md).
* **Inschakelen voorbeeldgegevens** kunt u voorbeeldgegevens die u gebruiken kunt om te testen van de service verzoek-antwoord geven. Als u de webservice in Machine Learning Studio hebt gemaakt, is de voorbeeldgegevens opgehaald uit de gegevens uw gebruikt voor uw model trainen. Als u de service via een programma hebt gemaakt, worden de gegevens worden opgehaald uit de voorbeeldgegevens die u hebt opgegeven als onderdeel van de JSON-pakket.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Verlenen of op te schorten toegang tot webservices voor gebruikers in de portal

De portal van Azure klassieke gebruikt, kunt u toestaan of weigeren van toegang aan specifieke gebruikers.

### <a name="access-for-users-of-new-web-services"></a>Toegang voor gebruikers van nieuwe webservices

Als u wilt dat andere gebruikers werkt met uw webservices in de portal Azure Machine Learning-webservices, moet u deze als co-beheerders voor uw abonnement op Azure toevoegen.

Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/) met uw Microsoft Azure-account: het account dat is gekoppeld aan het abonnement dat Azure gebruiken.

1. In het navigatiedeelvenster, klikt u op **Instellingen**, klik op **beheerders**.
2. Onderaan in het venster, klikt u op **toevoegen**. 
3. Typ het e-mailadres van de persoon die u wilt toevoegen als collega beheerder en selecteer vervolgens het abonnement waaraan u wilt dat de collega beheerder voor toegang tot in het dialoogvenster ADD A CO-ADMINISTRATOR.
4. Klik op **Opslaan**.

### <a name="access-for-users-of-classic-web-services"></a>Toegang voor gebruikers van klassieke webservices

Voor het beheren van een werkruimte:

Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/) met uw Microsoft Azure-account: het account dat is gekoppeld aan het abonnement dat Azure gebruiken.

1. Klik in het deelvenster Microsoft Azure services op **MACHINE LEARNING**.
1. Klik op de werkruimte die u wilt beheren.
1. Klik op het tabblad **configureren** .

U kunt toegang tot de Machine Learning-werkruimte op het tabblad configuratie uitstellen door te klikken op **weigeren**. Gebruikers wordt niet langer de werkruimte wilt openen in Machine Learning Studio. Als u wilt herstellen access, klikt u op **toestaan**.

Aan specifieke gebruikers:

Als u wilt beheren extra accounts die toegang tot de werkruimte in Machine Learning Studio hebben, klikt u op **aanmelden bij ML Studio** op het tabblad **DASHBOARD** . Hiermee opent u de werkruimte in Machine Learning Studio. U kunt op het tabblad **Instellingen** en klik vervolgens op **gebruikers**. U kunt klikken op **Meer gebruikers UITNODIGEN** gebruikers toegang geven tot de werkruimte, of Selecteer een gebruiker en klik op **verwijderen**.

> [AZURE.NOTE] De koppeling **aanmelden bij ML Studio** opent Machine Learning Studio met het Microsoft-Account dat u momenteel bent aangemeld bij. Het Microsoft-Account waarmee u aanmelden bij de portal voor Azure klassieke bij het maken van een werkruimte is niet automatisch gemachtigd die werkruimte wilt openen. Als u wilt een werkruimte opent, u moet zijn aangemeld bij de Microsoft-Account die is gedefinieerd als de eigenaar van de werkruimte, anders moet u een uitnodiging hebben ontvangen van de eigenaar van de deel te nemen aan de werkruimte.
