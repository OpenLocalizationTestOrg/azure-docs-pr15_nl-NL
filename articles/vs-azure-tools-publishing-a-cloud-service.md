<properties
   pageTitle="Het publiceren van een Cloudservice met de hulpmiddelen Azure | Microsoft Azure"
   description="Meer informatie over het publiceren van Azure cloud serviceprojecten met behulp van Visual Studio."
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

# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Een Cloudservice met de hulpmiddelen Azure publiceren

Met behulp van de Azure-hulpmiddelen voor Microsoft Visual Studio, kunt u uw Azure toepassing rechtstreeks vanuit de Visual Studio publiceren. Visual Studio ondersteunt geïntegreerde publiceren naar de tijdelijke of de productieomgeving van een cloudservice.

Voordat u een Azure-toepassing publiceren kunt, moet u een Azure-abonnement hebben. U moet ook een cloud-service en opslag-account instellen voor gebruik in uw toepassing. U kunt instellen deze bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.IMPORTANT] Wanneer u publiceert, kunt u de implementatieomgeving voor uw cloudservice. U moet ook een opslag-account dat is gebruikt voor het opslaan van de toepassing Inpakken voor implementatie selecteren. Het toepassingspakket is na de installatie verwijderd uit het opslag-account.

Als u ontwikkelen en testen van een Azure-toepassing, kunt u Web implementeren gebruiken om wijzigingen te publiceren stapsgewijs voor uw web-rollen. Nadat u uw toepassing bij een implementatieomgeving publiceert, Web implementeren, kunt u wijzigingen rechtstreeks implementeren in de virtuele machine die de Webrol wordt uitgevoerd. U hoeft niet te Inpakken en uw hele Azure toepassing elke keer dat u wilt bijwerken van uw Webrol om te testen om de wijzigingen te publiceren. Met deze methode kunt u de wijzigingen van web rol beschikbaar hebt in de cloud voor het testen van zonder in afwachting van uw hebt gepubliceerd naar een implementatieomgeving.

Gebruik de volgende procedures uit uw Azure-toepassing publiceren en een Webrol bijwerken met behulp van Web implementeren:

- Publiceren of een Azure-toepassing van Visual Studio Inpakken

- Bijwerken van een Webrol als onderdeel van de ontwikkeling en testen cyclus

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publiceren of een Azure-toepassing van Visual Studio Inpakken

Wanneer u uw Azure-toepassing publiceert, kunt u een van de volgende taken kunt doen:

- Een servicepakket maken: U kunt dit pakket en de configuratie servicebestand publiceren van uw toepassing bij een implementatieomgeving in de [Klassieke Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885).

