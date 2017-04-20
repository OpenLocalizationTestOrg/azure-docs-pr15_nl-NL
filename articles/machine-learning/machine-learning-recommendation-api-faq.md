<properties 
    pageTitle="Instellen en gebruiken van de Machine Learning aanbevelingen API | Microsoft Azure" 
    description="Microsoft aanbevelingen API ingebouwd met Azure Machine Learning Veelgestelde vragen" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Installatie en het gebruik van Machine Learning aanbevelingen API Veelgestelde vragen


**Wat is aanbevelingen?**

>[AZURE.NOTE]U moet beginnen met de Service van aanbevelingen-API-cognitieve in plaats van deze versie. De aanbevelingen cognitieve Service wordt deze service worden vervangen, en de nieuwe functies worden er worden ontwikkeld. Er worden nieuwe mogelijkheden, zoals batchen ondersteuning, een betere API Explorer, een duidelijkere API oppervlak, consistenter registratie/facturering ervaring, enzovoort.
> Meer informatie over het [migreren naar de nieuwe cognitieve Service](http://aka.ms/recomigrate)

Voor organisaties en bedrijven die afhankelijk van aanbevelingen zijn met cross-verkopen en Upsell producten en services naar hun klanten, vindt u aanbevelingen in Azure Machine Learning een aanbevelingen selfservice-engine. Dit is een implementatie van samenwerkingscultuur filteren die matrix factoriseren als de algoritme van de core gebruikt. Softwareontwikkelaars kunnen aanbevelingen openen met behulp van de REST API's. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Wat kan ik doen met aanbevelingen?**

AANBEVELINGEN als invoer een item of een reeks items en geeft als resultaat een lijst met relevante aanbevelingen. Bijvoorbeeld: een klant van een online winkel een product klikt. De online winkel wordt verzonden, dat product als invoer voor aanbevelingen, ontvangt een lijst met producten in en wordt bepaald welke van deze producten aan de klant wordt getoond. U kunt u aanbevelingen naar uw onlinewinkel optimaliseren of zelfs op de hoogte brengt uw binnenkant verkoop per afdeling of -gesprek-beheercentrum.

**Gelden er beperkingen gebruik?**

Aanbevelingen heeft de volgende gebruik beperkingen:
* Maximum aantal modellen per abonnement: 10
* Maximum aantal items dat een catalogus kan bevatten: 100.000
* Het maximum aantal gebruik punten die worden gehouden is ~ 5,000,000. De oudste worden verwijderd als nieuwe bestanden worden geüpload of gerapporteerd.
* Maximale grootte van de gegevens die kunnen worden verzonden in e-mailbericht (bijvoorbeeld catalogusgegevens importeren, gebruik importgegevens) is 200 MB
* Aantal transacties per seconde (TPS) voor een aanbevelingen model opbouwen die niet actief is, is ~ 2 TPS. Een aanbevelingen model opbouwen die actief is, kan maximaal 20 TPS bevatten.

##<a name="purchase-and-billing"></a>Aankoop- en facturering 


**Wat kost aanbevelingen gedurende de periode starten?**

Aanbevelingen is een service op basis van abonnement. Opladen is gebaseerd op het volume van transacties per maand. U kunt de [aanbieding pagina] controleren (https://datamarket.azure.com/dataset/amla/recommendations) in Microsoft Azure Marketplace voor prijsinformatie.

**Zijn er de kosten die is gekoppeld aan ondervindt aanbevelingen bijhouden en bewaren van Gebruikersactiviteit voor mij?**

Niet op het moment dat.

**Aanbevelingen is beschikt over een gratis proefversie?**

Er is een gratis spoor die beperkt tot 10.000 transacties per maand is.

**Wanneer ik krijgt voor aanbevelingen?**

Een betaald abonnement is een abonnement waarvoor er maandelijkse kosten zijn. Wanneer u een betaald abonnement koopt, wordt u direct geheven voor gebruik van de eerste maand. U kunt het bedrag dat is gekoppeld aan de aanbieding op de abonnementenpagina (plus belastingen) wordt geheven. Deze maandkalender kosten wordt elke maand op dezelfde agendadatum als uw oorspronkelijke aankoop gemaakt totdat u het abonnement opzegt. 

**Hoe kan ik upgraden naar een hoger niveau-service?**

U kunt kopen of bijwerken van uw abonnement van de [aanbieding pagina] (https://datamarket.azure.com/dataset/amla/recommendations)-pagina op Microsoft Azure Marketplace.

Wanneer u een abonnement upgraden:

* Transacties die op uw oude abonnement nog worden niet toegevoegd aan uw nieuwe abonnement. 
* U volledige prijs betalen voor het nieuwe abonnement, zelfs als u hebt niet-gebruikte transacties van uw oude abonnement.

Proces voor het upgraden van een abonnement:

* Nevigate naar de [aanbieding pagina] (https://datamarket.azure.com/dataset/amla/recommendations).
* Meld u aan bij de Marketplace als u nog niet bent aangemeld.
* In het rechterdeelvenster vindt u alle beschikbare plannen. Klik op het keuzerondje voor de upgrade naar het gewenste abonnement.
* Als u bijwerken wilt, klikt u op **OK**. Als u niet wilt bijwerken, klikt u op **Annuleren**.

**Belangrijke** Lees het dialoogvenster in voordat u een upgrade uitvoert omdat er op facturering en het gebruik consequenties.

**Wanneer wordt mijn abonnement op aanbevelingen beëindigd?**

Uw abonnement moet worden beëindigd wanneer u deze annuleren. Als u annuleren van uw abonnementen wilt, raadpleegt u de volgende instructies.

**Hoe zeg ik mijn abonnement aanbevelingen?**

Als u wilt uw abonnement opzegt, gebruikt u de volgende stappen. Als uw huidige abonnement een betaald abonnement is, wordt uw abonnement van kracht blijft tot het einde van de huidige factureringsperiode. Als u de annulering onmiddellijk effectieve zijn nodig, contact met ons opnemen bij [De ondersteuning van Microsoft](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Opmerking** Geen restitutie wordt uitgedrukt als u het abonnement vóór het einde van een factureringsperiode of voor niet-gebruikte transacties in een factureringsperiode.

* Navigeer naar de [aanbieding pagina] (https://datamarket.azure.com/dataset/amla/recommendations).
* Meld u aan bij de Marketplace als u nog niet bent aangemeld.
* Klik op **Annuleren** aan de rechterkant van de gegevenssetnaam en status. U kunt dit abonnement tot het einde van de huidige factureringsperiode of uw Transactielimiet is bereikt (maximumbelasting).

Als u uw abonnement annuleren wilt direct zo kunt u een nieuw abonnement kopen, moet u een tickets indienen via [Microsoft ondersteuning](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

##<a name="getting-started-with-recommendations"></a>Aan de slag met aanbevelingen

**Aanbevelingen is voor mij?** 

Aanbevelingen in Machine Learning is bedoeld voor organisaties en bedrijven die afhankelijk van aanbevelingen zijn met cross-verkopen en Upsell producten of services naar hun klanten. Als er een klant-website, een verkoopteam, een binnenkant verkoopteam of een callcenter, en als u een catalogus met meer dan een paar tientallen producten of services bieden, uw lijn onder kan profiteren van het gebruik van aanbevelingen. 

Experimenteren met aanbevelingen is bedoeld om eenvoudig worden. De huidige API-versie is vereist voor eenvoudige programmeren. Als u hulp nodig hebt, neemt u contact op met de leverancier die uw website ontwikkeld. Als u een interne IT-afdeling of een interne ontwikkelaar hebt, moeten deze mogelijk zijn om aanbevelingen voor u werken. 

**Wat zijn de vereisten voor het instellen van aanbevelingen?**

Aanbevelingen moet u beschikken over een logboek met gebruikersopties met betrekking tot de catalogus. Als u t zoals een logboek hebt en u een klant omlaag website hebt, aanbevelingen gebruikersactiviteit kunnen verzamelen voor u. 

Aanbevelingen vereist ook een catalogus met producten en services. Als u t dat de catalogus, aanbevelingen kunnen gebruiken voor het gebruik werkelijke klant en distill een catalogus. Een impliciete catalogus worden niet opgenomen voor items die zijn niet beschikbaar als onderdeel van de gebruikerstransacties.

**Hoe stel ik aanbevelingen in voor de eerste keer?**

Na [abonneren] (https://datamarket.azure.com/dataset/amla/recommendations) aanbevelingen, moet u de documentatie API gebruiken in de [Azure Machine Learning aanbevelingen aan de slag:](machine-learning-recommendation-api-quick-start-guide.md) voor het instellen van de service.

**Waar vind ik API-documentatie?** 

De API-documentatie is [Azure Machine Learning aanbevelingen aan de slag:](machine-learning-recommendation-api-quick-start-guide.md).

**Welke opties heb ik catalogus en het gebruik van gegevens uploaden naar aanbevelingen?**

U hebt twee opties voor het uploaden van de catalogus en het gebruik van gegevens: U kunt de gegevens exporteren vanuit uw CRM of andere logboeken en uploadt dit naar aanbevelingen, of u kunt labels toevoegen aan uw website die u activiteiten van gebruikers bijhouden wilt. Als u de laatste methode gebruikt, worden de gegevens worden opgeslagen in Azure wordt aangegeven.

##<a name="maintenance-and-support"></a>Onderhoud en ondersteuning

**Hoe groot kan mijn gegevensverzameling zijn?**

Elke set gegevens kunnen maximaal 100.000 catalogusitems bevatten en omhoog 2048 MB aan gegevens over zoekgebruik.
Een abonnement kan bovendien maximaal 10 gegevensverzamelingen (modellen) bevatten.

**Waar vind ik de technische ondersteuning voor aanbevelingen?**

Technische ondersteuning is beschikbaar op de [Ondersteuning van Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) -site.

**Waar vind ik de gebruiksvoorwaarden?**

[Microsoft Azure Machine Learning-servicevoorwaarden aanbevelingen API](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 
