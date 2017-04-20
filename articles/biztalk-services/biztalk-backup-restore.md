<properties 
    pageTitle="Maken en herstellen van een back-up in BizTalk Services | Microsoft Azure" 
    description="BizTalk Services bevat back-up en herstellen. Meer informatie over het maken en herstellen van een back-up en bepalen wat een back-wordt gemaakt. MAB, WABS" 
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


# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Back-up maken en deze herstellen

Azure BizTalk Services omvat mogelijkheden voor back-up en herstellen. In dit onderwerp wordt beschreven hoe back-up maken en terugzetten BizTalk-Services met behulp van de Azure klassieke portal.

U kunt ook een back-up BizTalk Services de [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584)gebruiken. 

> [AZURE.NOTE] Hybride verbindingen worden niet back-up gemaakt, ongeacht de versie. U moet uw hybride verbindingen opnieuw maken.

## <a name="before-you-begin"></a>Voordat u begint

- Back-up en herstellen mogelijk niet beschikbaar voor alle edities. Zie [BizTalk Services: edities grafiek](biztalk-editions-feature-chart.md).

- De portal van Azure klassieke gebruikt, kunt u een op aanvraag back-up maken of een geplande back-up maken. 

- Back-inhoud kan worden teruggezet naar dezelfde BizTalk Service of naar een nieuwe BizTalk Service. Als u wilt herstellen de BizTalk-Service met dezelfde naam, de bestaande BizTalk Service moeten worden verwijderd en de naam moet beschikbaar. Nadat u een BizTalk Service hebt verwijderd, duurt het langer dan het gewenste type voor dezelfde naam kan worden gecontroleerd. Als u geen tijd hebt om dezelfde naam kan worden gecontroleerd, herstellen naar een nieuwe BizTalk-Service.

- BizTalk-Services kan op dezelfde editie of een hogere editie worden hersteld. Voor informatie over het herstellen van BizTalk-Services op een lagere editie wordt van waarop de back-up is gemaakt, niet ondersteund.

    Op de Premium-editie kan bijvoorbeeld een back-up het gebruik van de Basic Edition worden hersteld. Een back-up de Premium-editie worden niet hersteld naar de standaardversie.

- De getallen bewerken besturingselement reservekopie op voor het behoud van bedrijfscontinuïteit van de getallen besturingselement. Als berichten worden verwerkt na de laatste back-up, deze back-inhoud herstellen kan leiden tot dubbele besturingselement getallen.

- Als een batch actieve berichten heeft, verwerkt door de batch **voordat** u een back-up. Wanneer u een back-up maakt (als de benodigde of geplande), worden berichten in batches worden nooit opgeslagen. 

    **Als u een back-up is gemaakt met de actieve berichten in een batch, worden deze berichten zijn niet back-up gemaakt en worden daarom verloren.**

- Optioneel: Klik In de Portal BizTalk Services stoppen alle bewerkingen management.


## <a name="create-a-backup"></a>Een back-up maken

Een back-up op elk gewenst moment kan worden gemaakt en volledig wordt beheerd door u. In dit gedeelte vindt u de stappen om te maken van back-ups met behulp van de Azure klassieke portal, waaronder:

