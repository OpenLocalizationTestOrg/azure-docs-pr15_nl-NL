<properties
   pageTitle="Beheren van uw toepassingen in Visual Studio | Microsoft Azure"
   description="Gebruik Visual Studio kunt maken, ontwikkelen, inpakken, implementeren en fouten opsporen in uw Service stof toepassingen en -services."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="seanmck;mikhegn"/>

# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Gebruik Visual Studio om te schrijven en beheren van uw Service stof toepassingen vereenvoudigen

U kunt uw stof van Azure-Service-toepassingen en -services via Visual Studio beheren. Zodra u [uw ontwikkelomgeving instellen hebt](service-fabric-get-started.md), kunt u Visual Studio Service stof toepassingen maken, services of pakket, register kunt toevoegen en implementeren van toepassingen in uw cluster plaatselijke ontwikkeling.

## <a name="deploy-your-service-fabric-application"></a>Uw Service stof-toepassing implementeren

Standaard is de volgende stappen uit in één bewerking combineert implementeren van een toepassing:

1. De toepassingspakket maken
2. Het toepassingspakket van de uploaden naar de afbeelding van de store
3. Het toepassingstype registreren
4. Als u een actieve toepassingsexemplaren
5. Een nieuw toepassingsexemplaar maken

In Visual Studio, wordt drukt u op **F5** ook uw toepassing implementeren en foutopsporing als bijlage toevoegen aan alle toepassingsexemplaren. Met **Ctrl + F5** kunt u een toepassing implementeren zonder foutopsporing, of u kunt publiceren naar een lokale of externe cluster met behulp van het profiel publiceren. Zie [publiceren een toepassing aan een externe cluster met behulp van Visual Studio](service-fabric-publish-app-remote-cluster.md)voor meer informatie.

### <a name="application-debug-mode"></a>Toepassing foutopsporingsmodus

Standaard Visual Studio bestaande exemplaren van uw toepassingstype verwijderd wanneer u stopt foutopsporing of (als u geïmplementeerd de app zonder te koppelen foutopsporing), wanneer u de toepassing opnieuw distribueren. In dat geval wordt de gegevens van de toepassing verwijderd. Terwijl lokaal foutopsporing, u kunt gegevens die u al hebt gemaakt bij het testen van een nieuwe versie van de toepassing behouden, moet blijven van de actieve toepassing of u wilt dat latere foutopsporing sessies voor het upgraden van de toepassing. Visual Studio Service stof Tools bieden een eigenschap met de naam van de **Toepassing fouten opsporen in modus**, welke besturingselementen of de **F5** u de toepassing verwijdert moet, de toepassing uitgevoerd blijven nadat een sessie voor foutopsporing eindigt of de toepassing worden bijgewerkt op de volgende foutopsporing sessies, in plaats van verwijderd en opnieuw geïmplementeerd inschakelen.

#### <a name="to-set-the-application-debug-mode-property"></a>De eigenschap toepassing fouten opsporen in modus instellen

1. Kies **Eigenschappen** in snelmenu van de toepassingsproject (of druk op **F4** ).
2. Stel de eigenschap **Toepassing fouten opsporen in modus** in het venster **Eigenschappen** .

    ![Eigenschap Application foutopsporing-modus instellen][debugmodeproperty]

Hierna ziet u de **Toepassing fouten opsporen in modus** van beschikbare opties.

1. **Automatisch upgraden**: de toepassing blijft worden uitgevoerd wanneer de foutopsporingssessie beëindigd. De volgende **F5** wordt behandeld de implementatie van een upgrade met behulp van niet-beheerde automatisch modus upgrade snel de toepassing naar een nieuwere versie van met een datumtekenreeks toegevoegd. Het upgradeproces behoudt alle gegevens die u hebt ingevoerd in een eerdere foutopsporingssessie.

2. **Toepassing houden**: de toepassing in het cluster blijft uitgevoerd wanneer de foutopsporingssessie beëindigd. Klik op de volgende **F5** de toepassing wordt verwijderd en de zojuist gemaakte toepassing wordt geïmplementeerd in het cluster.

