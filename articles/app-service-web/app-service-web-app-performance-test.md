<properties
   pageTitle="Test de prestaties van uw Azure web-app | Microsoft Azure"
   description="Azure web app prestatietests om te controleren hoe uw app omgaat laden van de gebruiker worden uitgevoerd. Meet antwoord tijd en fouten die problemen aangeven mogelijk zoeken."
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>Prestaties test uw Azure WebApp onder laden

Controleer uw web-app prestaties voordat u deze starten of updates naar productie implementeren. Op deze manier kunt u beter beoordelen of uw app klaar voor release-programma is. Erop meer vertrouwen dat uw app kan omgaan met het verkeer tijdens het piek gebruiken of bij uw volgende marketingcampagne.

In openbare voorbeeldweergave kunt u de prestatietest uw app gratis in de Portal Azure.
Deze tests gebruiker belasting van uw app simuleren over een bepaalde periode en meten van uw app antwoord. Uw testresultaten weergeven, bijvoorbeeld hoe snel uw app moet reageren op een bepaald aantal gebruikers. Ze worden ook weergegeven hoeveel aanvragen is mislukt, die mogelijk duiden op problemen met de app.      

![Prestatieproblemen vinden in uw web-app](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Voordat u begint

* U moet een [Azure-abonnement](https://account.windowsazure.com/subscriptions)als u nog niet hebt. Leer hoe u kunt [gratis een Azure-account opent](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

* U moet een [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -account aan uw prestaties test bijhouden. Een geschikte account wordt automatisch gemaakt bij het instellen van uw prestaties testen. Of u kunt een nieuw account maken of een bestaand account gebruiken als u de eigenaar van het account bent. 

* Implementeer de app voor het testen in een niet-productieve-omgeving. Hebben uw app een App-serviceplan dan het abonnement dat wordt gebruikt in productie gebruiken. Op deze manier die u niet van invloed zijn op bestaande klanten van of uw app in productie vertragen. 

## <a name="set-up-and-run-your-performance-test"></a>Instellen en uw prestatietest uitvoeren

0.  Meld u aan bij de [Portal van Azure](https://portal.azure.com). Als u wilt gebruiken in een Visual Studio Team Services-account dat u eigenaar bent, moet u zich aanmelden als de eigenaar van het account.

0.  Ga naar uw web-app.

    ![Ga naar door alles bladeren, Web Apps, uw web-app](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  Ga naar de **prestaties testen**.

    ![Ga naar extra, prestaties testen](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. U hebt nu een [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -account als uw prestaties test overzicht wilt bijhouden koppelen.

    Als er een Team Services-account te gebruiken, selecteert u dat account. Als u niet, een nieuw account maken.

    ![Selecteer bestaande Team Services-account of een nieuw account maken](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  Maak uw prestaties testen. Stel de details en de test uitvoeren. 

U kunt de resultaten in realtime kunt kijken terwijl de test wordt uitgevoerd.

Stel dat we hebben een app die u hebt gekregen van coupons bij vorig jaar feestdagen verkoop. Deze gebeurtenis heeft geduurd 15 minuten met een piekbelasting van 100 gelijktijdige klanten. Willen we dit jaar voor het aantal klanten Dubbelklik. Ook willen we klanttevredenheid verbeteren door de laadtijd voor pagina wordt afgetrokken van 5 seconden op 2 seconden. Zo is, wordt we onze bijgewerkte app prestaties met 250 gebruikers voor 15 minuten testen.

We wordt laden simuleren op onze app door te genereren virtuele gebruikers (klanten) die onze website op hetzelfde moment. Hier ziet ons hoeveel aanvragen worden verbroken of traag reageert.

  ![Maken, instellen en uw prestatietest uitvoeren](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  Standaard-URL van uw web-app wordt automatisch toegevoegd. 
   U kunt de URL als u wilt testen van andere pagina's (alleen voor HTTP GET aanvragen) wijzigen.

   *  Naar de plaatselijke omstandigheden simuleren en latentie verkleinen, selecteert u een locatie die zich het dichtst bij uw gebruikers voor het genereren van laden.

  Hier ziet u de test uitgevoerd. Tijdens de eerste minuut onze pagina wordt geladen traag we horen.

  ![Prestatietest Bezig met realtime gegevens](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  Wanneer de test klaar is, kunt u het Lees dat de pagina wordt na de eerste minuut veel sneller geladen. Hiermee kunt identificeren waar we mogelijk wilt laten beginnen met het oplossen van problemen.

  ![Voltooide prestatietest geeft resultaten weer, met inbegrip van mislukte aanvragen](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Test meerdere URL 's

U kunt ook prestatietests verwerken in een document meerdere URL's die een end-to-end-gebruiker mogelijkheid weergeven door het uploaden van een bestand Visual Studio Web Test uitvoeren. Aantal manieren kunt u een Visual Studio Web Test bestand zijn:

* [Verkeer met Fiddler vastleggen en exporteren als een Visual Studio-Web-testbestand](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Maken van een testbestand laden in Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

Uploaden en een Visual Studio-Web-testbestand uitvoeren:
 
0. Volg de stappen hierboven om te openen van het blad **nieuwe prestaties testen** .
   In dit blade, kies de optie CONFIGFURE testen gebruiken voor het openen van het blad **configureren test gebruiken** .  

    ![De configureren laden testen blade openen](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. Controleer of het TYPE TEST is ingesteld op **Visual Studio Web Test** en selecteer het HTTP-archiefbestand.
    Gebruik het pictogram 'map' het bestand Gegevenskiezer dialoogvenster openen.

    ![Een meerdere URL Visual Studio Web Test-bestand uploaden](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    Nadat het bestand is geüpload, raadpleegt u de lijst met URL's is getest in de sectie URL-DETAILS.
 
0. De belasting voor gebruiker en testen van de duur en kies **test uitvoeren**.

    ![Selecteer de gebruiker laden en duur](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    Als de test is voltooid, ziet u de resultaten in twee deelvensters. Deelvenster aan de linkerkant ziet u de gegevens performnace als een reeks grafieken.

    ![Het deelvenster prestaties resultaten](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    Het rechterdeelvenster bevat een overzicht van mislukte aanvragen, met het type fout en het aantal keren dat deze zijn aangebracht.

    ![Het verzoek om fouten deelvenster](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. Test opnieuw uitvoeren door te kiezen op de bovenkant van het rechterdeelvenster het pictogram **opnieuw uitvoeren** .

    ![De test opnieuw](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>Q & A

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>V: is er een beperking op hoe lang kan ik een toets worden uitgevoerd? 

**A**: Ja, u uw test snel aan de uren in de Portal Azure kunt uitvoeren.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>V: hoe lang kom ik aan prestatietests uitvoeren? 

**A**: na openbare preview-versie, u ontvang 20.000 virtuele gebruiker minuten (VUMs) gratis elke maand met uw Visual Studio Team Services-account. Een VUM is het aantal virtuele gebruikers vermenigvuldigd met het aantal minuten in de test. Als uw behoeften voldoet de gratis limiet overschrijdt, kunt u meer tijd aanschaffen en betalen alleen voor wat u gebruikt.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>V: waar kan ik hoeveel VUMs ik tot nu toe hebt gebruikt controleren?

**A**: U kunt dit bedrag in de Portal Azure controleren.

![Ga naar uw Team Services-account](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Controleer VUMs gebruikt](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>V: Wat is de standaardoptie en mijn bestaande tests ondervindt?

**A**: de standaardoptie voor prestaties laden tests is een handmatige test - is hetzelfde als testen voordat u de meerdere URL optie toegevoegd bij de portal.
Uw bestaande tests gaat u verder met het gebruik van de geconfigureerde URL en werkt als voorheen.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>V: welke functies niet worden ondersteund in de Visual Studio-Web-testbestand?

**A**: op dit moment deze functie niet ondersteunt Web Test-plug-ins, gegevensbronnen, en de extractie van regels. U moet uw Web-testbestand als u wilt verwijderen deze bewerken. We hopen dat ondersteuning toevoegen voor deze functies in toekomstige updates.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>V: worden deze door andere bestandsindelingen Web Test ondersteund?
  
**A**: op presenteren alleen Visual Studio Web Test bestanden-indeling worden ondersteund.
We zou graag van u horen als u ondersteuning nodig voor andere bestandsindelingen. Een e-mail sturen naar [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>V: welke andere manieren kan ik doen met een Visual Studio Team Services-account?

**A**: als u wilt uw nieuwe account hebt gevonden, gaat u naar ```https://{accountname}.visualstudio.com```. Delen van uw code, maken, testen en bijhouden van werk en verzenddatum software – alle in de cloud met een hulpmiddel of de taal. Meer informatie over hoe [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -functies en services helpen uw team gemakkelijker samen te werken en continu implementeren.

## <a name="see-also"></a>Zie ook

* [Eenvoudige cloud prestatietests uitvoeren](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Apache Jmeter prestatietests uitvoeren](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Opnemen en afspelen laden cloudgebaseerde tests](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Prestaties van uw app testen in de cloud](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)