- Publiceer uw Azure project van Visual Studio: als u wilt publiceren uw toepassing rechtstreeks naar Azure, gebruikt u de Wizard publiceren. Zie voor informatie [Wizard van Azure-toepassing publiceren](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Een servicepakket maken van Visual Studio

1. Wanneer u klaar bent voor uw toepassing publiceren, Solution Explorer openen, opent u het snelmenu voor het Azure project met de rollen en kies publiceren.

1. Als u wilt alleen een servicepakket maken, als volgt:  

  1. Kies in het snelmenu voor het project Azure, **pakket**.

  1. In het dialoogvenster **Pakket Azure-toepassing** , kiest u de configuratie van de service die u wilt een pakket maken en kies vervolgens de opbouwconfiguratie.

  1. (optioneel) Als u wilt inschakelen extern bureaublad voor de cloudservice nadat u de afbeelding publiceert, schakel het selectievakje **Extern bureaublad inschakelen voor alle rollen** en selecteer vervolgens **Instellingen** voor het configureren van extern bureaublad. Als u uw cloudservice foutopsporing wilt nadat u de afbeelding publiceert, schakelt u foutopsporing op afstand door te selecteren van **Externe foutopsporing inschakelen voor alle rollen**.

      Zie [Extern bureaublad met Azure rollen](vs-azure-tools-remote-desktop-roles.md)voor meer informatie.

  1. Als u wilt het pakket maken, kiest u de koppeling **pakket** .

      File Explorer ziet u de bestandslocatie van het zojuist gemaakte pakket. U kunt deze locatie kunt kopiëren, zodat u deze via de [portal van Azure klassieke gebruiken kunt](http://go.microsoft.com/fwlink/?LinkID=213885).

  1. Als u wilt dit pakket publiceren naar een implementatieomgeving, moet u deze locatie als locatie voor de pakket gebruiken wanneer u een cloudservice maken en dit pakket naar een omgeving met [Azure klassieke portal implementeren](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Optioneel) Kies **Annuleren en verwijderen**om de implementatieproces, in het snelmenu voor het artikel in het gebeurtenissenlogboek te annuleren. Hiermee stopt implementatie en Hiermee verwijdert u de implementatieomgeving van Azure.

    >[AZURE.NOTE] Als u wilt deze implementatieomgeving verwijderen nadat deze is geïmplementeerd, moet u de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Optioneel) Nadat uw rol-sessies hebt gestart, weergegeven Visual Studio de implementatieomgeving automatisch in de **Cloud Services** -knooppunt in Server Explorer. Hier ziet u de status van de afzonderlijke rol exemplaren. Zie [Azure beheren van bronnen met Cloud Verkenner](vs-azure-tools-resources-managing-with-cloud-explorer.md). De volgende afbeelding ziet de rol exemplaren terwijl ze nog steeds in de stand Bezig met initialiseren:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Bijwerken van een Webrol als onderdeel van de ontwikkeling en testen cyclus

Als de backend-infrastructuur van uw app stabiele is, maar de rollen web nodig vaker bijwerken, kunt u Web implementeren om te werken alleen een Webrol in uw project. Dit is handig als u niet wilt bouwen en implementeren van de backend werknemer rollen, of als u meerdere web rollen hebt en u wilt bijwerken slechts één van de rollen web.

### <a name="requirements"></a>Vereisten

Dit zijn de vereisten voor het gebruik van Web implementeren voor het bijwerken van uw Webrol:

- **Voor ontwikkelen en testen uitsluitend:** De wijzigingen worden aangebracht rechtstreeks in de virtuele machine waarop de Webrol wordt uitgevoerd. Als deze virtuele machine heeft gerecycled, gaan de wijzigingen verloren omdat het oorspronkelijke pakket dat u hebt gepubliceerd wordt gebruikt om de virtuele machine voor de rol opnieuw te maken. U moet uw toepassing voor de meest recente wijzigingen voor de Webrol opnieuw publiceren.

- **Alleen web rollen kunnen worden bijgewerkt:** Werknemer rollen kunnen niet worden bijgewerkt. Bovendien kunt u de RoleEntryPoint in web role.cs niet bijwerken.

- **Slechts één exemplaar van een Webrol ondersteunt:** U kunt meerdere exemplaren van een Webrol geen in uw implementatieomgeving. Meerdere web rollen elke met slechts één exemplaar worden echter wel ondersteund.

- **Moet u verbindingen met extern bureaublad inschakelen:** Dit is vereist zodat Web implementeren de beschikking over de gebruiker en het wachtwoord verbinding maken met de virtuele machine implementeren van de wijzigingen op de server waarop Internet Information Services (IIS) wordt uitgevoerd. Daarnaast moet u mogelijk verbinding maken met de virtuele machine een vertrouwd certificaat toevoegen aan IIS op deze computer virtuele. (Dit zorgt ervoor dat de verbinding met extern voor IIS die wordt gebruikt door het implementeren van Web beveiligd is.)

De volgende procedure wordt ervan uitgegaan dat u de wizard **Publiceren Azure-toepassing gebruikt** .

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Als u wilt inschakelen Web implementeren wanneer u uw toepassing publiceren

1. Als u wilt inschakelen op het **Web implementeren inschakelen** voor alle rollen selectievakje voor het web, moet u eerst verbindingen met extern bureaublad configureren. Selecteer **Extern bureaublad inschakelen** voor alle rollen en geef vervolgens de referenties die wordt gebruikt om verbinding te op afstand in het vak **Configuratie voor extern bureaublad** dat wordt weergegeven. Zie [Extern bureaublad met Azure rollen](vs-azure-tools-remote-desktop-roles.md) voor meer informatie.

1. Als u de Web implementeren voor de web-rollen in uw toepassing, selecteert u **Web implementeren inschakelen voor alle web rollen**.

    Een gele waarschuwing driehoek wordt weergegeven. Web Deploy standaard een niet-vertrouwde zelfondertekend certificaat, die niet geschikt is voor het uploaden van vertrouwelijke gegevens. Als u beveiligen van deze procedure voor gevoelige gegevens wilt, kunt u een SSL-certificaat moet worden gebruikt voor Web implementeren verbindingen toevoegen. Dit certificaat moet een vertrouwd certificaat. Zie voor informatie over hoe u dit doet, de sectie **Naar zorg Web implementeren Secure** verderop in dit onderwerp.

1. Kies **volgende** om weer te geven van het scherm **Samenvatting** , en kies vervolgens **publiceren** naar de cloudservice implementeren.

    De cloudservice is gepubliceerd. De virtuele machine die is gemaakt, heeft externe verbindingen ingeschakeld voor IIS zodat Web implementeren, kunnen worden gebruikt om het bijwerken van uw web-rollen zonder deze opnieuw te publiceren.

    >[AZURE.NOTE] Als er meer dan één exemplaar dat is geconfigureerd voor de rol van een webpagina, een waarschuwing ziet er verschijnt dat elke Webrol worden beperkt tot één exemplaar alleen in het pakket dat wordt gemaakt als u wilt uw toepassing publiceren. Selecteer **OK** om door te gaan. Zoals aangegeven in het gedeelte vereisten, kunt u meer dan één Webrol maar slechts één exemplaar van elke rol hebben.

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Als u wilt bijwerken van uw Webrol met behulp van Web implementeren

1. Als u wilt gebruiken met het Web implementeren, kunt u codewijzigingen aanbrengen door het project naar een of meer van uw web-rollen in Visual Studio die u wilt publiceren, en klik vervolgens met de rechtermuisknop op dit projectknooppunt in uw oplossing en wijs **publiceren**. Het dialoogvenster **Web publiceren** wordt weergegeven.

1. (Optioneel) Als u een vertrouwde SSL-certificaat wilt gebruiken voor externe verbindingen voor IIS hebt toegevoegd, kunt u het selectievakje **toestaan niet worden vertrouwd certificaat** wissen. Zie de sectie **Naar zorg Web implementeren Secure** verderop in dit onderwerp voor informatie over het toevoegen van een certificaat Web implementeren om veilig te maken.

1. Als u wilt gebruiken met het Web implementeren, moet de publiceren om de gebruikersnaam en het wachtwoord dat u vooraf voor uw verbinding met extern bureaublad instelt wanneer u het pakket voor het eerst hebt gepubliceerd.

  1. Typ de naam van de gebruiker in het vak **gebruikersnaam in te voeren**.

  1. Voer in het vak **wachtwoord**het wachtwoord.

  1. (Optioneel) Als u dit wachtwoord opslaan in dit profiel wilt, kiest u het **wachtwoord opslaan**.

1. Als u de wijzigingen wilt uw Webrol publiceren, kies **publiceren**.

    De statusregel publiceren waarde **gestart**wordt weergegeven. Wanneer de publicatie is voltooid, **publiceren is voltooid** wordt weergegeven. De wijzigingen zijn nu aan de Webrol op uw computer virtuele geïmplementeerd. Nu kunt u uw Azure-toepassing starten in de Azure-omgeving om uw wijzigingen te testen.

### <a name="to-make-web-deploy-secure"></a>Naar het beveiligen van Web implementeren

1. Web Deploy standaard een niet-vertrouwde zelfondertekend certificaat, die niet geschikt is voor het uploaden van vertrouwelijke gegevens. Als u beveiligen van deze procedure voor gevoelige gegevens wilt, kunt u een SSL-certificaat moet worden gebruikt voor Web implementeren verbindingen toevoegen. Dit certificaat moet een vertrouwd certificaat, die u van een certificeringsinstantie (CA ontvangt).

    U kunt uw Web implementeren beveiligen voor elke virtuele machine voor elk van uw web-rollen, moet u de vertrouwde certificaat dat u wilt gebruiken voor web uploaden naar de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)implementeren. Dit zorgt ervoor dat het certificaat is toegevoegd aan de virtuele machine die voor de web-functie wordt gemaakt wanneer u uw toepassing publiceren.

