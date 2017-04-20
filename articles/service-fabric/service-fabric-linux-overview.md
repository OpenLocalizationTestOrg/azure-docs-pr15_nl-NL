<properties
   pageTitle="Azure-Service stof op Linux | Microsoft Azure"
   description="Service stof clusters ondersteuning Linux en Java, wat inhoudt dat u kunt wel om te implementeren en host Service stof toepassingen geschreven in Java en C# op Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Service stof op Linux

Het voorbeeld van Service stof op Linux kunt u bouwen, en ten zeerste beschikbaar, ten zeerste scalable toepassingen op Linux beheren, net zoals u zou in Windows doen. De Service stof kaders (betrouwbare Services en betrouwbare betrokkenen) zijn beschikbaar in Java op Linux naast C# (.NET Core).  U kunt ook [Gast uitvoerbare services](service-fabric-deploy-existing-app.md) met een taal of framework maken. Bovendien ondersteunt de voorbeeldversie orchestrating Docker containers. Docker containers kunnen Gast uitvoerbare bestanden of systeemeigen Service configuratieservices, waarmee de Service stof kaders uitvoeren.

Service stof op Linux is conceptueel gelijk is aan Service stof op Windows (behalve OS specificaties en programming taal support). De meeste van onze [bestaande documentatie](http://aka.ms/servicefabricdocs) toegepast dus, zodat u vertrouwd raken met de technologie.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Ondersteunde besturingssystemen en talen

De beperkte voorbeeldversie ondersteunt het maken van een en-klare ontwikkeling clusters naast meerdere machines clusters in Azure Ubuntu Server 16.04 uitgevoerd. De voorbeeldversie ondersteunt de betrouwbare betrokkenen en de kaders betrouwbare Stateless Services in Java en C# naast Gast uitvoerbare bestanden en orchestrating Docker containers.  

>[AZURE.NOTE] Betrouwbare verzamelingen worden nog niet ondersteund in Linux. Alleen clusters standaard worden niet ondersteund op - slechts één vak en Azure Linux meerdere machines clusters worden ondersteund in de Preview-versie.

## <a name="supported-tooling"></a>Ondersteund gereedschap

De voorbeeldversie ondersteunt interactie met het cluster via Azure CLI. Voor ontwikkelaars van Java, wordt integratie met Eclips en Yeoman geleverd bij Eclips ondersteund op Linux en klik op OSX. De integratie OSX maakt gebruik van een Linux VM achter de schermen via vagrant. Voor C# ontwikkelaars, integratie met Yeoman geleverd aan Toepassingssjablonen genereren.

## <a name="next-steps"></a>Volgende stappen


1. Vertrouwd raken met de [Betrouwbare betrokkenen](service-fabric-reliable-actors-introduction.md) en [Betrouwbare Services](service-fabric-reliable-services-introduction.md) programming kaders.

2. [Uw ontwikkelomgeving op Linux voorbereiden](service-fabric-get-started-linux.md)

3. [Uw ontwikkelomgeving op OSX voorbereiden](service-fabric-get-started-mac.md)

4. [Uw eerste Service stof Java-toepassing op Linux maken](service-fabric-create-your-first-linux-application-with-java.md)
