<properties 
    pageTitle="Python machine learning scripts uitvoeren | Microsoft Azure" 
    description="Overzichten ontwerpen grondbeginselen van ondersteuning voor Python scripts in Azure Machine Learning en basisgebruik scenario's, mogelijkheden en beperkingen." 
    keywords="Python machine learning-, pandas, python pandas, python scripts, python scripts uitvoeren"
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Python machine learning scripts uitvoeren in Azure Machine Learning Studio

In dit onderwerp worden de huidige ondersteuning voor Python scripts in Azure Machine Learning van de ontwerpen grondbeginselen. De belangrijkste functies kunnen u ook vinden, waaronder ondersteuning voor het importeren van bestaande code, visualisaties exporteren en klik ten slotte enkele beperkingen en lopende werk worden besproken.

[Python](https://www.python.org/) is een onmisbaar hulpmiddel bij het hulpmiddel borst van veel gegevens wetenschappers. Deze heeft:

-  de syntaxis van een elegante en beknopt, 
-  ondersteuning voor meerdere platforms, 
-  een grote verzameling krachtige bibliotheken, en 
-  hulpmiddelen voor de ontwikkeling vervallen. 

Python wordt gebruikt in alle fasen van de werkstroom meestal gebruikt in machine learning-modellen, nemen van gegevens en verwerkt, functie constructie en model-training, en klik vervolgens gegevensvalidatie en implementatie van de modellen. 

Azure Machine Learning Studio ondersteunt insluiten Python scripts in verschillende delen van een machine learning experiment en ze ook naadloos publiceren als scalable, geoperationaliseerd webservices op Microsoft Azure.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Beginselen van Python scripts in Machine Learning ontwerpen
De primaire gebruikersinterface naar Python in Azure Machine Learning Studio is via het [Python Script uitvoeren] [ execute-python-script] module Zie afbeelding 1.

![image1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Afbeelding 1. De module **Python Script uitvoeren** .

Het [Python Script uitvoeren] [ execute-python-script] module accepteert maximaal drie invoeritems en genereert maximaal twee uitvoer (Zie hieronder), net als de R analoge, het [R-Script uitvoeren] [ execute-r-script] module. De Python programmacode wordt uitgevoerd is ingevoerd in het parametervak als een speciaal benoemde ingangspunt functie genoemd `azureml_main`. Hier volgen de belangrijke principes voor de uitvoering van deze module gebruikt:

1.  *Moet idiomatische voor Python gebruikers.* De meeste Python gebruikers factor hun code als-functies in modules, zodat een groot aantal uitvoerbare instructies plaatsen in een module op het hoogste niveau relatief niet vaak voorkomen is. Het script gaat hierdoor kunnen ook een speciaal benoemde Python-functie in plaats van alleen een reeks instructies. De objecten die staan weergegeven in de functie zijn standaard Python bibliotheek typen zoals [Pandas](http://pandas.pydata.org/) gegevensframes en [NumPy](http://www.numpy.org/) matrices.
2.  *Hoge beeldkwaliteit tussen lokaal moet hebben en cloud executions.* De backend gebruikt voor het uitvoeren van de code Python is gebaseerd op [Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1, een veelgebruikte platforms wetenschappelijke Python verdeling. Deze wordt geleverd met dicht bij 200 van de meest voorkomende Python-pakketten. Gegevens wetenschappers kunnen daarom foutopsporing en kwalificeren van hun code, op hun lokale Azure Machine Learning-compatibele Anaconda-omgeving. Gebruik vervolgens de bestaande ontwikkeling omgevingen zoals [IPython](http://ipython.org/) notitieblok of nieuwe [Python Tools voor Visual Studio](http://aka.ms/ptvs) uit te voeren als onderdeel van een experiment Azure Machine Learning met hoge betrouwbaarheid. Verdere, de `azureml_main` ingangspunt is een vanille Python-functie en zonder specifieke Azure Machine Learning-code worden geschreven of de SDK geïnstalleerd.
3.  *Moet naadloos opbouwbare met andere Azure Machine Learning-modules.* Het [Python Script uitvoeren] [ execute-python-script] module accepteert, als invoer en uitvoer, standaard Azure Machine Learning-gegevenssets. Het onderliggende framework verbinding transparant en efficiënt van de Azure Machine Learning en Python runtimes (ondersteunende functies zoals ontbrekende waarden). Python kan dus worden gebruikt in combinatie met bestaande Azure Machine Learning-werkstromen, waaronder mensen die bij R en SQLite inbellen. Een kunt daarom voorzien de werkstromen die:
  * Gebruik Python en Pandas voor gegevens voorverwerking en -verwijdering 
  * de gegevens feed aan een SQL-transformatie, deelnemen aan meerdere gegevenssets voor functies van formulier 
  * met de uitgebreide verzameling algoritmen in Azure Machine Learning, modellen trainen en 
  * evalueren en na verwerken de resultaten met R.


## <a name="basic-usage-scenarios-in-machine-learning-for-python-scripts"></a>Basisgebruik scenario's in Machine Learning voor Python scripts
In dit gedeelte we enkele van de eenvoudige gebruik van de [Python Script uitvoeren] enquête[ execute-python-script] module.
Zoals eerder is vermeld, wordt elke invoer voor de module Python worden weergegeven als Pandas gegevensframes. Meer informatie over Python Pandas en hoe deze kan worden gebruikt om gegevens te bewerken effectief en efficiënt kan worden gevonden in *Python voor gegevensanalyse* (O'Reilly, 2012) door West McKinney. De functie moet één Pandas gegevensframe verpakt binnen een Python [sequentie](https://docs.python.org/2/c-api/sequence.html) , zoals een tupel, lijst of NumPy matrix als resultaat. Het eerste element van deze reeks klikt u vervolgens in de eerste uitvoerpoort van de module geretourneerd. Dit schema wordt weergegeven in afbeelding 2.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Afbeelding 2. Toewijzing van poorten naar parameters invoer- en retourwaarde naar uitvoerpoort.

Gedetailleerdere semantiek van hoe de invoer poorten aan de parameters van toegewezen de `azureml_main` functie worden weergegeven in de tabel 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabel 1. De toewijzing van invoer poorten naar functieparameters.

De toewijzing tussen invoer-poorten en -functieparameters is positionele. De eerste verbonden invoer poort is toegewezen aan de eerste parameter van de functie en de tweede invoer (als er verbinding is) is toegewezen aan de tweede parameter van de functie.

## <a name="translation-of-input-and-output-types"></a>Vertaling van invoer- en uitvoerbereik typen
Zoals eerder beschreven, wordt invoer gegevenssets in Azure Machine Learning worden geconverteerd naar gegevens frames in de Pandas en uitvoer gegevensframes worden geconverteerd naar een Azure Machine Learning gegevenssets. De volgende conversies worden uitgevoerd:

1.  Tekenreeks en numerieke kolommen worden geconverteerd naar-is en ontbrekende waarden in een gegevensset worden omgezet in 'NA'-waarden in Pandas. De dezelfde conversie voor de manier waarop terug (NB waarden in Pandas worden geconverteerd naar ontbrekende waarden in Azure Machine Learning).
2.  Index vectoren in Pandas worden niet ondersteund in Azure Machine Learning. Alle invoergegevens frames in de functie Python hebben altijd een 64-bits numerieke index tussen 0 en het aantal rijen min 1. 
3.  Azure Machine Learning-gegevenssets geen dubbele kolomnamen en kolomnamen die geen tekenreeksen zijn. Als een uitvoer gegevensframe niet-numerieke kolommen bevat, wordt gebeld door het kader `str` van de kolomnamen. Een dubbele kolomnamen worden ook automatisch vervormd om te garanderen dat de namen uniek zijn. Het achtervoegsel (2) wordt toegevoegd aan het eerste duplicaat, (3) naar de tweede dubbele, enzovoort.

## <a name="operationalizing-python-scripts"></a>Dwarsas Python scripts
Een [Python Script uitvoeren] [ execute-python-script] modules gebruikt in een score experiment worden aangeroepen wanneer gepubliceerd als een webservice. Afbeelding 3 ziet u bijvoorbeeld een score experiment met de code één Python expressie te evalueren. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Afbeelding 3. De webservice voor het evalueren van een expressie Python.

Een webservice die is gemaakt op basis van dit experiment neemt als invoer van een expressie Python (als een tekenreeks), verzonden naar de interpreter Python en retourneert een tabel met zowel de expressie als het geëvalueerde resultaat.

## <a name="importing-existing-python-script-modules"></a>Bestaande Python script modules importeren
Er is een algemene gebruiksvoorbeeld voor veel gegevens wetenschappers bestaande Python scripts in Azure Machine Learning experimenten opnemen. In plaats van het samenvoegen van en alle code plakken in een vak één script het [Python Script uitvoeren] [ execute-python-script] module accepteert een derde invoer poort waaraan een zip-bestand met de modules Python kan worden verbonden. Het bestand is vervolgens bestanden door het kader execution gedurende runtime en de inhoud worden toegevoegd aan het bibliotheekpad van de interpreter Python. De `azureml_main` ingangspunt functie kan deze modules vervolgens rechtstreeks importeren.

Een voorbeeld, kunt u het bestand Hello.py met een eenvoudige functie voor "Hallo, wereld".

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

Afbeelding 4. De gebruiker gedefinieerde functie.

Maak vervolgens een bestand Hello.zip Hello.py met:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Afbeelding 5. ZIP-bestand met door gebruiker gedefinieerd Python code.

Vervolgens upload dit als een gegevensset naar Azure Machine Learning Studio. Maken en uitvoeren van een eenvoudige experiment die gebruikmaakt van de code Python in het bestand Hello.zip door deze te koppelen aan de derde invoer poort van het Script Python uitvoeren, zoals in deze afbeelding.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

Afbeelding 6. Voorbeeld experiment met de gebruiker gedefinieerde Python code die zijn geüpload als een zipbestand.

De module uitvoer ziet u dat het zip-bestand uitgepakt is en de functie `print_hello` daadwerkelijk is uitgevoerd.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
Afbeelding 7. De gebruiker gedefinieerde functie wordt gebruikt in het [Python Script uitvoeren] [ execute-python-script] module.

## <a name="working-with-visualizations"></a>Werken met visualisaties
Waarnemingspunten die zijn gemaakt met behulp van MatplotLib die kunt visualiseren van de browser kunnen worden geretourneerd door de [Python Script uitvoeren][execute-python-script]. Maar de waarnemingspunten worden niet automatisch omgeleid naar afbeeldingen als ze worden bij het gebruik van R. Zodat moet de gebruiker expliciet eventuele waarnemingspunten in PNG-bestanden opslaan als ze zijn terug naar Azure Machine Learning worden geretourneerd. 

Om te kunnen de afbeeldingen van MatplotLib genereren, moet u de volgende procedure concurrentie:

* de backend overschakelen naar "Al" van de standaard Qt gebaseerde renderer 
* een nieuwe afbeelding-object maken 
* de as krijgen en alle waarnemingspunten erin genereren 
* de afbeelding opslaan in een PNG-bestand 

Dit proces wordt geïllustreerd in de volgende afbeelding 8 waarmee een spreidings-tekeningen-matrix met de functie scatter_matrix in Pandas.
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Figuur 8. MatplotLib afbeeldingen naar afbeeldingen op te slaan.



Afbeelding 9 ziet een experiment waarin het script eerder weergegeven om terug te keren wordt getekend via de tweede uitvoerpoort.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Afbeelding 9. Zomersportevenementen waarnemingspunten gegenereerd op basis van Python code.

Het is mogelijk om terug te keren meerdere afbeeldingen door ze in verschillende afbeeldingen op te slaan, de Azure Machine Learning-runtime opgehaald alle afbeeldingen en voegt ze samen voor visualisatie.


## <a name="advanced-examples"></a>Geavanceerde voorbeelden
De Anaconda-omgeving die is geïnstalleerd in Azure Machine Learning bevat algemene-pakketten zoals NumPy SciPy, en Scikits-leert en deze kunnen effectief worden gebruikt voor verschillende gegevensverwerking taken in een normale machine learning-pijplijn. Ziet u het gebruik van ensemble cursisten in Scikits meer te berekenen van de functie urgentie scores voor een gegevensset als u bijvoorbeeld de volgende experiment en script. De scores kunnen vervolgens worden gebruikt om uit te voeren gecontroleerde onderdelen selecteren voordat u die naar een ander machine learning-model.

Hieronder vindt u de functie Python te berekenen van de urgentie scores en de volgorde op basis van deze functies:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Afbeelding 10. Functie rang functies door scores.
 Het volgende experiment vervolgens expressie wordt berekend en geeft als resultaat de urgentie scores van functies in de gegevensset "Pima Indische Diabetes" Azure Machine weten:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
Afbeelding 11. Experimenteren met rang functies in de gegevensset Pima Indische Diabetes.

## <a name="limitations"></a>Beperkingen 
Het [Python Script uitvoeren] [ execute-python-script] momenteel heeft de volgende beperkingen:

1.  *Sandbox worden uitgevoerd.* De runtime Python is momenteel sandbox en daardoor kan geen toegang tot het netwerk of het lokale bestandssysteem permanente zodanig. Alle bestanden lokaal opgeslagen zijn geïsoleerd en verwijderd nadat de module is voltooid. De code Python geen toegang tot de meeste mappen op de computer die wordt uitgevoerd op, de uitzondering wordt de huidige map en submappen.
2.  *Geen geavanceerde ontwikkeling en ondersteuning voor foutopsporing.* De module Python biedt momenteel geen ondersteuning van IDE-functies zoals intellisense en foutopsporing. Ook als de module tijdens de uitvoering mislukt, de volledige Python stacktrace is beschikbaar, maar moet worden weergegeven in het uitvoerlogboek voor de module. Momenteel raden dat u ontwikkelen en fouten opsporen in hun Python scripts in een omgeving zoals IPython en importeer de code in de module.
3.  *Tijd optellen frame uitvoer.* Het ingangspunt Python is alleen toegestaan als resultaat een gegevensframe één als uitvoer. Het is niet momenteel mogelijk om terug te keren willekeurige Python objecten zoals ervaren modellen rechtstreeks terug naar de Azure Machine Learning-runtime. [R-Script uitvoeren]zoals[execute-r-script], die dezelfde beperking bevat, is echter mogelijk in veel gevallen aan pickle objecten in een matrix van de bytes en vervolgens terug die binnenkant van een gegevensframe.
4.  *Niet kunnen Python installatie aanpassen*. De enige manier om toe te voegen aangepaste Python modules is momenteel via het zip-bestand om eerder beschreven. Dit is geschikt voor kleine modules, is maar het lastige voor grote modules (met name die met systeemeigen dll-bestanden) of een groot aantal modules. 


##<a name="conclusions"></a>Conclusies
Het [Python Script uitvoeren] [ execute-python-script] module biedt een wetenschappelijk gegevens voor het opnemen van bestaande Python code in cloud gehoste machine learning werkstromen in Azure Machine Learning en voor naadloos mogelijk maken als onderdeel van een webservice. De Python script-module werkt natuurlijk samen met andere modules in Azure Machine Learning en kan worden gebruikt voor een reeks taken uit verkennen oude verwerking, naar het ophalen van functies, evaluatie en na verwerking van de resultaten. De backend-runtime gebruikt voor uitvoering is gebaseerd op Anaconda, een Python uitgebreid geteste en de meest gebruikte kwadraatverdeling. Hiermee kunt u eenvoudig voor u naar de ingebouwde bestaande code-elementen in de cloud.

We bieden extra functionaliteit voor het [Python Script uitvoeren] verwachten[ execute-python-script] module zoals de mogelijkheid om te trainen en gegevensmodellen in Python mogelijk te maken en toe te voegen betere ondersteuning voor de ontwikkeling en foutopsporing in Azure Machine Learning Studio.

## <a name="next-steps"></a>Volgende stappen

Zie het [Python Developer Center](/develop/python/)voor meer informatie.

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
