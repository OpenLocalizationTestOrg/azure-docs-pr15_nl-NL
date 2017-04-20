<properties
   pageTitle="Verbinding maken met gegevensbronnen | Microsoft Azure"
   description="Hoe kan ik artikel hoe u verbinding maken met gegevensbronnen met Azure-gegevenscatalogus ontdekt markeren."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>Verbinding maken met gegevensbronnen

## <a name="introduction"></a>Inleiding
**Microsoft Azure-gegevenscatalogus** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van discovery voor enterprise-gegevensbronnen fungeert. **Azure-gegevenscatalogus** is met andere woorden, helpen bij het personen ontdekken, inzichtelijk maken en gegevensbronnen, en helpt organisaties gebruikt om meer waarde van de bestaande gegevens. Een belangrijke aspecten van dit scenario wordt gebruikt de gegevens: wanneer een gebruiker een gegevensbron ontdekt en kunt u het doel, de volgende stap is het verbinding maken met de gegevensbron moeten worden geplaatst van de gegevens kunnen worden gebruikt.

## <a name="data-source-locations"></a>Gegevensbronlocaties
Tijdens de registratie van gegevensbron ontvangt **De gegevenscatalogus Azure** metagegevens over de gegevensbron. Deze metagegevens bevat de details van de locatie van de gegevensbron. De details van de locatie vindt, varieert van gegevensbron met gegevensbron, maar bevat altijd de informatie die nodig is om verbinding te maken. Bijvoorbeeld bevat de locatie voor een SQL Server-tabel de servernaam, de databasenaam van de, schemanaam, en de naam van de tabel, terwijl de locatie voor een SQL Server Reporting Services-rapport de servernaam van de en het pad naar het rapport bevat. Andere typen gegevensbronnen heeft locaties die overeenkomen met de structuur en mogelijkheden van het bronsysteem.

## <a name="integrated-client-tools"></a>Geïntegreerde clienthulpprogramma 's
De eenvoudigste manier verbinding maken met een gegevensbron is gebruik van de ' geopend in de... " menu in de portal **Azure-gegevenscatalogus** . In dit menu bevat een overzicht van de opties voor de verbinding met de activa van de geselecteerde gegevens.
Wanneer u met de standaardweergave van de tegel, kan dit menu is beschikbaar op de tegel van elke.

 ![Een SQL Server-tabel openen in Excel vanuit de tegel van de activa gegevens](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Wanneer u met de lijstweergave, is het menu beschikbaar in de zoekbalk boven aan het venster van de portal.

 ![In Report Manager van een SQL Server Reporting Services-rapport openen vanuit de zoekbalk](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Ondersteunde clienttoepassingen
Bij gebruik van de ' geopend in de... " menu voor gegevensbronnen in de gegevenscatalogus van Azure-portal, de juiste clienttoepassing moet worden geïnstalleerd op de clientcomputer.

| Geopend in de toepassing | Bestandsextensie / protocol | Ondersteunde toepassingsversies |
| --- | --- | --- |
| Excel | ODC | Excel 2010 of hoger |
| Excel (boven 1000) | ODC | Excel 2010 of hoger |
| Power Query | .xlsx | Excel 2016 of Excel 2010 of Excel 2013 met de Power Query voor Excel-invoegtoepassing geïnstalleerd
| Power BI Desktop | .pbix | Power BI Desktop juli 2016 of hoger |
| SQL Server Data Tools | vsweb: / / | Visual Studio 2013 Update 4 of hoger met SQL Server-tooling geïnstalleerd |
| Report Manager | http:// | Zie [Browservereisten voor SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Uw gegevens, de hulpprogramma 's
De beschikbare opties in het menu is afhankelijk van het type gegevens activa geselecteerd. Natuurlijk niet alle mogelijke gereedschap worden opgenomen de "geopend in de..." menu, maar het is nog steeds eenvoudig verbinding maken met de gegevensbron met behulp van een hulpmiddel client. Wanneer een activum gegevens is geselecteerd in de **Gegevenscatalogus van Azure** -portal, wordt de volledige locatie wordt weergegeven in het deelvenster Eigenschappen.

 ![Informatie over de databaseverbinding voor een SQL Server-tabel](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

De verbindingsgegevens informatie wordt verschillen van de gegevensbron van het type naar het gegevenstype van de bron, maar de gegevens die zijn opgenomen in de portal krijgt u alles wat die u moet verbinding maken met de gegevensbron in een hulpmiddel client. Gebruikers kunnen de details van de verbinding voor de gegevensbronnen die ze hebben ontdekt met **Azure-gegevenscatalogus**, zodat ze werken met de gegevens in hun hulpprogramma van keuze kopiëren.

## <a name="connecting-and-data-source-permissions"></a>Verbinding maken en gegevens bron-machtigingen
Hoewel **De gegevenscatalogus Azure** gegevensbronnen makkelijker kan worden gevonden maakt, blijft de toegang tot de gegevens zelf onder het beheer van de gegevensbron eigenaar of beheerder. Een gegevensbron in **De gegevenscatalogus Azure** ontdekken geeft geen een gebruiker de machtigingen voor toegang tot de gegevensbron zelf.

Om eenvoudiger voor gebruikers die kennismaken met een gegevensbron maar bent niet gemachtigd voor toegang tot de gegevens, kunnen gebruikers gegevens in de eigenschap toegang aanvragen opgeven wanneer aantekeningen maken van een gegevensbron. Informatie die hier, waaronder koppelingen naar het proces of contactpersoon voor toegang tot de bron van gegevens: wordt gepresenteerd samen met de locatie van de gegevensbroninformatie in de portal.

 ![Informatie over de databaseverbinding met verzoek om access instructies](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>Overzicht
Een gegevensbron met **De gegevenscatalogus Azure** registreren, maakt die gegevens makkelijker kan worden gevonden door structurele en beschrijvende metagegevens van de gegevensbron te kopiëren naar de catalogus-service. Als een gegevensbron is geregistreerd, en heeft gevonden, gebruikers kunnen verbinden met de gegevensbron van de **Gegevenscatalogus van Azure** -portal "geopend in de..." " menu of gebruik de hulpmiddelen voor hun gegevens van keuze.

## <a name="see-also"></a>Zie ook
- [Aan de slag met Azure-gegevenscatalogus](data-catalog-get-started.md) zelfstudie voor meer informatie over het verbinding maken met gegevensbronnen.
