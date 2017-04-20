<properties
   pageTitle="Bijwerken van uw siteverzameling Azure RemoteApp | Microsoft Azure"
   description="Meer informatie over het bijwerken van uw siteverzameling Azure RemoteApp"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Bijwerken van een siteverzameling in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Er komen per keer, onvermijdelijk, wanneer u moet de apps of afbeelding in uw siteverzameling Azure RemoteApp bijwerken. Als u een van de afbeeldingen die wordt geleverd bij uw Azure RemoteApp-abonnement in een cloud- of hybride verzameling, worden alle updates worden verwerkt door Azure RemoteApp zelf, zodat u eenvoudig kunt plaatst.

Echter als u een aangepaste afbeelding (dat u al eens opgebouwd maken of dat u hebt gemaakt met het wijzigen van een van onze afbeeldingen) gebruikt, bent u verantwoordelijk voor het onderhouden van een afbeelding van de en apps. Als u bijwerken van uw afbeelding of een van de apps hierin wilt, moet u voor het maken van een nieuwe, bijgewerkte versie van de afbeelding en klikt u vervolgens de bestaande afbeelding in uw siteverzameling vervangen door deze nieuwe bijgewerkte afbeelding.

Zo is, hoe ga u over het bijwerken van uw siteverzameling? Het is zeer eenvoudig:

1. Werk de afbeelding die u hebt gebruikt in uw siteverzameling. Alle patches of updates nodig toepassen en sla deze op een nieuwe naam.
2. [Uploaden](remoteapp-uploadimage.md) of [importeren](remoteapp-image-on-azurevm.md) die afbeelding naar RemoteApp.
3. Klik nu op de pagina siteverzameling op **bijwerken**.
4. Kies de nieuwe afbeelding in de lijst **Afbeelding van sjabloon** .
4. Hier is het lastig gedeelte - u nodig hebt om te bepalen hoe te handelen gebruikers die momenteel een app in de siteverzameling gebruikt. Hebt u de volgende opties:
    - **Geef gebruikers 60 minuten na het bijwerken**. Zodra het bijwerken is voltooid, verschijnt Azure RemoteApp een bericht naar alle actieve gebruikers hun werk opslaan en meld u af en meld u weer aan om door te geven. Na 60 minuten wordt alle actieve gebruikers die niet hebt afgemeld automatisch afgemeld. Gebruikers kunnen zich meteen aanmelden weer in.
    - **Meld u direct aan gebruikers**. Zodra het bijwerken is voltooid, meld u alle gebruikers automatisch zonder waarschuwing af. Als u deze optie kiest, kunnen gebruikers gegevens verliezen. Echter ze verbinding kunnen maken met de app onmiddellijk.

1. Klik op het vinkje om de update te starten.
