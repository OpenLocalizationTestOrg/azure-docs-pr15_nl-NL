<properties
    pageTitle="Een Machine Learning-werkruimte beheren | Microsoft Azure"
    description="De toegang tot Azure Machine Learning-werkruimten beheren en te implementeren en te beheren ML API-webservices"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Een werkruimte Azure Machine Learning beheren

>[AZURE.NOTE] De procedures in dit artikel zijn relevant voor Azure Machine Learning klassieke-webservices. Zie voor informatie over het beheren van webservices in de portal Machine Learning-webservices, [beheren een webservice met behulp van de portal Azure Machine Learning-webservices](machine-learning-manage-new-webservice.md).

Met de portal van Azure klassieke, kunt u uw werkruimten Machine Learning om te beheren:

- Controleren hoe de werkruimte wordt gebruikt
- De werkruimte wilt toestaan of weigeren van access configureren
- Webservices die zijn gemaakt in de werkruimte beheren
- De werkruimte verwijderen

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Bovendien vindt het tabblad dashboard u een overzicht van het gebruik van de werkruimte en snel aan te duiden van uw werkruimtegegevens.  

> [AZURE.TIP] Azure Machine Learning Studio, klik op het tabblad **WEB SERVICES** kunt u toevoegen, bijwerken of verwijderen van een Machine Learning-webservice.

Voor het beheren van een werkruimte:

1.  Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/) met uw Microsoft Azure-account: het account dat is gekoppeld aan het abonnement dat Azure gebruiken.
2.  Klik in het deelvenster Microsoft Azure services op **MACHINE LEARNING**.
3.  Klik op de werkruimte die u wilt beheren.

De pagina van de werkruimte heeft drie tabbladen:

- **DASHBOARD** - kunt u weergave-werkruimte gebruik en informatie
- **Configureren** - kunt u de toegang tot de werkruimte beheren
- **WEB SERVICES** - kunt u webservices die zijn gepubliceerd uit deze werkruimte beheren

## <a name="to-monitor-how-the-workspace-is-being-used"></a>Om te controleren hoe de werkruimte wordt gebruikt

Klik op het tabblad **DASHBOARD** .

U kunt vanuit het dashboard, algehele gebruik van uw werkruimte weergeven en een kijkje van informatie over de werkruimte.

- De grafiek **berekenen** weergegeven berekeningscluster resources wordt gebruikt door de werkruimte. U kunt de weergave relatieve of absolute waarden wijzigen en kunt u het tijdsbestek weergegeven in de grafiek wijzigen.
- **Gebruik overzicht** weergeven Azure opslag wordt gebruikt door de werkruimte
- **Snel aan te duiden** biedt een overzicht van informatie over de werkruimte en nuttige koppelingen.

> [AZURE.NOTE] De koppeling **aanmelden bij ML Studio** opent Machine Learning Studio met het Microsoft-Account dat u momenteel bent aangemeld bij. Het Microsoft-Account waarmee u aanmelden bij de portal voor Azure klassieke bij het maken van een werkruimte is niet automatisch gemachtigd die werkruimte wilt openen. Als u wilt een werkruimte opent, u moet zijn aangemeld bij de Microsoft-Account die is gedefinieerd als de eigenaar van de werkruimte, anders moet u een uitnodiging hebben ontvangen van de eigenaar van de deel te nemen aan de werkruimte.


## <a name="to-grant-or-suspend-access-for-users"></a>Om te kennen of schorten toegang voor gebruikers ##

Klik op het tabblad **configureren** .

Vanuit het tabblad configuratie kunt u het volgende doen:

- Toegang tot de werkruimte Machine Learning schorten door te klikken op weigeren. Gebruikers wordt niet langer de werkruimte wilt openen in Machine Learning Studio. Als u wilt herstellen access, klikt u op toestaan.