[Klik op aanvraag back-up maken](#backupnow)

[Een back-up plannen](#backupschedule)

#### <a name="backupnow"></a>Klik op aanvraag back-up maken
1. Klik in de klassieke Azure portal Selecteer **BizTalk Services**en selecteer vervolgens de gewenste BizTalk Service back-up maken.
2. Selecteer onder aan de pagina een **Back-up** in het tabblad **Dashboard** .
3. Voer een back-naam. Voer bijvoorbeeld *myBizTalkService*BU*datum*.
4. Kies een blob storage-account en selecteer het vinkje om te beginnen van de back-up.

Zodra de back-up is voltooid, wordt een container met de back-naam die u invoert in het opslag-account gemaakt. Deze container bevat de configuratie van de back-up van uw BizTalk-Service.

#### <a name="backupschedule"></a>Een back-up plannen

1. Klik in de klassieke Azure portal Selecteer **BizTalk-Services**, de naam van de BizTalk Service die u wilt plannen van de back-up en selecteer vervolgens het tabblad **configureren** .
2. De **back-Status** instellen op **automatisch**. 
3. Selecteer de **Opslag-Account** om het back-up opslaan, voert u de **frequentie** als u wilt maken van de back-ups, en hoe lang behouden de back-ups (**Bewaarbeleid dagen**):

    ![][AutomaticBU]

    **Notities**   
    - **Bewaarbeleid dagen**moet de bewaarperiode groter dan de back-frequentie.
    - Selecteer **altijd ten minste één back-up behouden**, zelfs als deze verstreken de bewaarperiode is.
    

4. Selecteer **Opslaan**.


Wanneer een geplande back-uptaak wordt uitgevoerd, wordt een container (om op te slaan back-upgegevens) in het opslag-mailaccount dat u hebt ingevoerd. De naam van de container heet *BizTalk Service naam-datum-tijd*. 

Als het dashboard BizTalk Service ziet u de status van een **mislukt** :

![Laatste geplande back-status][BackupStatus] 

De koppeling wordt geopend de logboeken Management Services bewerking om u te helpen oplossen. Zie [BizTalk Services: problemen met de bewerking Logboeken oplossen](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Herstellen

Vanaf de portal van Azure klassieke of de [BizTalk-Service REST API herstellen](http://go.microsoft.com/fwlink/p/?LinkID=325582), kunt u back-ups terugzetten. In dit gedeelte vindt u de stappen om te herstellen met behulp van de klassieke portal.

#### <a name="before-restoring-a-backup"></a>Voordat u een back-up terugzet

- Nieuwe bijhouden, archivering en winkels cmdlets voor controle kunnen worden opgenomen tijdens een BizTalk-Service wordt hersteld.

- Dezelfde gegevens bewerken Runtime is hersteld. De bewerken Runtime back-up van het besturingselement getallen. De herstelde besturingselement getallen zijn in de volgorde van de tijd van de back-up. Als berichten worden verwerkt na de laatste back-up, deze back-inhoud herstellen kan leiden tot dubbele besturingselement getallen.

#### <a name="restore-a-backup"></a>Een back-up herstellen

1. Selecteer **Nieuw**in de portal voor Azure klassieke > **App Services** > **BizTalk Service** > **herstellen**:

    ![Een back-up herstellen][Restore]

2. In de **Back-URL**, selecteer het mappictogram van de en uitvouwen van het Azure opslag-account dat de back-up configuratie BizTalk Service opgeslagen. Vouw de container en selecteer in het rechterdeelvenster de bijbehorende back-up txt-bestand. 
<br/><br/>
Selecteer **openen**.

3. Klik op de pagina **Herstellen BizTalk Service** Voer een **Naam van de Service BizTalk** en controleer of de **URL voor het domein**, **Edition**en **regio** voor de herstelde BizTalk-Service. **Een nieuw exemplaar van de SQL-database maken** voor de database bijhouden:

    ![][RestoreBizTalkService]

    Selecteer de pijl-rechts.

4.  Controleer of de naam van de SQL-database, voert u de fysieke server waar de SQL-database worden gemaakt en een gebruikersnaam en wachtwoord voor de server.


    Als u configureren van de SQL-database-versie wilt, selecteer de grootte en andere eigenschappen van **Geavanceerde Database-instellingen configureren**. 

    Selecteer de pijl-rechts.

5. Maak een nieuwe opslag-account of een bestaand account voor de opslag voor de BizTalk Service invoeren.

7. Selecteer het vinkje om te beginnen met terugzetten.

Zodra het herstel succes is voltooid, wordt een nieuwe BizTalk Service wordt weergegeven in een onderbroken staat op de pagina BizTalk Services in de portal van Azure klassieke.



### <a name="postrestore"></a>Na het herstellen van een back-up

De BizTalk Service altijd weer in een **onderbroken** toestand wordt hersteld. In deze modus kunt u configuratiewijzigingen aanbrengt aanbrengen voordat de nieuwe omgeving functionele is, waaronder:

- Als u BizTalk-Service-toepassingen met de SDK van Azure BizTalk Services hebt gemaakt, mogelijk moet u de referenties Access Control (ACS) in deze toepassingen voor gebruik met de herstelde omgeving bijwerken.

- U herstellen een BizTalk Service als u wilt repliceren van een bestaande BizTalk Service-omgeving. In dit geval als er zijn geconfigureerd in de oorspronkelijke BizTalk Services-portal overeenkomsten met een bron FTP-map, mogelijk moet u de overeenkomsten in de herstelde omgeving naar een andere bron FTP-map gebruiken bijwerken. Anders kunnen er twee verschillende afspraken probeert op te halen hetzelfde bericht.

- Als u teruggezet als u wilt dat meerdere BizTalk Service omgevingen, zorg er dan voor dat u snel de juiste omgeving in de Visual Studio-toepassingen, PowerShell-cmdlets, REST API's of handelsdag Partner Management OM-API's.

- Het is een goede gewoonte automatische back-ups configureren op de herstelde BizTalk Service-omgeving.

Als u wilt de BizTalk-Service starten in de portal van Azure klassieke, selecteer de herstelde BizTalk-Service en selecteer **cv** in de taakbalk. 



## <a name="what-gets-backed-up"></a>Waarvan een back-wordt gemaakt

Als een back-up wordt gemaakt, de volgende items zijn back-up gemaakt:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Items back-up gemaakt</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Services-Portal</strong></td>
</tr> 
<tr>
<td>Configuratie en Runtime</td> 
<td>
<ul>
<li>Details partner en profiel</li>
<li>Partner overeenkomsten</li>
<li>Aangepaste stroombaan geïmplementeerd</li>
<li>Bruggen geïmplementeerd</li>
<li>Certificaten</li>
<li>Transformaties geïmplementeerd</li>
<li>Pijpleidingen</li>
<li>Sjablonen die zijn gemaakt en opgeslagen in de Portal BizTalk diensten</li>
<li>X12 ST01 en GS01 toewijzingen</li>
<li>Besturingselement getallen (bewerken)</li>
<li>AS2 bericht Microfoon waarden</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Azure BizTalk-Service</strong></td>
</tr> 
<tr>
<td>SSL-certificaat</td> 
<td>
<ul>
<li>SSL-certificaat-gegevens</li>
<li>Wachtwoord voor SSL-certificaat</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk-Service-instellingen</td> 
<td>
<ul>
<li>Aantal schaal-eenheden</li>
<li>Edition</li>
<li>Productversie</li>
<li>Land/Datacenter</li>
<li>Access Control-Service (ACS) naamruimte en van toetsen</li>
<li>Het bijhouden van de verbindingsreeks van de database</li>
<li>Opslag account verbindingsreeks archiveren</li>
<li>Cmdlets voor controle verbindingsreeks voor opslag-account</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Aanvullende Items</strong></td>
</tr> 
<tr>
<td>Het bijhouden van de Database</td> 
<td>Wanneer de BizTalk-Service is gemaakt, worden de details van de Database bijhouden ingevoerd, met inbegrip van de Azure SQL Database-Server en de naam van de Database bijhouden. De Database voor het bijhouden van wijzigingen is niet automatisch back-up gemaakt.
<br/><br/>
<strong>Belangrijke</strong><br/>
Als de Database bijhouden wordt verwijderd en de databasebehoeften is hersteld, moet een vorige back-up bestaan. Als u een back-up niet bestaat, zijn de Database bijhouden en de gegevens niet worden hersteld. In dit geval door een nieuwe Database voor het bijhouden van wijzigingen te maken met dezelfde naam van de database. Geografische-replicatie wordt aanbevolen.</td>
</tr> 
</table>

## <a name="next"></a>Volgende

Als u wilt maken Azure BizTalk Services in de portal van Azure klassieke, gaat u naar [BizTalk Services: gebruik van Azure inrichting klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=302280). Als u wilt beginnen met het maken van toepassingen, gaat u naar [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

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

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 
