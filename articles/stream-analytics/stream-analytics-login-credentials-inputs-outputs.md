<properties 
    pageTitle="Stream Analytics: Draaien aanmelden referenties voor de invoer en uitvoer | Microsoft Azure" 
    description="Informatie over het bijwerken van de referenties voor de invoer van de Stream Analytics en uitvoer."
    keywords="aanmeldingsreferenties"
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

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Aanmeldingsreferenties voor invoer en uitvoer in Stream Analytics taken draaien

##<a name="abstract"></a>Samenvatting
Azure Stream Analytics vandaag nog niet is toegestaan worden vervangen door de referenties op een invoer/uitvoer, terwijl de taak wordt uitgevoerd.

Terwijl Azure Stream Analytics ondersteuning biedt voor een taak van de laatste uitvoer hervatten, wilde we delen van het hele proces voor het minimaliseren van de vertraging tussen de stoppen en starten van de taak en de aanmeldingsreferenties draaien.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>Deel 1: de nieuwe set referenties voorbereiden:
In dit onderdeel is van toepassing op de volgende invoer/uitvoer:

* -Blobopslag
* Gebeurtenis Hubs
* SQL-Database
* Table Storage

Voor andere invoer/uitvoer, gaat u verder met deel 2.

###<a name="blob-storagetable-storage"></a>Opslag/tabel-blobopslag
1.  Ga naar de extensie opslagruimte op de Azure-beheerportal:  
![graphic1][graphic1]
2.  Zoek de opslag gebruikt door uw taak en gaat u naar deze:  
![graphic2][graphic2]
3.  Klik op de opdracht toegangstoetsen beheren:  
![graphic3][graphic3]
4.  Tussen de primaire sleutel van Access en secundaire sneltoets, **selecteert u de dat niet wordt gebruikt door uw taak**.
5.  Klik op opnieuw genereren:  
![graphic4][graphic4]
6.  Kopieer de nieuwe sleutel:  
![graphic5][graphic5]
7.  Ga naar deel 2.

###<a name="event-hubs"></a>Gebeurtenis hubs
1.  Ga naar de extensie van de Service USB op de Azure-beheerportal:  
![graphic6][graphic6]
2.  Zoek de Service Bus Namespace gebruikt door uw taak en gaat u naar deze:  
![graphic7][graphic7]
3.  Als u uw werk gebruikt een gedeelde-beleid op de Service Bus Namespace, Ga naar stap 6  
4.  Ga naar het tabblad gebeurtenis Hubs:  
![graphic8][graphic8]
5.  Zoek de gebeurtenis-Hub gebruikt door uw taak en gaat u naar deze:  
![graphic9][graphic9]
6.  Ga naar het tabblad configureren:  
![graphic10][graphic10]
7.  Zoek op de vervolgkeuzelijst beleidsnaam het gedeelde-beleid dat is gebruikt door uw taak:  
![graphic11][graphic11]
8.  Tussen de primaire sleutel en de tweede sleutel, **selecteert u de dat niet wordt gebruikt door uw taak**.  
9.  Klik op opnieuw genereren:  
![graphic12][graphic12]
10. Kopieer de nieuwe sleutel:  
![graphic13][graphic13]
11. Ga naar deel 2.  

###<a name="sql-database"></a>SQL-Database

>[AZURE.NOTE] Opmerking: u moet verbinding maken met de SQL-Database-Service. We gaan om weer te geven hoe u dit de ervaring management gebruikt in de beheerportal van Azure, maar u kunt u sommige aan de clientzijde gebruiken, zoals SQL Server Management Studio ook.

1.  Ga naar de extensie SQL-Databases op de Azure-beheerportal:  
![graphic14][graphic14]
2.  Zoek de SQL-Database die wordt gebruikt door de koppeling voor uw taak en **Klik op de server** op dezelfde regel:  
![graphic15][graphic15]
3.  Klik op de opdracht beheren:  
![graphic16][graphic16]
4.  Type Database outmodel:  
![graphic17][graphic17]
5.  Typ uw gebruikersnaam, wachtwoord en klik op Log in:  
![graphic18][graphic18]
6.  Klik op nieuwe Query:  
![graphic19][graphic19]
7.  Typen in de volgende query < login_name > vervangen door uw gebruikersnaam en het vervangen van <enterStrongPasswordHere> met uw nieuwe wachtwoord:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Klik op uitvoeren:  
![graphic20][graphic20]
9.  Ga terug naar stap 2 en deze keer Klik op de database:  
![graphic21][graphic21]
10. Klik op de opdracht beheren:  
![graphic22][graphic22]
11. Typ uw gebruikersnaam, wachtwoord en klik op aanmelden:  
![graphic23][graphic23]
12. Klik op nieuwe Query:  
![graphic24][graphic24]
13. Typ in de volgende query < gebruikersnaam > vervangen door een naam waarmee u identificeren van deze aanmelding in de context van deze database wilt (u kunt u bijvoorbeeld voor < login_name >, heeft hetzelfde resultaat geven) en < login_name > te vervangen door de gebruikersnaam van uw nieuwe:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Klik op uitvoeren:  
![graphic25][graphic25]
15. U moet nu de nieuwe gebruiker met dezelfde rollen en bevoegdheden voor uw oorspronkelijke gebruiker had opgeeft.
16. Ga naar deel 2.

