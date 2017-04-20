<properties 
    pageTitle="Downloaden van de Azure SDK for Java" 
    description="Informatie over het downloaden van de Azure SDK for Java, met steekproef-code die zijn opgegeven voor Maven projecten en eenvoudige installatiestappen voor de Tookit Azure voor Eclips." 
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

# <a name="download-the-azure-sdk-for-java"></a>Downloaden van de Azure SDK for Java

In dit artikel bevat instructies voor het downloaden en installeren van de Azure-bibliotheken voor Java.

**Notitie:** De bibliotheken Azure voor Java worden gedistribueerd onder de [Apache licentie, versie 2.0][license].

## <a name="azure-libraries-for-java---manual-download"></a>Azure bibliotheken voor Java - handmatig downloaden

Handmatig installeren van de Azure-bibliotheken voor Java, klikt u op <http://go.microsoft.com/fwlink/?LinkId=690320> om een zipbestand bevat alle bibliotheken en alle afhankelijkheden te downloaden.

Nadat u het zip-bestand naar uw computer hebt gedownload, de inhoud te extraheren en gebruik een van de volgende opties om de oppervlak-bestanden toevoegen aan uw project:

* Het oppervlak-bestanden importeren in uw project Java in Eclips.

* Configureer het **Pad maken** voor uw project Java in Eclips naar Neem het pad naar de oppervlak-bestanden.

Zie het artikel [Java bouwen pad] op de website van Eclips voor gedetailleerde informatie over het instellen van opbouwen paden in Eclips.

**Notitie:** Zie de license.txt en ThirdPartyNotices.txt-bestand in het ZIP voor licentie en andere informatie.

## <a name="azure-libraries-for-java---building-with-maven"></a>Azure bibliotheken voor Java - bouwen met Maven

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Stap 1: uw project instellen voor Maven gebruiken voor opbouwen

Maven om projecten te maken in Eclips welke gebruik van de Azure bibliotheken voor Java, volgens de instructies in de [Aan de slag met Azure Management bibliotheken voor Java] [ maven-getting-started] artikel. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Stap 2: de instellingen van uw Maven met de vereiste afhankelijkheden configureren

Wanneer uw project is geconfigureerd voor Maven gebruiken om te worden gemaakt, kunt u toevoegen de de vereiste afhankelijkheden aan uw pom.xml-bestand met syntaxis, zoals in het volgende voorbeeld. Houd er rekening mee dat u wilt toevoegen van elke afhankelijkheid dat wordt vermeld in het volgende voorbeeld. u moet alleen de specifieke afhankelijkheden waarvoor uw project toevoegen.

> [AZURE.NOTE] Binnen elke `<version>` element in het volgende voorbeeld wordt de 'n.n.n' tijdelijke aanduidingen vervangen in dit voorbeeld geldige versienummers, die kan worden verkregen uit de [Bibliotheek van Azure-bibliotheken op Maven].

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Installatie van de Azure Toolkit voor Eclips

In deze sectie bevat basisinstructies voor het installeren van de Azure Toolkit voor Eclips; Zie de [installatie van de Azure Toolkit voor Eclips]voor gedetailleerde instructies.

### <a name="prerequisites"></a>Vereisten voor

1. Windows operting-systemen vermeld in het artikel [Wat is er nieuw in de Toolkit Azure voor Eclips] .
1. Macintosh- of Linux operting systemen vermeld in het artikel [Wat is er nieuw in de Toolkit Azure voor Eclips] .
1. IDE Eclips voor Java EE ontwikkelaars, Indigo of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>.

### <a name="basic-installation-steps"></a>Basisstappen voor installatie

1. Selecteer in het menu **Help** in Eclips, **Nieuwe Software installeren**.
1. Voer de locatie van site <http://dl.microsoft.com/eclipse> en druk op **Enter**.
1. Selecteer de items worden geïnstalleerd en klik op **Voltooien**.

De Toolkit Azure voor Eclips wordt gebruikt voor de meest recente versie van de Azure-SDK. Dit kan worden gedownload met het installatieprogramma van Web Platform (WebPI) op <http://go.microsoft.com/fwlink/?LinkID=252838>. Echter als u niet geïnstalleerd hebt, wordt wanneer u uw eerste Azure-implementatieproject, de Toolkit Azure voor Eclips maakt automatisch geïnstalleerd de juiste versie van de Azure-SDK.

## <a name="see-also"></a>Zie ook

[Azure Toolkit voor Eclips]

[Installatie van de Azure Toolkit voor Eclips] 

[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure bibliotheken bibliotheek op Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java opbouwen pad]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Wat is er nieuw in de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=690333
