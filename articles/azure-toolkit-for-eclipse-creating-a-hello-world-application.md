<properties
    pageTitle="Een Hallo wereld-Cloudservice voor Azure in Eclips maken"
    description="Informatie over het maken van een eenvoudige Hallo allemaal-toepassing met behulp van de Azure-Toolkit voor Eclips."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Een Hallo wereld-Cloudservice voor Azure in Eclips maken #

De volgende stappen hoe u maken en implementeren van een eenvoudige JSP toepassing Azure met behulp van de Azure-Toolkit voor Eclips. Een voorbeeld JSP voor eenvoudig wordt weergegeven, maar ten zeerste dezelfde stappen wordt geschikt te maken voor een servlet Java, wat betreft Azure-implementatie is.

De toepassing ziet er ongeveer als volgt uit:

![][ic600360]

## <a name="prerequisites"></a>Vereisten voor ##

* Een Java ontwikkelaars Kit (JDK), v 1.7 of hoger.
* IDE Eclips voor Java EE ontwikkelaars, Indigo of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>.
* Een verdeling van een webserver Java gebaseerde of toepassingsserver, zoals Apache Tomcat, GlassFish, JBoss-toepassingsserver, Jetty of IBM® WebSphere®-toepassing Server Liberty Core.
* Een Azure-abonnement die kan worden aangeschaft van <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor Eclips. Zie de [installatie van de Azure Toolkit voor Eclips][]voor meer informatie.

## <a name="to-create-a-hello-world-application"></a>Een toepassing Hallo allemaal maken ##

Eerst beginnen we uitschakelen met het maken van een project Java.

*  Eclips, starten en klik op het menu **bestand**, klikt u op **Nieuw**en klik vervolgens op **Dynamische Web Project**. (Als er geen **Dynamische Web Project** vermeld als een beschikbare project na te klikken op **bestand**, **Nieuw**en voer daarna de onderstaande stappen: klik op **bestand**, klikt u op **Nieuw**, klik op **Project...**, **Web**uitvouwen, klik op **Dynamische Web Project**en klik op **volgende**.)
*  Geef het project **MyHelloWorld**voor toepassing van deze zelfstudie wordt. (Zorgen dat u deze naam gebruiken, opeenvolgende stappen in deze zelfstudie verwachten uw WAR-bestand MyHelloWorld naam). Uw scherm ziet er ongeveer als volgt uit: ![][ic589576]
* Klik op **Voltooien**.
* Uitvouwen **MyHelloWorld**binnen de Eclips Projectverkenner weergave. Met de rechtermuisknop op **webinhoud**, klikt u op **Nieuw**en klik vervolgens op **JSP-bestand**.
* Klik in het dialoogvenster **Nieuw JSP-bestand** een naam geven het bestand **index.jsp**. Houd de bovenliggende map als **MyHelloWorld/webinhoud**, zoals wordt weergegeven in het volgende:  ![][ic659262]
* Klik in het dialoogvenster **Selecteer JSP sjabloon** voor toepassing van deze zelfstudie selecteert u **Nieuw JSP-bestand (html)** en klik op **Voltooien**.
* Wanneer het index.jsp-bestand is geopend in Eclips toevoegen aan tekst om weer te geven dynamisch **Hallo wereld!** binnen de bestaande `<body>` element. Uw bijgewerkte `<body>` inhoud moet worden weergegeven als volgt te werk:
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* Sla index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Naar het implementeren van de toepassing Azure snelle en eenvoudige manier ##

Zodra u een webtoepassing Java wilt testen hebt, kunt u de snelkoppeling voor het volgende proberen rechtstreeks op de Azure cloud.

1. Klik in de Eclips Projectverkenner op **MyHelloWorld**.
1. Op de werkbalk Eclips, klik op de vervolgkeuzeknop **publiceren** en klik op **Publiceren als Azure Cloud-Service**
    ![][publishDropdownButton]
