<properties
   pageTitle="Azure gegevenscatalogus releaseopmerkingen | Microsoft Azure"
   description="Releaseopmerkingen voor Azure-gegevenscatalogus."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Azure gegevenscatalogus releaseopmerkingen

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Informatie voor 20 November 2015 release van Azure-gegevenscatalogus

### <a name="opening-data-sources-in-power-bi-desktop"></a>De gegevensbronnen openen in Power BI Desktop

Wanneer u met de optie 'Openen in Power BI Desktop' in de **Gegevenscatalogus van Azure** -portal, kunnen gebruikers een van de volgende problemen in de toepassing Power BI Desktop optreden:

- Het dialoogvenster met de titel 'Kan niet naar geopende Document' wordt weergegeven
- De Power BI Desktop-toepassing wordt geopend, maar het bestand is niet leeg

Voor elke situatie, kan het probleem worden opgelost door te downloaden en installeren van de nieuwste versie van Power BI Desktop uit [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Notities voor de November 13 2015 versie van Azure-gegevenscatalogus

### <a name="registering-and-connecting-to-teradata"></a>Registreren en verbinding maakt met Teradata

Wanneer u verbinding maakt met Teradata gegevensbronnen gebruikers de juiste Teradata ODBC-stuurprogramma geïnstalleerd die overeenkomen met de bitness (32-bits of 64-bits) van de software die wordt gebruikt.

Vanaf deze datum van de release ADC het meest recente [Teradata ODBC-stuurprogramma voor windows (versie 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) is compatibel met Office 2013, maar niet met Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Notities voor 13 juli 2015 versie van Azure-gegevenscatalogus

### <a name="registering-and-connecting-to-oracle-database"></a>Registreren en verbinding maken met een Oracle-Database

Wanneer u verbinding maakt met Oracle-Database-gegevensbronnen die gebruikers moeten hebben geïnstalleerd op de juiste Oracle-stuurprogramma's die overeenkomen met de bitness (32-bits of 64-bits) van de software die wordt gebruikt.

-   Wanneer u registreert Oracle-gegevensbronnen op een computer met 32-bits Windows, worden de 32-bits Oracle-stuurprogramma's gebruikt
-   Wanneer u registreert Oracle-gegevensbronnen op een computer met Windows 64-bits, worden de 64-bits Oracle-stuurprogramma's gebruikt
-   Wanneer u verbinding maakt met een Oracle-gegevensbronnen met Excel op een computer met de 32-bits versie van Microsoft Office, wordt Klik op de 64 bitsversie van Windows, inclusief de 32-bits Oracle-stuurprogramma's gebruikt
-   Wanneer u verbinding maakt met een Oracle-gegevensbronnen Excel gebruiken op een computer waarop de 64-bits versie van Microsoft Office, worden de 64-bits Oracle-stuurprogramma's gebruikt

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registreren en verbinding maken met SQL Server Reporting Services

Ondersteuning voor gegevensbronnen van SQL Server Reporting Services (SSRS) is momenteel alleen beschikbaar voor alleen Native modus-servers. Ondersteuning voor SQL Server Reporting Services in SharePoint-modus wordt toegevoegd in een latere versie.

### <a name="opening-data-assets-in-excel"></a>Gegevens-activa openen in Excel

Bij het openen van gegevens activa in Microsoft Excel in de **Gegevenscatalogus van Azure** -portal, kunnen gebruikers kunnen gevraagd om het dialoogvenster **Beveiligingskennisgeving van Microsoft Excel beschreven** . Dit is standaard, normaal en gebruikers kunnen **inschakelen** om verder te selecteren.

Zie voor meer informatie [in- of uitschakelen van beveiligingsmeldingen over koppelingen en bestanden van verdachte websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Configuratie van proxy en beleid en gegevens registratie gegevensbron

Gebruikers kunnen een situatie waar ze zich aanmelden kunnen bij de portal Azure-gegevenscatalogus, maar als zij proberen te Meld u aan bij de data bron registratie tool een foutbericht dat voorkomt dat deze aanmelden die ze ondervinden ondervinden.

Er zijn twee mogelijke oorzaken voor dit probleem probleem:

**Oorzaak 1: Active Directory Federation Services configuratie** De gegevensbron registratie hulpmiddel maakt gebruik van formulierverificatie voor het valideren van gebruikers die zich aanmelden met Active Directory. Voor een succesvolle aanmelding, moet formulierverificatie zijn ingeschakeld in de globale verificatie-beleid door een Active Directory-beheerder.

In sommige gevallen, kan deze fout gebeuren alleen wanneer de gebruiker bevindt zich op het netwerk van het bedrijf of alleen wanneer de gebruiker verbinding maakt van buiten het bedrijfsnetwerk. De globale verificatie-beleid kan verificatiemethoden afzonderlijk worden ingeschakeld voor intranet en extranet verbindingen. Aanmeldingsfouten kunnen optreden als formulierverificatie is niet ingeschakeld voor het netwerk van waaruit de gebruiker verbinding maakt.

Zie [Beleidsregels voor verificatie configureren](https://technet.microsoft.com/library/dn486781.aspx)voor meer informatie.

**Oorzaak 2: proxy netwerkconfiguratie** Als het bedrijfsnetwerk bevinden een proxyserver gebruikt, kunnen de registratie-hulpmiddel mogelijk geen verbinding maken met Azure Active Directory via de proxy. Gebruikers kunnen Zorg ervoor dat het hulpmiddel registratie door van het hulpprogramma configuratiebestand, bewerken in deze sectie toevoegen aan het bestand:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Zoek het bestand RegistrationTool.exe.config, het registratiehulpprogramma voor starten en open vervolgens het hulpprogramma Windows Taakbeheer. Klik op het tabblad Details in Taakbeheer met de rechtermuisknop op RegistrationTool.exe en kies bestandslocatie openen in het pop-upmenu.