##<a name="part-2-stopping-the-stream-analytics-job"></a>Deel 2: De Stream Analytics-taak stoppen
1.  Ga naar de Stream Analytics-extensie op de Azure-beheerportal:  
![graphic26][graphic26]
2.  Zoek uw werk en gaat u naar deze:  
![graphic27][graphic27]
3.  Ga naar het tabblad invoeritems of de uitvoer van op basis van of u de referenties op een invoer of op een uitvoer zijn draaien.  
![graphic28][graphic28]
4.  Klik op de opdracht Stop en Bevestig dat de taak is gestopt:  
![graphic29][graphic29] wachten om door de taak om te stoppen.
5.  Zoek de invoer/uitvoer die u wilt draaien referenties op en gaat u naar deze:  
![graphic30][graphic30]
6.  Ga verder naar deel 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Deel 3: De referenties voor de Stream Analytics-taak bewerken

###<a name="blob-storagetable-storage"></a>Opslag/tabel-blobopslag
1.  Ga naar het veld opslag Accountsleutel en uw nieuwe gegenereerde sleutel plakt:  
![graphic31][graphic31]
2.  Klik op de opdracht opslaan en Bevestig uw wijzigingen op te slaan:  
![graphic32][graphic32]
3.  Een verbindingstest automatisch gestart wanneer u de wijzigingen opslaat Zorg ervoor dat wil zeggen geslaagd is.
4.  Ga verder met deel 4.

###<a name="event-hubs"></a>Gebeurtenis hubs
1.  Het veld gebeurtenis Hub beleidssleutel vinden en uw nieuwe gegenereerde sleutel plakt:  
![graphic33][graphic33]
2.  Klik op de opdracht opslaan en Bevestig uw wijzigingen op te slaan:  
![graphic34][graphic34]
3.  Een verbindingstest automatisch gestart wanneer u de wijzigingen opslaat Zorg ervoor dat deze met succes is verstreken.
4.  Ga verder met deel 4.

###<a name="power-bi"></a>Power BI
1.  Klik op de autorisatie verlengen:  
* ![graphic35][graphic35]
* Hier krijgt u de volgende bevestiging:  
* ![graphic36][graphic36]
2.  Klik op de opdracht opslaan en Bevestig uw wijzigingen op te slaan:  
![graphic37][graphic37]
3.  Een verbindingstest automatisch gestart wanneer u uw wijzigingen opslaan, zorg ervoor dat deze is verstreken.
4.  Ga verder met deel 4.

###<a name="sql-database"></a>SQL-Database
1.  De velden gebruikersnaam en wachtwoord vinden en plak de zojuist gemaakte set referenties in deze:  
![graphic38][graphic38]
2.  Klik op de opdracht opslaan en Bevestig uw wijzigingen op te slaan:  
![graphic39][graphic39]
3.  Een verbindingstest automatisch gestart wanneer u de wijzigingen opslaat Zorg ervoor dat deze met succes is verstreken.  
4.  Ga verder met deel 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>Deel 4: Uw werk beginnend bij de laatste keer dat gestopt
1.  Ga verder af van de invoer/uitvoer:  
![graphic40][graphic40]
2.  Klik op de opdracht Start:  
![graphic41][graphic41]
3.  Kies de laatste keer dat gestopt en klik op OK:  
 ![graphic42][graphic42]
4.  Ga verder naar deel 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>Deel 5: De oude set referenties verwijderen
In dit onderdeel is van toepassing op de volgende invoer/uitvoer:
* -Blobopslag
* Gebeurtenis Hubs
* SQL-Database
* Table Storage

###<a name="blob-storagetable-storage"></a>Opslag/tabel-blobopslag
Herhaal deel 1 voor de toegangstoets die eerder is gebruikt door uw taak nu ongebruikte toegangstoets verlengen.

###<a name="event-hubs"></a>Gebeurtenis hubs
Herhaal deel 1 voor de sleutel die eerder is gebruikt door uw taak de nu ongebruikte toets verlengen.

###<a name="sql-database"></a>SQL-Database
1.  Ga terug naar het queryvenster uit deel 1 stap 7 en typ de volgende query < previous_login_name > vervangen door de naam van de gebruiker die eerder is gebruikt door uw taak:  
`DROP LOGIN <previous_login_name>`  
2.  Klik op uitvoeren:  
    ![graphic43][graphic43]  

U moet de volgende bevestiging ontvangt: 

    Command(s) completed successfully.

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
