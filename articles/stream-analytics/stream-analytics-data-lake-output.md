<properties
    pageTitle="Stream Analytics gegevensopslag Lake uitvoer | Microsoft Azure"
    description="Configuratie van verificatie en machtiging van een Azure Lake gegevensopslag in een Stream Analytics-project"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Stream Analytics gegevensopslag Lake uitvoer

Stream Analytics taken ondersteunen verschillende uitvoermethoden, namelijk een [Azure Lake gegevensopslag](https://azure.microsoft.com/services/data-lake-store/). Azure Lake gegevensopslag is een hele bedrijf hyper-schaal opslagplaats voor grote gegevens analytische werkbelasting. Gegevensopslag Lake kunt u gegevens van elke grootte, type en opname snelheid van operationele en experimentele analysegegevens op te slaan.

## <a name="authorize-a-data-lake-store-account"></a>Een account voor gegevensopslag Lake Autoriseer

1.  Wanneer Lake gegevensopslag als een uitvoer in de beheerportal van Azure is geselecteerd, wordt u gevraagd naar het gebruik van uw bestaande Lake gegevensopslag Autoriseer of toegang tot het voorbeeld van de gegevens Lake Store via de Portal van Azure-klassieke aanvragen.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  Als u al toegang aan meer gegevensopslag hebt, klikt u op "Nu Autoriseer" en voor een korte tijd een pagina wordt weergegeven waarin wordt aangegeven dat "Omleidt naar autorisatie..". De pagina wordt automatisch gesloten en u krijgt de pagina waarmee u de Lake gegevensopslag-uitvoer configureren.

Als u niet hebt aangemeld voor gegevens Lake Store Preview, kunt u Volg de koppeling "Nu registeren" starten van de aanvraag of volg de [instructies weergeven gestart](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>De eigenschappen van de uitvoer Lake gegevensopslag configureren

Nadat u het geverifieerd Lake gegevensopslag-account hebt, kunt u de eigenschappen voor uw Lake gegevensopslag-uitvoer configureren. De onderstaande tabel is de lijst met eigenschapnamen en de bijbehorende beschrijvingen voor het configureren van uw uitvoer Lake gegevensopslag.

<table>
<tbody>
<tr>
<td><B>NAAM VAN EIGENSCHAP</B></td>
<td><B>BESCHRIJVING</B></td>
</tr>
<tr>
<td>Uitvoeralias</td>
<td>Dit is een beschrijvende naam in query's gebruikt om de queryuitvoer naar deze gegevens Lake Store.</td>
</tr>
<tr>
<td>Gegevensopslag Lake-Account</td>
<td>De naam van de opslag-account waar u uw uitvoer verzendt. U krijgt een vervolgkeuzelijst Lake gegevensopslag accounts waaraan de gebruiker is aangemeld bij de portal toegang tot heeft.</td>
</tr>
<tr>
<td>Pad voorvoegsel patroon [<I>optionele</I>]</td>
<td>Het bestandspad gebruikt om te schrijven van uw bestanden in de opgegeven gegevens Lake Store-Account. <BR>{date}, {tijd}<BR>Voorbeeld 1: Map1/logboeken / {date} / {afstemmen}<BR>Voorbeeld 2: Map1/logboeken / {date}</td>
</tr>
<tr>
<td>Datumnotatie [<I>optionele</I>]</td>
<td>Als het token datum wordt gebruikt in het voorvoegsel pad, kunt u de datumnotatie waarin uw bestanden worden opgeslagen. Voorbeeld: Jjjj/MM-DD</td>
</tr>
<tr>
<td>Tijdnotatie [<I>optionele</I>]</td>
<td>Als de tijd token wordt gebruikt in het voorvoegsel pad, geeft u de tijdnotatie waarin uw bestanden worden opgeslagen. De enige ondersteunde waarde is momenteel [HH.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Serialiseringsindeling voor uitvoergegevens. JSON, CSV- en Avro worden ondersteund.</td>
</tr>
<tr>
<td>Codering</td>
<td>Als CSV- of JSON opmaken, moet een codering worden opgegeven. De enige ondersteunde codering indeling is UTF-8 op dit moment.</td>
</tr>
<tr>
<td>Een scheidingsteken</td>
<td>Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal veelvoorkomende scheidingstekens voor serialiseren CSV-gegevens. Ondersteunde waarden zijn komma, puntkomma, spatie, tab en verticale balk.</td>
</tr>
<tr>
<td>Indeling</td>
<td>Alleen van toepassing op JSON serialisatie. Gescheiden geeft aan dat de uitvoer doordat elk JSON-object gescheiden door een nieuwe regel worden opgemaakt. Matrix geeft aan dat de uitvoer worden opgemaakt als een matrix van JSON objecten.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Autorisatie voor gegevensopslag Lake verlengen

Er is momenteel een beperking waar het verificatietoken moet worden handmatig vernieuwd elke 90 dagen voor alle taken met Lake gegevensopslag uitvoer. U moet ook voor de verificatie van uw account voor gegevensopslag Lake opnieuw als u uw wachtwoord hebt gewijzigd sinds de taak is gemaakt of laatst geverifieerd. Een symptoom van dit probleem is geen uitvoer taak en een fout in de logboeken aan de bewerking die aangeeft dat u een nieuwe autorisatie.

U lost dit probleem op door uw actieve taak stoppen en Ga naar uw uitvoer Lake gegevensopslag. Klik op de koppeling "Verlengen autorisatie" en voor een korte tijd een pagina wordt weergegeven waarin wordt aangegeven dat "Omleidt naar autorisatie..". De pagina wordt automatisch gesloten en als geslaagd, wordt aangegeven 'Autorisatie is vernieuwd'. U en vervolgens klikt u op 'Opslaan' onder aan de pagina wilt en kan verdergaan opnieuw starten van uw werk van de laatste keer dat kan worden gestopt om gegevensverlies te voorkomen.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)
