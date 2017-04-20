<properties
    pageTitle="Computergroepen in Log Analytics Meld u zoekopdrachten | Microsoft Azure"
    description="Computergroepen in Log Analytics kunnen u bereik log zoekopdrachten tot een bepaalde reeks computers.  In dit artikel worden de verschillende methoden die u gebruiken kunt om computergroepen en hoe ze worden gebruikt in een zoekopdracht log te maken."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>Meld u computergroepen in Log Analytics zoekopdrachten
Computergroepen in Log Analytics kunnen u bereik [zoekopdrachten Meld u aan](log-analytics-log-searches.md) bij een bepaalde reeks computers.  Elke groep wordt gevuld met de computers door een query die u definieert of door groepen importeren uit andere bronnen.  Wanneer de groep in een zoekopdracht log is opgenomen, zijn de resultaten zijn beperkt tot de records die overeenkomen met de computers in de groep.

## <a name="creating-a-computer-group"></a>Maken van een computergroep
U kunt een computergroep maken in Log Analytics gebruiken met een van de methoden in de volgende tabel.  Meer informatie over elke methode worden gegeven in de onderstaande secties. 

| Methode | Beschrijving |
|:---|:---|
| Log zoeken       | Maak een logboek zoeken die resulteert in een lijst met computers en de resultaten opslaan als een computergroep. |
| Log zoeken API   | Gebruik de zoeken-API logboek maken op een computergroep op basis van de resultaten van een zoekopdracht log. |
| Active Directory | Automatisch scannen het groepslidmaatschap van alle agentcomputers die deel uitmaken van Active Directory-domein en een groep maken in Log Analytics voor elke beveiligingsgroep.
| WSUS              | Automatisch WSUS-servers of clients voor groepen doelgroepen scannen en een groep maken in Log Analytics voor elk label. |


### <a name="log-search"></a>Log zoeken

Computergroepen gemaakt op basis van een zoekopdracht Log bevat alle computers die het resultaat van een zoekopdracht die u definieert.  Deze query wordt uitgevoerd telkens wanneer de computergroep wordt gebruikt, zodat alle wijzigingen sinds de groep is gemaakt worden weergegeven.

Gebruik de volgende procedure een computergroep maken van een logboek met uw zoekopdracht.

1. [Een zoekopdracht logboek maken](log-analytics-log-searches.md) die resulteert in een lijst met computers.  De zoekopdracht moet een unieke set computers retourneren met iets zoals **Distinct Computer** of **maateenheid count() door Computer** in de query.  
2. Klik op de knop **Opslaan** boven aan het scherm.
3. Selecteer **Ja** om te **deze query opslaan als een computergroep:**.
4. Typ een **naam** en een **categorie** voor de groep.  Als een zoekopdracht met dezelfde naam en categorie al bestaat, wordt u gevraagd om te overschrijven.  U kunt meerdere zoekopdrachten met dezelfde naam hebben in verschillende categorieën. 

Voorbeeld van zoekopdrachten die u kunt opslaan als een computergroep Hier volgen.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Meld u Zoek-API

Computergroepen gemaakt met de API voor het zoeken van Log zijn hetzelfde als zoekopdrachten die zijn gemaakt met een zoekopdracht Log.