1. Als u wilt een vertrouwde SSL-certificaat toevoegen aan IIS voor externe verbindingen wilt gebruiken, de volgende stappen uit:

  1. Als u wilt verbinden met de virtuele machine die de Webrol wordt uitgevoerd, selecteert u het exemplaar van de Webrol in de **Cloud Explorer** of **Server Explorer**en kies vervolgens de opdracht **verbinding maken met extern bureaublad** . Zie [Extern bureaublad met Azure rollen](vs-azure-tools-remote-desktop-roles.md)voor meer informatie over het verbinding maken met de virtuele machine.

      Uw browser wordt u gevraagd te downloaden een. RDP-bestand.

  1. Als u wilt een SSL-certificaat hebt toegevoegd, opent u de management-service in IIS-beheer. SSL in IIS-beheer inschakelen via de koppeling **Bindingen** in het deelvenster **actie** . Het dialoogvenster **Site Binding toevoegen** wordt weergegeven. Kies **toevoegen**en kiest u HTTPS in de vervolgkeuzelijst **Type** . Kies het SSL-certificaat dat u had ondertekend door een Certificeringsinstantie en dat u hebt geüpload naar de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)in de lijst **SSL-certificaat** . Zie de [Verbindingsinstellingen voor de Management-Service configureren](http://go.microsoft.com/fwlink/?LinkId=215824)voor meer informatie.

      >[AZURE.NOTE] Als u een vertrouwde SSL-certificaat toevoegt, wordt de gele waarschuwing driehoek niet langer weergegeven in de **Wizard publiceren**.

## <a name="include-files-in-the-service-package"></a>Bestanden opnemen in de servicepakket

Mogelijk moet u een specifieke bestanden opnemen in uw-pakket, zodat ze zijn beschikbaar op de virtuele machine die is gemaakt voor een rol. U wilt bijvoorbeeld een .exe of een MSI-bestand dat wordt gebruikt door een opstartscript naar uw-pakket toevoegen. Of moet u mogelijk een constructie die moeten worden een webproject rol of werknemer rol toevoegen. Bestanden die ze moeten worden toegevoegd aan de oplossing voor uw Azure toepassing opnemen.

### <a name="to-include-files-in-the-service-package"></a>Bestanden in de servicepakket opnemen

1. Voeg een constructie aan een servicepakket via de volgende stappen uit:

  1. Open het projectknooppunt voor het project dat de waarnaar wordt verwezen constructie ontbreekt in **Solution Explorer** .

  1. Als u wilt de vergadering toevoegen aan het project, opent u het snelmenu voor de map **verwijzingen** en kies vervolgens **Add Reference**. Het dialoogvenster toevoegen wordt weergegeven.

  1. Kies de verwijzing die u wilt toevoegen en kies vervolgens de knop **OK** .

      De verwijzing wordt toegevoegd aan de lijst onder de map **verwijzingen** .

  1. Open het snelmenu voor de vergadering die u hebt toegevoegd en kies **Eigenschappen**. Het venster **Eigenschappen** wordt weergegeven.

      Als u wilt opnemen kiest u deze constructie in de servicepakket, in de **lokale kopie lijst** **waar**.

1. Open het projectknooppunt voor het project dat de waarnaar wordt verwezen constructie ontbreekt in **Solution Explorer** .

1. Als u wilt de vergadering toevoegen aan het project, opent u het snelmenu voor de map **verwijzingen** en kies vervolgens **Add Reference**. Het dialoogvenster **Toevoegen** wordt weergegeven.

1. Kies de verwijzing die u wilt toevoegen en kies vervolgens de knop **OK** .

    De verwijzing wordt toegevoegd aan de lijst onder de map **verwijzingen** .

1. Open het snelmenu voor de vergadering die u hebt toegevoegd en kies **Eigenschappen**. Het venster Eigenschappen wordt weergegeven.

1. Als u wilt deze constructie opnemen in de servicepakket, in de **Lokale kopie** lijst, kiest u **waar**.

1. Om bestanden in de servicepakket die zijn toegevoegd aan uw project van de rol web, opent u het snelmenu voor het bestand en kies vervolgens **Eigenschappen**. Kies **inhoud** in de keuzelijst **Actie maken** vanuit het venster **Eigenschappen** .

1. Om bestanden in de servicepakket die zijn toegevoegd aan uw project van de rol werknemer, opent u het snelmenu voor het bestand en kies vervolgens **Eigenschappen**. Kies **als nieuwere kopiëren** in de keuzelijst **naar uitvoer directory kopiëren** vanuit het venster **Eigenschappen** .

## <a name="next-steps"></a>Volgende stappen

Meer informatie over publiceren naar Azure van Visual Studio, raadpleegt u de [Wizard van Azure-toepassing publiceren](vs-azure-tools-publish-azure-application-wizard.md).