1. Als u deze toepassing Azure voor de eerste keer publiceren wilt en u een project Azure-implementatie van deze toepassing voordat u niet hebt gemaakt, een project Azure-implementatie voor u automatisch gemaakt. Zie de volgende melding te zien, waarin ook het pakket JDK en toepassingsserver die automatisch wordt geïmplementeerd als uw toepassing wilt uitvoeren.
    ![][ic789598]

    Deze methode snelkoppeling kunt een snelle en eenvoudige manier om te testen van uw toepassing in Azure zonder dat u moet een specifieke server of -JDK die verschilt van de standaardwaarden configureren. Als u tevreden met de standaardinstellingen bent, kunt u Klik op **OK** om door te gaan met de volgende stappen uit.
    Echter als u wilt wijzigen van de JDK of toepassingsserver gebruiken voor uw toepassing, kunt u die later doen door het bewerken van het project Azure-implementatie die automatisch voor u is gemaakt, of u kunt nu op **Annuleren** klikken en lees het **gedeelte over Azure implementatie-projecten** van deze zelfstudie.
1. Klik in het dialoogvenster **publiceren naar Azure** :
    1. Als er geen abonnementen op selecteren in de lijst met **abonnementen** die nog, als volgt te werk als de abonnementsgegevens van uw wilt importeren:
        1. Klik op **importeren uit bestand publiceren-instellingen**.
        1. Klik op **Publiceren-instellingen-bestand downloaden**in het dialoogvenster **Gegevens importeren-abonnement** . Als u uw Azure-account nog niet bent aangemeld, wordt u gevraagd aan te melden. Vervolgens wordt u gevraagd om te publiceren opslaan een Azure instellingenbestand. Sla deze op uw lokale computer.
        1. Nog steeds in het dialoogvenster **Importgegevens abonnement** klikt u op de knop **Bladeren** , selecteer het bestand dat publiceren instellingen die u lokaal in de vorige stap hebt opgeslagen en klik vervolgens op **openen**. Uw scherm er ongeveer als volgt uit: ![][ic644267]
        1. Klik op **OK**.
    1. Voor **abonnement**, selecteert u het abonnement dat u gebruiken voor uw implementatie wilt.
    1. Selecteer het opslag-account dat u wilt gebruiken voor **opslag-account**, of klik op **Nieuw** om een nieuwe opslag-account maken.
    1. Selecteer de cloudservice die u wilt gebruiken voor **de naam van de Service**, of klik op **Nieuw** om te maken van een nieuwe cloudservice.
    1. Voor de **Doel-besturingssysteem**, selecteert u de versie van het besturingssysteem dat u wilt gebruiken voor uw implementatie.
    1. Selecteer voor de **doel-omgeving**, voor toepassing van deze zelfstudie **tijdelijke**. (Wanneer u klaar bent om te implementeren naar uw productielocatie, moet u wijzigen dit naar **productie**.)
    1. Optioneel: Zorg ervoor dat **overschrijven vorige implementatie** is ingeschakeld als u wilt dat de nieuwe distributie moeten worden overschreven de vorige implementatie. Wanneer u deze optie inschakelt, wordt u geen ervaring "409 conflict" problemen bij het publiceren op dezelfde locatie.
        Houd er rekening mee dat het dialoogvenster **publiceren naar Azure** een sectie voor **Externe toegang bevat**. Standaard externe toegang niet is ingeschakeld en we wordt deze niet inschakelen voor dit voorbeeld. Als u wilt externe toegang inschakelen, voert u een gebruikersnaam en wachtwoord wilt gebruiken wanneer u extern logboekregistratie in. Zie [Externe toegang inschakelen voor Azure-implementaties in Eclips][]voor meer informatie over externe toegang.
        Uw **publiceren naar Azure** -dialoogvenster ziet er ongeveer als volgt uit: ![][ic719488]