3. **Toepassing verwijderen** wordt de toepassing moeten worden verwijderd wanneer de foutopsporingssessie beëindigd.

Voor het **Upgraden van automatisch** gegevens bewaard door de upgrade mogelijkheden van de toepassing van Service stof toe te voegen, maar deze is afgestemd om te optimaliseren voor prestaties in plaats van beveiliging. Zie [Service stof toepassing een upgrade uitvoeren](service-fabric-application-upgrade.md)voor meer informatie over het upgraden van toepassingen en hoe u een upgrade kunt uitvoeren in een echte-omgeving.

![Voorbeeld van de nieuwe versie van de toepassing met datum toegevoegd][preservedata]

>[AZURE.NOTE] Deze eigenschap bestaat niet vóór versie 1.1 van de Service stof hulpprogramma's voor Visual Studio. Gebruik de eigenschap **Gegevens behouden op Start** om te bereiken hetzelfde gedrag gezien vóór 1.1. De optie "Toepassing behouden" is geïntroduceerd in versie 1.2 van de Service stof hulpprogramma's voor Visual Studio.

## <a name="add-a-service-to-your-service-fabric-application"></a>Een service toevoegen aan uw Service stof-toepassing

U kunt nieuwe services toevoegen aan uw toepassing om uit te breiden functioneert.  Om ervoor te zorgen dat de service is opgenomen in uw toepassingspakket, moet u de service tot en met het **Nieuwe stof Service...** menu-item toevoegen.

![Een nieuwe stof service toevoegen aan uw toepassing][newservice]

Selecteer een type Service stof project om toe te voegen aan uw toepassing en geef een naam voor de service.  Zie [een kader voor uw service kiezen](service-fabric-choose-framework.md) kunt u bepalen welk servicetype gebruiken.

![Selecteer een type stof Service project om toe te voegen aan uw toepassing][addserviceproject]

De nieuwe service worden, toegevoegd aan uw oplossing en de bestaande toepassingspakket. De Serviceverwijzingen en een standaardexemplaar service worden, toegevoegd aan manifest van de toepassing. De service worden gemaakt en de volgende keer dat u de toepassing implementeren gestart.

![De nieuwe service worden, toegevoegd aan uw toepassingsmanifest][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Uw Service stof-toepassing Inpakken

Als u wilt implementeren de toepassing en de bijbehorende services op een cluster, moet u een toepassingspakket maken.  Het pakket organiseert manifest van de toepassing, service manifest(s) en andere benodigde bestanden in een specifieke indeling.  Visual Studio ingesteld en beheerd door het pakket in de map van het toepassingsproject, in de map 'pak'.  **Pakket** klikken in het contextmenu van de **toepassing** wordt gemaakt of het toepassingspakket wordt bijgewerkt.  U kunt dit doen als u de toepassing implementeren met behulp van aangepaste PowerShell-scripts.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Toepassingen en toepassingstypen via Cloud Verkenner verwijderen

U kunt eenvoudige cluster management bewerkingen uit in Visual Studio via Cloud Verkenner, die u vanuit het menu **Beeld starten kunt** kunt uitvoeren. U kunt bijvoorbeeld toepassingen verwijderen en de toepassingstypen op lokale of externe clusters inrichting.

![Verwijderen van een toepassing](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Zie [uw cluster met Service stof Explorer visualiseren](service-fabric-visualizing-your-cluster.md)voor uitgebreidere cluster management-functionaliteit.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

- [Service stof toepassingsmodel](service-fabric-application-model.md)
- [Installatie van service stof toepassingen](service-fabric-deploy-remove-applications.md)
- [Parameters voor toepassingen voor meerdere omgevingen beheren](service-fabric-manage-multiple-environment-app-configuration.md)
- [Voor foutopsporing in uw toepassing Service stof](service-fabric-debugging-your-application.md)
- [Uw cluster visualiseren met behulp van de Service stof Explorer](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
