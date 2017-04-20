<properties 
    pageTitle="Taken die zijn toegestaan in verschillende provincies of statussen in BizTalk Services | Microsoft Azure" 
    description="De acties/bewerkingen toegestaan in andere MAB status: stoppen, starten, opnieuw, uitstellen, hervatten, verwijderen, schaal, configuratie en back bijwerken" 
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



# <a name="biztalk-services-service-state-chart"></a>BizTalk Services: Service staat grafiek
Afhankelijk van de huidige status van de service BizTalk, zijn er bewerkingen die u wel en niet uitvoeren op de BizTalk-service.

Zoals inrichten u een nieuwe BizTalk-service in de portal van Azure klassieke. Nadat deze voltooid is, wordt de BizTalk-service is in actieve status. In de actieve weergave, kunt u de BizTalk-service stoppen. Als stoppen gelukt is, wordt de service BizTalk gaat naar de status van een gestopt. Als stoppen mislukt, wordt de service BizTalk gaat naar de status van een StopFailed. U kunt de BizTalk-service opnieuw in de stand StopFailed. Als u een bewerking dat niet is toegestaan probeert, zoals cv de BizTalk-service, het volgende foutbericht weergegeven:

**Bewerking is niet toegestaan**

Als u wilt een BizTalk Service inrichten, gaat u naar [BizTalk Services: gebruik van Azure inrichting klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).

De volgende tabellen bevatten de bewerkingen of de acties die kunnen worden voltooid als de BizTalk-Service in een specifieke staat. Een ✔ betekent dat de bewerking is toegestaan in die staat. Een lege waarde betekent dat de bewerking niet uitvoeren terwijl u in die staat.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Starten, stoppen, starten, onderbreken, hervatten, en verwijderen
<table border="1">
<tr>
        <th colspan="15">Bewerking of actie</th>
</tr>

<tr>
        <th rowspan="18">Servicestatus BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Starten</th>
        <th>Stoppen</th>
        <th>Opnieuw starten</th>
        <th>Uitstellen</th>
        <th>Cv</th>
        <th>Verwijderen</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Actieve</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Uitgeschakeld</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Geschorst</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Gestopt</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Service-Update is mislukt</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Schaal, configuratie updates en back-bewerkingen
<table border="1">
<tr>
        <th colspan="15">Bewerking of actie</th>
</tr>

<tr>
        <th rowspan="18">Servicestatus BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Schaal</th>
        <th>Configuratie bijwerken</th>
        <th>Back-up maken</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Actieve</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Uitgeschakeld</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Geschorst</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Gestopt</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Service-Update is mislukt</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Zie ook
- [BizTalk Services: Inrichting klassieke portal Azure gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk Services: De tabbladen Dashboard, Monitor en schaal](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk Services: Ontwikkelaars, Basic, Standard en Premium edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk Services: Back-up maken en deze herstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Services: beperken](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk Services: De naam van de uitgever en de uitgever sleutel](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Hoe kan ik gebruiken de SDK van Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