1. Klik op **publiceren** publiceren naar de tijdelijke-omgeving.
    Wanneer u wordt gevraagd om uit te voeren van een volledige opbouwen, klikt u op **Ja**. Dit kan enige tijd duren voor de eerste opbouwen.
    Een **Logboek met Azure activiteit** wordt weergegeven in de sectie van de weergaven van uw Eclips met tabbladen.
    ![][ic719489]
   U kunt dit logboek, evenals de weergave van de **Console** , gebruiken om de voortgang van uw implementatie weer te geven. Er is een alternatief Meld u aan bij de [Beheerportal van Azure][], met behulp van de sectie **Cloud Services** en de status controleren.
1. Wanneer uw implementatie is geïmplementeerd, wordt het **Gebeurtenissenlogboek van Azure** status **Published**weergegeven. Klik op **Published**, zoals wordt weergegeven in de volgende afbeelding, en uw browser wordt geopend een exemplaar van uw implementatie.
    ![][ic719490]

Omdat dit een implementatie naar een testomgeving was, de naam van de DNS-ervan worden met de notatie http://&lt;*guid*&gt;. cloudapp.net en de URL hebben bevatten de naam van de DNS- en achtervoegsel voor uw toepassing. Bijvoorbeeld http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (Het gedeelte **MyHelloWorld** is hoofdlettergevoelig.) U kunt ook de naam van de DNS-bekijken als u klikt op de naam van de implementatie in de Azure Platform Management Portal (binnen de Cloud Services-gedeelte van de beheerportal).

Hoewel dit overzicht van een implementatie naar de testomgeving, een implementatie aan productie volgt van dezelfde stappen, behalve in het dialoogvenster **publiceren naar Azure** en selecteert u **productie** in plaats van de **tijdelijke** voor de **doel-omgeving**. Een implementatie naar productie resultaten in een URL op basis van de DNS-naam van uw keuze wordt in plaats van een GUID zoals deze wordt gebruikt voor tijdelijke.

>[AZURE.WARNING] U hebt nu uw Azure-toepassing in de cloud geïmplementeerd. Echter voordat u verder gaat, en zich daarna realiseert dat een gedistribueerde toepassing, zelfs als deze niet wordt uitgevoerd, blijft toenemen factureerbare tijd voor uw abonnement. Het is dus erg belangrijk dat u ongewenste implementaties uit uw Azure-abonnement verwijderen.

## <a name="about-azure-deployment-projects"></a>Azure-implementatie projecten ##

Om te kunnen een of meer Java-toepassingen naar Azure implementeert, is een Azure-implementatie-Project nodig. Dit wordt afgespeeld de rol van de 'pakket' die uw toepassingen worden samengevoegd moeten in om te worden gepubliceerd op Azure.

Naast de informatie over uw toepassingen, een Azure-implementatie-project bevat ook informatie over andere belangrijke onderdelen van de implementatie belangrijker: de container van de toepassing-server om uit te voeren in uw WebApp en de runtime Java uit te voeren op. Azure ondersteunt een groot aantal Java runtimes en Java-toepassingsservers die kunt u kiezen uit.

Hoewel het voorbeeld hier gebruikt sterk vereenvoudigd voor studiedoeleinden, kan een project Azure-implementatie ook andere belangrijke configuratiegegevens waarmee u kunt de cloudservices van bijna willekeurig complexe scalable, ten zeerste beschikbaar, met meerdere niveaus maken met uw toepassingen bevatten. U kunt **sessie affiniteit ("Plaktoetsen sessies")**, **snel caching** **Foutopsporing op afstand**, **SSL offloading**, **firewall/poort routering**, **RAS**en een aantal andere krachtige mogelijkheden inschakelen.

Als u de vorige sectie van deze zelfstudie ("naar uw toepassing implementeren naar Azure snelle en eenvoudige manier") hebt uitgevoerd, kunt u ziet nu een nieuw project Azure-implementatie in de Projectverkenner automatisch voor u gegenereerd en met de naam '**MyHelloWorld_onAzure**'.