Als u wilt beheren extra accounts die toegang tot de werkruimte in Machine Learning Studio hebben, klikt u op **aanmelden bij ML Studio** op het tabblad **DASHBOARD** (Zie de voorgaande opmerking over **aanmelden bij ML Studio**). Hiermee opent u de werkruimte in Machine Learning Studio. U kunt op het tabblad **Instellingen** en klik vervolgens op **gebruikers**. U kunt klikken op **Meer gebruikers UITNODIGEN** gebruikers toegang geven tot de werkruimte, of Selecteer een gebruiker en klik op **verwijderen**.


## <a name="to-manage-web-services-in-this-workspace"></a>Voor het beheren van webservices in deze werkruimte

Klik op het tabblad **WEB SERVICES** .

Er verschijnt een lijst van webservices uit deze werkruimte die zijn gepubliceerd.
Als u wilt beheren een webservice, klikt u op de naam in de lijst om de pagina met webonderdelen-service.

Een webservice wellicht een of meer eindpunten gedefinieerd.

- U kunt meer eindpunten naast het eindpunt "Standaard" definiëren. Als u wilt het eindpunt hebt toegevoegd, klikt u op **Eindpunten beheren** onder aan het dashboard om te openen van de portal Azure Machine Learning-webservices.

- Verwijderen van een eindpunt (u kunt het eindpunt "Standaard" niet verwijderen), klikt u op het selectievakje aan het begin van de rij eindpunt en klik op **verwijderen**. Hiermee verwijdert u het eindpunt van de webservice.

    > [AZURE.NOTE] Als een toepassing het eindpunt van de service web gebruikt wordt wanneer het eindpunt wordt verwijderd, wordt de toepassing een foutbericht wanneer u de volgende keer die wordt geprobeerd voor toegang tot de service.

Klik op de naam van een Web-service-eindpunt om dit te openen. 

Vanuit het dashboard, kunt u de algehele gebruik van uw webservice weergeven over een tijdsperiode. U kunt de periode om weer te geven in de periode vervolgkeuzelijst in de rechterbovenhoek van de grafieken gebruik selecteren. Het dashboard bevat de volgende informatie:

- Een grafiek stap van het aantal aanvragen weergegeven **Aanvragen verloop van tijd** over de selecteerde tijdsperiode. Hiermee kunt doen als er pieken in gebruik identificeren.
- **Aanvraag en respons aanvragen** geeft het totale aantal aanvraag en respons oproepen die de service heeft ontvangen via de selecteerde tijdsperiode en hoeveel is mislukt.
- **Tijd voor gemiddelde berekenen van aanvraag en respons** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen verzoeken weergegeven.
- **Batch aanvragen** geeft het totale aantal Batch aanvragen voor de service heeft ontvangen over de selecteerde tijdsperiode en hoeveel is mislukt.
- **Latentie van gemiddeld taak** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen verzoeken weergegeven.
- **Fouten** wordt het totale aantal fouten die zijn opgetreden oproepen naar de webservice weergegeven.
- **Services kosten** worden de kosten voor de facturering-abonnement dat is gekoppeld aan de service weergegeven.

U kunt de volgende eigenschappen bijwerken op de pagina configureren:

* **Beschrijving** , kunt u een beschrijving voor de webservice in te voeren. Beschrijving is een vereist veld.
* **Logboekregistratie** , kunt u in- of uitschakelen van fout bij het aanmelden op het eindpunt. Voor meer informatie over logboekregistratie, Zie inschakelen [logboekregistratie voor Machine Learning-webservices](machine-learning-web-services-logging.md).
* **Inschakelen voorbeeldgegevens** kunt u voorbeeldgegevens die u gebruiken kunt om te testen van de service verzoek-antwoord geven. Als u de webservice in Machine Learning Studio hebt gemaakt, is de voorbeeldgegevens opgehaald uit de gegevens uw gebruikt voor uw model trainen. Als u de service via een programma hebt gemaakt, worden de gegevens worden opgehaald uit de voorbeeldgegevens die u hebt opgegeven als onderdeel van de JSON-pakket.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
