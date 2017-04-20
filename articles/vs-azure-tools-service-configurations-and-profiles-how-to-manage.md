<properties
   pageTitle="Het beheren van configuraties en profielen | Microsoft Azure"
   description="Meer informatie over het werken met bestanden die service configuraties en profielen configuratie | welke instellingen voor de implementatieomgevingen opslaan en publiceren instellingen voor cloudservices."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-manage-service-configurations-and-profiles"></a>Het beheren van configuraties en profielen

## <a name="overview"></a>Overzicht

Wanneer u een cloudservice publiceert, Visual Studio configuratiegegevens opgeslagen in de twee soorten configuratiebestanden: service-configuraties en profielen. Instellingen voor de implementatieomgevingen voor een Azure cloudservice opgeslagen met configuraties (.cscfg-bestanden). Azure gebruikt deze configuratiebestanden bij het beheren van uw cloudservices. Aan de andere kant profielen (.azurePubxml-bestanden) store publicatie-instellingen voor cloudservices. Deze instellingen zijn een record van wat u kiezen wanneer u de wizard Publiceren gebruikt, en lokaal worden gebruikt door Visual Studio. Dit onderwerp wordt uitgelegd hoe u werkt met beide soorten configuratiebestanden.

## <a name="service-configurations"></a>Configuraties

U kunt meerdere configuraties voor elk van uw implementatieomgevingen maken. U kunt bijvoorbeeld een service te configureren voor de lokale omgeving waarmee u kunt uitvoeren en een Azure-toepassing en een andere service te configureren voor uw productieomgeving testen maken.

U kunt toevoegen, verwijderen, naam wijzigen en deze serviceconfiguraties op basis van uw vereisten voor wijzigen. U kunt deze configuraties beheren in Visual Studio, zoals wordt weergegeven in de volgende afbeelding.