U kunt ook hebt deze zelfstudie door eerst project te maken op een lege Azure-implementatie uzelf en vervolgens toe uw toepassingen toe te voegen. Het is een langere proces, maar zodat u meer controle over de eerste configuratie vanaf het begin.

U een nieuw project van Azure-implementatie maken, klikt u op de knop **Nieuw Azure implementatieproject** ![][ic710876].

Ongeacht of u werken met een bestaand project van Azure-implementatie, of maken van een geheel nieuwe, u kunt wijzigen van de configuratie-instellingen en -onderdelen, zoals de JDK of de toepassingsserver, gelijkmatig zijn gemakkelijk op elk gewenst moment.

De JDK, of de toepassingsserver of de toepassingslijst in een bestaand Azure-implementatie-project wijzigen:

1. Vouw het knooppunt uit project (bijvoorbeeld **MyHelloWorld_onAzure**) in de Projectverkenner
2. Met de rechtermuisknop op **WorkerRole1**
3. Vouw het submenu **Azure** in het contextmenu
4. Klik op **Serverconfiguratie**

Ongeacht of u bent gestart serverconfiguratiestappen door te bewerken van een bestaand project van Azure-implementatie, zoals hierboven, of u maakt een nieuw account maken, ziet u hetzelfde type als dialoogvensters zodat u kunt uw JDK, server-en-toepassing configureren. Meer informatie over het wijzigen van de instellingen in de dialoogvensters, bijvoorbeeld om af te wijzigen van de JDK, de toepassingsserver en toepassingen toevoegen of verwijderen in een implementatie, raadpleegt u het artikel [configuratie-eigenschappen Server][] .

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Alleen Windows: uw toepassing naar de emulator berekeningscluster implementeren ##

>[AZURE.NOTE] De Azure emulator is alleen beschikbaar voor Windows. Dit gedeelte overslaan als u een besturingssysteem dan Windows gebruikt.

Als u een nieuw Azure-implementatie-project de stappen eerder, dat wil zeggen impliciet hebt gemaakt door te publiceren van uw toepassing met Azure, de JDK en de toepassing zijn servers geconfigureerd voor de cloud, maar niet voor lokale emulatie. Uw project om voor te bereiden testen in de lokale emulator, als volgt te werk:

1. Klik in de Eclips Projectverkenner op **MyHelloWorld_onAzure**.
1. Klik met de rechtermuisknop op **WorkerRole1**.
1. Vouw het submenu **Azure** in het contextmenu.
1. Klik op **Serverconfiguratie**.
1. Schakel op het tabblad **JDK** als de toolkit standaard vooraf is geconfigureerd lokale JDK voor u. Zo niet, of als u wilt wijzigen, aangenomen dat posities, zorg ervoor dat het selectievakje **de JDK uit dit bestandspad voor het testen van lokaal gebruiken** is ingeschakeld en u op de locatie van de JDK-installatie die u wilt gebruiken geeft. Als u wijzigen wilt, klikt u op de knop **Bladeren** en selecteer de locatie van de map van het JDK gebruiken met het besturingselement bladeren.
1. Klik op het tabblad **Server** .
1. Voer in het tekstvak **lokale serverpad** onderaan in het dialoogvenster het pad van een server lokaal zijn geïnstalleerd, die overeenkomt met het type en het nummer van de primaire versie van de server die boven aan het dialoogvenster, onder het selectievakje **Deploy een server van dit type** is geselecteerd. Als u een ander type of primaire versie van de toepassingsserver gebruiken wilt, eerst de selectie onder dat het selectievakje wijzigen.
1. Klik op **OK**.
1. Op de werkbalk Eclips, klikt u op de knop **uitvoeren in Azure Emulator** ![][ic710879]. Als de knop **uitvoeren in Azure Emulator** niet is ingeschakeld, zorg ervoor dat **MyHelloWorld_onAzure** is geselecteerd in de Eclips Projectverkenner en zorg ervoor dat de Eclips Projectverkenner de focus als het huidige venster heeft. Hiermee wordt eerst een volledige versie van uw project starten en vervolgens uw Java-webtoepassing in de emulator berekeningscluster starten. (Notitie dat afhankelijk van de kenmerken van de prestaties van uw computer, het eerste bouwen tussen een paar seconden een paar minuten duurt, maar latere genereert krijgt sneller.) Nadat de eerste stap van de build is voltooid, wordt u door Windows Gebruikersaccountbeheer (UAC) gevraagd toe te staan dat deze opdracht om uw computer te wijzigen. Klik op **Ja**.