Zie [computergroepen in Log Analytics Meld zoeken REST API](log-analytics-log-search-api.md#computer-groups)voor meer informatie over het maken van de groep van een computer met de API voor het zoeken van Log.

### <a name="active-directory"></a>Active Directory

Als u Log Analytics configureert importeren lidmaatschappen van Active Directory-groep, wordt deze het groepslidmaatschap van een domein toegevoegd computers met de OMS-agent analyseren.  Een computergroep in Log Analytics gemaakt voor elke beveiligingsgroep in Active Directory en elke computer wordt toegevoegd aan de computergroepen die overeenkomt met de beveiligingsgroepen die ze lid van zijn.  Deze lidmaatschap elke 4 uur continu worden bijgewerkt.  

U configureren Log Analytics als u wilt importeren van Active Directory-beveiligingsgroepen in het menu **Computergroepen** op Log Analytics- **Instellingen**.  Selecteer **automatisering** en selecteer **importeren Active Directory-groepslidmaatschappen van computers**.  Er is geen verdere configuratie vereist.

![Computergroepen uit Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Wanneer groepen zijn geïmporteerd, wordt het menu lijst met het aantal computers met groepslidmaatschap gedetecteerd en het aantal groepen die zijn geïmporteerd.  U kunt klikken op een van de volgende koppelingen om de records **ComputerGroup** met deze informatie te geven.

### <a name="windows-server-update-service"></a>Windows Server Update-Service

Als u Log Analytics WSUS groepslidmaatschappen importeren configureert, wordt deze het doelgroepen groepslidmaatschap van alle computers met de OMS-agent analyseren.  Als u aan de clientzijde werkt hebt doelgroepen, elke computer die is verbonden met OMS en maakt deel uit van een WSUS hebt samengesteld groepen de groepslidmaatschap geïmporteerd op Log Analytics. Als u werkt met servers moet doelgroepen, de OMS agent worden geïnstalleerd op de server WSUS in volgorde voor de groepslidmaatschapgegevens worden geïmporteerd op OMS.  Deze lidmaatschap elke 4 uur continu worden bijgewerkt. 

U configureren Log Analytics als u wilt importeren van Active Directory-beveiligingsgroepen in het menu **Computergroepen** op Log Analytics- **Instellingen**.  Selecteer **Active Directory** en selecteer **importeren Active Directory-groepslidmaatschappen van computers**.  Er is geen verdere configuratie vereist.

![Computergroepen uit Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Wanneer groepen zijn geïmporteerd, wordt het menu lijst met het aantal computers met groepslidmaatschap gedetecteerd en het aantal groepen die zijn geïmporteerd.  U kunt klikken op een van de volgende koppelingen om de records **ComputerGroup** met deze informatie te geven.

## <a name="managing-computer-groups"></a>Computergroepen beheren

U kunt computergroepen die zijn gemaakt met een zoekopdracht log of de API voor het zoeken van Log in het menu **Computergroepen** op Log Analytics- **Instellingen**weergeven.  Klik op de **x** in de kolom **verwijderen** om de computergroep te verwijderen.  Klik op het pictogram **leden weergeven** voor een groep waaraan u wilt uitvoeren van de groep log zoeken die resulteert in de leden. 

![Opgeslagen computergroepen](media/log-analytics-computer-groups/configure-saved.png)

Als u wilt wijzigen in de groep, moet u een nieuwe groep maken met dezelfde **categorie** en **de naam van** de oorspronkelijke groep overschrijven.

## <a name="using-a-computer-group-in-a-log-search"></a>Een computergroep gebruiken in een logboek zoeken
U de volgende syntaxis gebruiken om te verwijzen naar een computergroep in een zoekopdracht log.  Aangegeven met de **categorie** optioneel en alleen vereist als u computergroepen met dezelfde naam in verschillende categorieën hebben. 

    $ComputerGroups[Category: Name]

Wanneer u een zoekopdracht uitvoert, worden eerst de leden van een computer-groep in de zoekopdracht opgenomen omgezet.  Als de groep is gebaseerd op een logboek met zoeken, wordt klikt u vervolgens die zoeken uitgevoerd om terug te keren van de leden van de groep voordat u het bovenste niveau log zoekactie uitvoert.

Computergroepen worden meestal gebruikt met **de component in het logboek zoeken zoals in het volgende voorbeeld** .

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Computer groepsrecords

Een record is gemaakt in de bibliotheek OMS voor elke computer groepslidmaatschap gemaakt op basis van Active Directory of WSUS.  Deze records een soort **ComputerGroup** en hebt de eigenschappen in de volgende tabel.  Records worden niet gemaakt voor computergroepen op basis van log zoekopdrachten.

| Eigenschap | Beschrijving |
|:--|:--|
| Type                | *ComputerGroup* |
| SourceSystem        | *SourceSystem*  |
| Computer            | De naam van de lidcomputer. |
| Groep               | De naam van de groep. |
| GroupFullName       | Volledige pad naar de groep met inbegrip van de bron- en de bronnaam van de.
| GroupSource         | Gegevensbron die groep is verzameld uit. <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | Naam van de bron die de groepen is verzameld uit.  Voor Active Directory is dit de domeinnaam. |
| ManagementGroupName | De naam van de management group voor SCOM agenten.  Voor andere gemachtigden, is dit de AOI -\<werkruimte-ID\> |
| TimeGenerated       | Datum en tijd groep op de computer is gemaakt of bijgewerkt. |



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [log zoekopdrachten](log-analytics-log-searches.md) om de gegevens die worden verzameld uit gegevensbronnen en oplossingen te analyseren.  