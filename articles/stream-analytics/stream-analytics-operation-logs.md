<properties 
    pageTitle="Fouten opsporen in bewerking te gebruiken en service Logboeken in Stream Analytics | Microsoft Azure" 
    description="Hoe kan ik gebruik Stream Analytics bewerking Logboeken" 
    keywords="Service-Logboeken"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Fouten opsporen in Stream Analytics-taken met Logboeken aan de service en bewerking

Alle Azure services opgeven operationele logboekregistratie berichten naar gebruikers recorddetails die betrekking hebben op beheertaken uit te voeren. Klik in Azure Stream Analytics, deze informatie kan worden gebruikt voor foutopsporing zoals taakstatus, voortgang van het project, weergeven en foutberichten voor het bijhouden van de voortgang van een taak na verloop van tijd vanaf begin met het verwerking om uit te voeren.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Zoeken naar bewerking Logboeken in de beheerportal van Azure

Logboeken aan de bewerking u kunt op twee manieren openen:  

- Dashboard van de Stream Analytics-taak  
- Management-Services in de klassieke Azure-portal  

## <a name="dashboard-of-the-stream-analytics-job"></a>Dashboard van de Stream Analytics-taak

Een koppeling naar de bijbehorende logboekbestanden van een taak Stream Analytics wordt weergegeven op het tabblad van de Dashboard van de taak. Als u op deze koppeling klikt, wordt deze de filters instellen op een manier die of het meest recente logboeken voor die specifieke taak wordt weergegeven.

  ![Selecteer Management Services-Logboeken](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Management-Services

Om handmatig naar de logboeken aan de bewerking voor gaan Stream analyses en andere services in de klassieke Azure-portal:

1.  Klik op **Management Services** in de [klassieke Azure-portal](https://manage.windowsazure.com).
2.  Selecteer **Stream Analytics** voor **Type** en de naam van de taak voor **De naam van de Service**.  

  ![Selecteer Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Controlelogboeken bijhouden vinden in de portal van Azure ##

Als u wilt zoeken operationele logboeken voor uw Stream Analytics-taak in de portal van Azure, klikt u op **Bladeren** en selecteer vervolgens **controlelogboeken**.

  ![Azure-portal Stream Analytics selecteren](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Hiermee opent u een blade met gebeurtenissen van de laatste zeven dagen voor alle resources in uw abonnement.  U kunt filteren om gebeurtenissen te bekijken van een type opgeven of een bepaald tijdsbestek door te klikken op de opdracht **Filter** .

  ![Azure-portal Stream Analytics selecteren](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Log informatie

U kunt filteren op tijdsbereik en de Status om weer te geven van de logboeken voor uw taak.

Klik op de knop **Details** onderaan in het venster meer details over een geselecteerde gebeurtenis weergeven in de portal Azure Management. 

  ![Selecteer Details](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Klik in de Azure-portal op een logboek om de gedetailleerde gebeurtenissen hierin weer te geven.

  ![Azure-portal Details selecteren](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Hierin kunt u het blad **Detail** openen door te klikken op de gebeurtenis.

  ![Azure-portal Details selecteren](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Fouten opsporen in een mislukte taak

Klik op het pictogram zoeken en het type 'mislukte' in de portal Azure Management. Het resultaat is alle logboeken met fouten als resultaat. 

  ![Voor foutopsporing in een mislukte taak](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Klik in de portal Azure kunt u filteren op niveau van bericht **kritiek** gebeurtenissen weergeven.

  ![Azure portal foutopsporing](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

U kunt een van de fouten selecteren en klik op de **Details** voor meer informatie over de fout.  Enkele foutberichten bevatten ook informatie over het beperken van het probleem. 

  ![Details van bewerking](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Als u wilt contact opnemen met [ondersteuning](https://azure.microsoft.com/support/options/) of bevatten informatie aan het team via het [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), Opmerking: de Details van de bewerking, specifiek de **Correlatie-ID**. 

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
