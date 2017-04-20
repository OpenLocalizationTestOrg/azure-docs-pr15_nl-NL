<properties 
   pageTitle="Fouten opsporen in I-SQL-taken | Microsoft Azure" 
   description="Leer hoe U SQL mislukte hoekpunt gebruik van Visual Studio foutopsporing. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>Fouten opsporen in C#-code in I-SQL voor gegevens Lake Analytics-taken 

Informatie over het gebruik van Azure gegevens Lake Visual Studio tools voor foutopsporing mislukte I-SQL-opdrachten vanwege bugs bij te werken binnen gebruikerscode. 

Het hulpmiddel Visual Studio kunt u gecompileerde code en nodig hoekpunt gegevens downloaden uit cluster traceren en fouten opsporen in mislukte taken.

Grote gegevenssystemen bieden meestal uitbreidbaarheidsmodel tot en met talen zoals Java, C#, Python, enzovoort. Veel van deze systemen bieden beperkte runtime foutopsporing informatie, waardoor u moeilijk te runtimefouten opsporen in aangepaste code. De meest recente Visual Studio tools wordt geleverd met een functie, "Hoekpunt foutopsporing is mislukt". Deze functie gebruikt, kunt u downloaden de runtime-gegevens van Azure naar de lokale computer zodat u kunt fouten opsporen in mislukte aangepaste C#-code met dezelfde runtime en exacte invoergegevens vanuit de cloud.  Nadat de problemen worden opgelost, kunt u de gewijzigde code opnieuw uitvoeren in Azure wordt aangegeven van de hulpmiddelen.

Zie [fouten opsporen in uw aangepaste code in Azure gegevens Lake gebruiksanalyses](https://mix.office.com/watch/1bt17ibztohcb)voor een video-presentatie van deze functie.

>[AZURE.NOTE] Visual Studio mogelijk hangen of loopt vast als u niet over de volgende twee vensters upgrades: [Microsoft visuele C++ 2015 distribueren pakket Update 2](https://www.microsoft.com/download/details.aspx?id=51682), [Universal C Runtime voor Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##<a name="prerequisites"></a>Vereisten voor
-   Via het artikel [aan de slag](data-lake-analytics-data-lake-tools-get-started.md) hebt voltooid.

## <a name="create-and-configure-debug-projects"></a>Maken en configureren van foutopsporing projecten

Wanneer u een mislukte taak in gegevens Lake Visual Studio hulpmiddel opent, krijgt u een melding. De gedetailleerde foutgegevens worden weergegeven in het tabblad fout en de gele balk boven aan het venster. 

![Azure gegevens Lake Analytics I-SQL foutopsporing visual studio downloaden hoekpunt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**Downloaden van hoekpunt en een oplossing foutopsporing maakt**

1.  Open een mislukte I-SQL-taak in de Visual Studio.
2.  Klik op **downloaden** om te downloaden van alle vereiste resources en invoer-streams. Klik op **opnieuw** als het downloaden is mislukt.
3.  Klik op **openen** nadat het downloaden is voltooid als een lokale foutopsporing project wilt maken. Een nieuwe Visual Studio-oplossing **VertexDebug** aangeroepen met een leeg project genoemd **LocalVertexHost** wordt gemaakt.

Als de gebruiker gedefinieerde operators in I-SQL-code achter (Script.usql.cs) worden gebruikt, moet u een klasse bibliotheek C#-project maken met de code die door de gebruiker gedefinieerde operators en opnemen van het project in de oplossing VertexDebug.

Als u dll-stroombaan hebt geregistreerd met uw gegevens Lake Analytics-database, moet u de broncode van de stroombaan toevoegen aan de oplossing VertexDebug.
 
Als u een aparte C# klassenbibliotheek voor uw I-SQL-code en de geregistreerde dll-verzamelingen voor uw gegevens Lake Analytics-database hebt gemaakt, moet u het bronproject van de stroombaan C# toevoegen aan de oplossing VertexDebug.

In sommige gevallen kunt u door de gebruiker gedefinieerde operators in I-SQL-code achter (Script.usql.cs)-bestand in de oorspronkelijke oplossing gebruiken. Als u hoe het werkt wilt, moet u een C#-bibliotheek met de broncode maken en wijzig de naam van de constructie in de sectie die is geregistreerd in het cluster. U kunt de naam van de constructie geregistreerd in het cluster door te schakelen van het script die in het cluster hebt uitgevoerd. U kunt doen door het openen van de I-SQL-taak en klikt u op 'script' in het deelvenster aan de taak. 

**Voor het configureren van de oplossing**

1.  Uit Solution explorer met de rechtermuisknop op het C#-project dat u zojuist hebt gemaakt en klik vervolgens op **Eigenschappen**.
2.  Stel het pad uitvoer als LocalVertexHost project mappad werken. U gaat LocalVertexHost project werken mappad via LocalVertexHost eigenschappen.
3.  Het C#-project bouwen om te kunnen plaatst u het bestand .pdb in het project LocalVertexHost werkmap of kunt u het bestand .pdb handmatig kopiëren naar deze map.
4.  Controleer in de **Instellingen voor uitzonderingen**algemene taal Runtime uitzonderingen:

![Azure gegevens Lake Analytics I-SQL foutopsporing visual studio-instelling](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>Fouten opsporen in de taak

Nadat u een oplossing foutopsporing door te downloaden van het hoekpunt hebt gemaakt en de omgeving hebt geconfigureerd, kunt u beginnen voor foutopsporing in uw I-SQL-code.

1.  Uit Solution Explorer met de rechtermuisknop op het **LocalVertexHost** project dat u zojuist hebt gemaakt, wijst u **fouten opsporen in**en klik vervolgens op **nieuw exemplaar Start**. De LocalVertexHost moet worden ingesteld als het project opstarten. Mogelijk ziet u het volgende bericht voor de eerste keer dat u kunt negeren. Het duurt ongeveer een minuut om te gaan naar het scherm foutopsporing.
 
    ![Azure gegevens Lake Analytics I-SQL foutopsporing visual studio waarschuwing](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  Gebruik Visual Studio gebaseerd foutopsporing ervaring (controle, variabelen, enzovoort) naar het probleem op te lossen. 
5.  Nadat u een probleem hebt vastgesteld, de code vast en wijzigt u de C#-project voordat u deze opnieuw testen totdat alle problemen opgelost worden. Na de foutopsporing is voltooid, het uitvoervenster met het volgende bericht 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>De taak opnieuw indienen

Als u klaar bent voor foutopsporing in uw I-SQL-code, kunt u de mislukte taak opnieuw indienen.

1. Nieuwe dll-stroombaan naar uw database ADLA registreren.

    1.  Van Server Explorer/Cloud Explorer in gegevens Lake Visual Studio hulpmiddel Vouw **Databases** 
    2.  Met de rechtermuisknop op stroombaan voor Register samenstellen. 
    3.  Uw nieuwe dll-stroombaan met de database ADLA registreren.
 
2.  Of uw C#-code kopiëren naar script.usql.cs--C#-code achter bestand.
3.  Uw taak opnieuw aan te bieden.

##<a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Aan de slag met Azure gegevens Lake Analytics U SQL-taal](data-lake-analytics-u-sql-get-started.md)
- [Zelfstudie: ontwikkelen I-SQL-scripts met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [I-SQL door de gebruiker gedefinieerde operatoren voor taken van Azure gegevens Lake analyses ontwikkelen](data-lake-analytics-u-sql-develop-user-defined-operators.md)