![Configuraties beheren](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

U kunt ook het dialoogvenster **Configuraties beheren** openen vanuit de eigenschappenvensters van de rol. Als de eigenschappen voor een rol in uw project Azure, opent u het snelmenu voor die rol en kies vervolgens **Eigenschappen**. Klik op het tabblad **Instellingen** Vouw de lijst **Service te configureren** en selecteer vervolgens **beheren** om het dialoogvenster **Configuraties beheren** te openen.

### <a name="to-add-a-service-configuration"></a>De configuratie van een service toevoegen

1. Open het snelmenu voor het project Azure in Oplossingverkenner en selecteer **Configuraties beheren**.

    Het dialoogvenster **Configuraties beheren** wordt weergegeven.

1. Als u wilt toevoegen van een service te configureren, moet u een kopie van een bestaande configuratie maken. Kies de configuratie die u wilt kopiëren in de lijst naam en selecteer vervolgens **de kopie maken**Klik hiertoe.

1. (Optioneel) Naar een andere naam voor de serviceconfiguratie opgeven, kiest u de configuratie van de nieuwe service uit de lijst naam en selecteer vervolgens **naam wijzigen**. Typ in het tekstvak **naam** de naam die u wilt gebruiken voor deze service te configureren en selecteer vervolgens **OK**.

    Een nieuwe service configuratiebestand met de naam ServiceConfiguration. [Naam van nieuwe] .cscfg wordt toegevoegd aan het Azure project in Solution Explorer.


### <a name="to-delete-a-service-configuration"></a>Verwijderen van een service te configureren

1. Open het snelmenu voor het project Azure in Oplossingverkenner en selecteer **Configuraties beheren**.

    Het dialoogvenster **Configuraties beheren** wordt weergegeven.

1. Als u wilt verwijderen van een service te configureren, kiest u de configuratie die u wilt verwijderen uit **de lijst** en selecteer vervolgens **verwijderen**. Er verschijnt een dialoogvenster om te bevestigen dat u wilt deze configuratie verwijderen.

1. Selecteer **verwijderen**.

     Het configuratiebestand service is verwijderd uit het Azure project in Solution Explorer.


### <a name="to-rename-a-service-configuration"></a>Om de naam van een service te configureren te

1. Open het snelmenu voor het project Azure in Solution Explorer en selecteer vervolgens **Configuraties beheren**.

    Het dialoogvenster **Configuraties beheren** wordt weergegeven.

1. De naam van een service te configureren, de configuratie van de nieuwe service kiezen uit **de lijst** en selecteer vervolgens **naam wijzigen**. Klik in het tekstvak **naam** Typ de naam die u wilt gebruiken voor deze service te configureren en selecteer vervolgens **OK**.

    De naam van het configuratiebestand service wordt gewijzigd in het Azure project in Solution Explorer.

### <a name="to-change-a-service-configuration"></a>Als u wilt wijzigen van een service te configureren

- Als u wijzigen van een service te configureren wilt, opent u het snelmenu voor de specifieke rol die u wilt wijzigen in het Azure project en selecteer vervolgens **Eigenschappen**. Zie [hoe: de rollen configureren voor een Azure-Cloudservice met Visual Studio](https://msdn.microsoft.com/library/azure/hh369931.aspx) voor meer informatie.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Verschillende combinaties maken met behulp van profielen

Met behulp van een profiel, kunt u automatisch doorvoeren in de **Wizard publiceren** met verschillende combinaties van instellingen voor verschillende doeleinden. Bijvoorbeeld, kunt u één standaardprofiel voor foutopsporing hebt en een andere versie genereert. In dat geval uw profiel **Foutopsporing** zou hebben **IntelliTrace** ingeschakeld en de configuratie **fouten opsporen in** is geselecteerd en uw profiel **Release** moet **IntelliTrace** uitgeschakeld en de **Release** -configuratie die is geselecteerd. U kunt ook verschillende profielen om te implementeren van een service met een ander opslag-account.

Wanneer u de wizard voor de eerste keer uitvoert, wordt een standaardprofiel gemaakt. Visual Studio slaat het profiel in een bestand met een extensie .azurePubXml, die wordt toegevoegd aan uw project Azure onder de map **profielen** . Als u verschillende mogelijkheden handmatig opgeven wanneer u de wizard later uitvoert, wordt het bestand automatisch bijgewerkt. Voordat u de volgende procedure uitvoert, moet u al hebt gepubliceerd uw cloudservice ten minste eenmaal.

### <a name="to-add-a-profile"></a>Een profiel toevoegen

1. Open het snelmenu voor uw Azure project en selecteer vervolgens **publiceren**.

1. Selecteer de knop **Profiel opslaan** als de volgende afbeelding ziet naast de lijst **doelprofiel** . Hiermee wordt een profiel voor u gemaakt.

    ![Een nieuw profiel maken](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Nadat het profiel is gemaakt, selecteert u **< beheren >** in de lijst **doelprofiel** .

    Het dialoogvenster **Profielen beheren** wordt weergegeven, als de volgende afbeelding ziet.

    ![Dialoogvenster profielen beheren](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. Klik in **de lijst** Kies een profiel en selecteer vervolgens een **Kopie maken**.

1. Kies de knop **sluiten** .

    Het nieuwe profiel wordt weergegeven in de doellijst profiel.

1. Selecteer het profiel dat u zojuist hebt gemaakt in de lijst **doelprofiel** . De instellingen van de Wizard publiceren worden gevuld met de opties in het profiel dat u hebt geselecteerd.

1. Selecteer de knoppen **vorige** en **volgende** om weer te geven van elke pagina van de Wizard publiceren en pas dit vervolgens aan de instellingen voor dit profiel. Zie [Publiceren Azure toepassing Wizard](http://go.microsoft.com/fwlink/p/?LinkID=623085) voor informatie.

1. Als u klaar bent met het aanpassen van de instellingen, selecteer **volgende** om terug te gaan naar de pagina instellingen. Het profiel wordt opgeslagen wanneer u de service publiceert met behulp van deze instellingen of als u **Opslaan** naast de lijst met profielen kiest.

### <a name="to-rename-or-delete-a-profile"></a>Naam wijzigen of verwijderen van een profiel

1. Open het snelmenu voor uw Azure project en selecteer vervolgens **publiceren**.

1. Selecteer in de lijst **doelprofiel** **beheren**.

1. In het dialoogvenster **Profielen beheren** , selecteer het profiel dat u wilt verwijderen en selecteer vervolgens **verwijderen**.

1. Selecteer **OK**in het dialoogvenster bevestiging dat wordt weergegeven.

1. Selecteer **sluiten**.

### <a name="to-change-a-profile"></a>Een profiel wijzigen

1. Open het snelmenu voor uw Azure project en selecteer vervolgens **publiceren**.

1. Selecteer het profiel dat u wilt wijzigen in de lijst **doelprofiel** .

1. Selecteer de knoppen **vorige** en **volgende** om weer te geven van elke pagina van de Wizard publiceren en wijzig vervolgens de gewenste instellingen. Zie [Publiceren Azure toepassing Wizard](http://go.microsoft.com/fwlink/p/?LinkID=623085) voor informatie.

1. Als u klaar bent met het wijzigen van de instellingen, selecteer **volgende** om terug te gaan naar de pagina **Instellingen** .

1. (Optioneel) Selecteer **publiceren** naar de cloudservice met de nieuwe instellingen publiceren. Als u niet wilt dat uw cloudservice op dit moment publiceren en u de Wizard publiceren sluit, Visual Studio wordt u gevraagd of u wilt de wijzigingen in het profiel opslaan.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het configureren van andere delen van uw Azure project van Visual Studio, Zie [een Project Azure configureren](http://go.microsoft.com/fwlink/p/?LinkID=623075)