>[AZURE.IMPORTANT] Als u de UAC-prompt niet ziet, Controleer de Windows-taakbalk voor het UAC-pictogram en klikt u op het eerste. Soms Gebruikersaccountbeheer prompt wordt niet weergegeven als een bovenste venster, maar alleen als een taakbalkpictogram zichtbaar is.

1. Bekijk de uitvoer van de berekeningscluster emulator interface om te bepalen of er problemen hebt met uw project zijn. Afhankelijk van de inhoud van uw implementatie, kan dit een paar minuten voor uw toepassing volledig worden gestart binnen de emulator berekeningscluster duren.
1. Start uw browser en gebruik de URL `http://localhost:8080/MyHelloWorld` als het adres (de `MyHelloWorld` deel van de URL is hoofdlettergevoelig). Ziet u uw MyHelloWorld-toepassing (de uitvoer van index.jsp), vergelijkbaar met de volgende afbeelding: ![][ic589579]

Wanneer u klaar bent voor uw toepassing uitgevoerd in de emulator berekenen, klikt u op de werkbalk Eclips, klikt u op de knop **Opnieuw Azure Emulator** ![][ic710880].

## <a name="to-delete-your-deployment"></a>Uw implementatie verwijderen ##

Als u wilt verwijderen van uw implementatie binnen de Toolkit Azure voor Eclips, zorg ervoor dat **MyHelloWorld_onAzure** is geselecteerd in de Eclips Projectverkenner, zorgen dat de Projectverkenner Eclips heeft het huidige venster richten en klik op de knop **publicatie ongedaan maken** , ![][ic710883], op de werkbalk Eclips. (U kunt dezelfde bewerking kan doen door met de rechtermuisknop op **MyHelloWorld_onAzure** in Projectverkenner Eclips van **Azure** klikken en vervolgens te klikken op **Undeploy vanuit de Cloud Azure**.) Het dialoogvenster **Azure Project publicatie ongedaan maken** wordt weergegeven.

![][ic719491]

Selecteer de service en cloud-abonnement met de implementatie, selecteert u de implementatie die u wilt verwijderen en klik vervolgens op **publicatie ongedaan maken**.

(Een alternatief voor het gebruik van de toolkit verwijderen van de implementatie is het gebruik van de sectie **Cloudservices** van de Azure-beheerportal: Ga naar uw implementatie, selecteert u deze en klik vervolgens op de knop **verwijderen** . Hiermee wordt stoppen en vervolgens verwijderen, de implementatie. Als u alleen wilt stoppen met de implementatie en deze niet verwijderen, klikt u op de knop **stoppen** in plaats van de knop **verwijderen** , maar bovengenoemde, als u niet de implementatie verwijdert, blijven factureerbare kosten toenemen voor uw implementatie, zelfs als deze is gestopt).

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Installatie van de Azure Toolkit voor Eclips][] 

[Wat is er nieuw in de Azure Toolkit voor Eclips][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Externe toegang inschakelen voor Azure implementaties in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Configuratie-servereigenschappen]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Wat is er nieuw in de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png
