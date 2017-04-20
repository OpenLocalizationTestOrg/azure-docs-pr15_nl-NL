<properties
   pageTitle="Visual Studio en SSDT voor SQL datawarehouse installeren | Microsoft Azure"
   description="Visual Studio en SQL Server Development Tools (SSDT) voor SQL Azure datawarehouse installeren"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Visual Studio-2015 en SSDT voor SQL datawarehouse installeren

Als u wilt het ontwikkelen van toepassingen voor SQL Data Warehouse, wordt u aangeraden in Visual Studio-2015 gebruiken met de meest recente versie van SQL Server Data Tools (SSDT).  Visual Studio 2013 Update 5 met SSDT wordt ook ondersteund voor compatibiliteit met eerdere versies.  

Gebruik van Visual Studio met SSDT kunt u met de SQL Server-Object Explorer visueel verkennen van tabellen, weergaven, opgeslagen procedures en veel meer objecten in uw SQL Data Warehouse, evenals query's uitvoeren.

> [AZURE.NOTE] SQL Data Warehouse biedt nog geen ondersteuning voor Visual Studio databaseprojecten.  Deze functie wordt toegevoegd aan een toekomstige versie.

## <a name="step-1-install-visual-studio-2015"></a>Stap 1: Installeer Visual Studio-2015

Volg deze koppelingen als u wilt downloaden en installeren van Visual Studio-2015. Als u al hebt Visual Studio 2013 of 2015 is geïnstalleerd, kunt u gaat u verder met stap 2, SSDT installeert.

1. [Visual Studio-2015 downloaden][].
2. Volg de [Installatie van Visual Studio][] -handleiding op MSDN en kiest u de standaardconfiguraties.

## <a name="step-2-install-ssdt"></a>Stap 2: SSDT installeren

Als u wilt installeren SSDT voor Visual Studio gewoon zoeken naar een SSDT update van Visual Studio door deze stappen uit.

1. Klik in Visual Studio op **Extra** / **Extensions en Updates...**  /  **Updates**
2. Selecteer **Productupdates** en zoek naar **Microsoft SQL Server-updatebericht voor database gereedschap**

Als een update niet wordt gevonden, kunt u de meest recente versie geïnstalleerd nodig hebt.  Om te bevestigen SSDT is geïnstalleerd, klikt u op **Help** / **Over Microsoft Visual Studio** en uiterlijk voor SQL Server Data Tools in de lijst.  De nieuwste versie van SSDT is 14.0.60525.0.  Als de optie installeren niet beschikbaar in Visual Studio is, kunt u kunt ook u via de pagina [SSDT downloaden][] die u wilt downloaden en installeren SSDT handmatig.

## <a name="next-steps"></a>Volgende stappen

Nu dat u de meest recente versie van SSDT hebt, bent u klaar om [verbinding][] te naar uw SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[verbinding maken]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio-2015 downloaden]: https://www.visualstudio.com/downloads/
[Installatie van Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT downloaden]: https://msdn.microsoft.com/library/mt204009.aspx
