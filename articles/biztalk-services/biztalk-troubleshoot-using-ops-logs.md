<properties 
    pageTitle="Problemen met BizTalk-Services met bewerking Logboeken | Microsoft Azure" 
    description="Problemen oplossen met bewerking logboeken BizTalk-Services. MAB, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: Problemen met de bewerking Logboeken

## <a name="what-are-the-operation-logs"></a>Wat zijn de logboeken aan de bewerking
Logboeken aan de bewerking is een Management Services-functie beschikbaar is in de portal van Azure klassieke waarmee u kunt de weergave van historische logboeken bewerkingen uitgevoerd op uw Azure services, met inbegrip van BizTalk-Services. Hiermee kunt u historische gegevens betrekking hebben op beheertaken uit te voeren voor uw abonnement op BizTalk Service tot 180 dagen weergeven.

> [AZURE.NOTE]Deze functie alleen wordt vastgelegd logboeken van het beheer van BizTalk-Services, zoals waarop de service is gestart, back afgerond, enzovoort. Deze acties worden bijgehouden ongeacht of ze vanaf de portal van Azure klassieke of met behulp van de [BizTalk-Service REST API's](http://msdn.microsoft.com/library/azure/dn232347.aspx)worden uitgevoerd. Voor een volledige lijst met bewerkingen die worden bijgehouden met behulp van Management-Services, raadpleegt u [Bewerkingen bijgehouden met Azure Management Services](#bizops).<br/><br/>
De logboeken van activiteiten op BizTalk Service runtime (zoals bericht verwerkt door bruggen, enzovoort.), wordt dit niet vastgelegd. Als u wilt deze logboeken bekijken, door de weergave bijhouden vanaf de portal BizTalk-Services te gebruiken. Zie [Berichten bijhouden](http://msdn.microsoft.com/library/azure/hh949805.aspx)voor meer informatie.

## <a name="view-biztalk-services-operation-logs"></a>Weergave BizTalk Services bewerking Logboeken
1. Klik in de klassieke Azure portal **Management-Services**te selecteren en selecteer vervolgens het tabblad **Logboeken aan de bewerking** .
2. De logboeken op basis van verschillende parameters zoals abonnement, datumbereik, servicetype (bijvoorbeeld BizTalk Services), de servicenaam van de of status van de bewerking (geslaagd, mislukt), kunt u filteren.
3. Selecteer het vinkje om weer te geven van de gefilterde lijst. De volgende afbeelding ziet u activiteiten die betrekking hebben op testbiztalkservice:  ![bewerking logboeken weergeven][ViewLogs] 
4. Als u wilt bekijken over een bepaalde bewerking, selecteert u de rij en klik op **Details** in de taakbalk onder.


## <a name="bizops"></a>Bewerkingen die zijn bijgehouden met behulp van Azure Management-Services
De volgende tabel bevat de bewerkingen die worden bijgehouden met behulp van de Azure Management-Services:

De bewerkingsnaam van de | Taak
--- | ---
CreateBizTalkService | Tijdens het maken van een nieuwe BizTalk Service
DeleteBizTalkService | Bewerking is een BizTalk-Service verwijderen
RestartBizTalkService | Om een BizTalk-Service opnieuw te starten
StartBizTalkService | Bewerking een BizTalk-Service te starten
StopBizTalkService | Bewerking is een BizTalk-Service stoppen
DisableBizTalkService | Bewerking een BizTalk-Service uitschakelen
EnableBizTalkService | Bewerking een BizTalk-Service inschakelen
BackupBizTalkService | Bewerking back-up een BizTalk-Service
RestoreBizTalkService | Bewerking een BizTalk-Service van de opgegeven back-up herstellen
SuspendBizTalkService | Bewerking om op te schorten een BizTalk-Service
ResumeBizTalkService | Bewerking is een BizTalk Service hervatten
ScaleBizTalkService | Bewerking is een BizTalk Service schaal omhoog of omlaag
ConfigUpdateBizTalkService | Bewerking bij de configuratie van een BizTalk-Service
ServiceUpdateBizTalkService | Bewerking upgraden of een BizTalk Service naar een andere versie downgraden
PurgeBackupBizTalkService | Bewerking back-ups van de BizTalk Service buiten de bewaarperiode wissen


## <a name="see-also"></a>Zie ook
- [Back-up maken BizTalk-Service](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [Back-up terugzetten BizTalk-Service](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk Services: Ontwikkelaars, Basic, Standard en Premium edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk Services: Inrichting klassieke portal Azure gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk Services: Inrichting Status grafiek](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk Services: De tabbladen Dashboard, Monitor en schaal](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk-Services: beperken](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk Services: De naam van de uitgever en de uitgever sleutel](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Hoe kan ik gebruiken de SDK van Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 
