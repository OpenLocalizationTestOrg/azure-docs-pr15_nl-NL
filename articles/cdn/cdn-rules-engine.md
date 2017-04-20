<properties
    pageTitle="Standaardgedrag HTTP in Azure CDN met de regels-engine | Microsoft Azure"
    description="De engine regels kunt u aanpassen hoe HTTP aanvragen worden verwerkt door Azure CDN, zoals de bezorging van bepaalde soorten inhoud blokkeren, in de cache beleid en HTTP-headers wijzigen."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="override-default-http-behavior-using-the-rules-engine"></a>HTTP standaardgedrag met de regels-engine overschrijven

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

De engine regels kunt u om aan te passen hoe HTTP-aanvragen worden verwerkt, zoals de bezorging van bepaalde soorten inhoud blokkeren, een in de cache beleid definieert en HTTP-headers wijzigen.  Deze zelfstudie demonstreert het in de cache gedrag van CDN activa een regel maken die wordt gewijzigd.  Er is ook videomateriaal beschikbaar in de sectie "[Zie ook](#see-also)".

## <a name="tutorial"></a>Zelfstudie

1. Klik in het blad CDN-profiel op de knop **beheren** .

    ![CDN profiel blade knop beheren](./media/cdn-rules-engine/cdn-manage-btn.png)

    De beheerportal CDN wordt geopend.

2. Klik op het tabblad gevolgd door **Regels Engine** **HTTP grote** .

    Opties voor een nieuwe regel worden weergegeven.

    ![CDN-opties voor nieuwe regel](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] De volgorde waarin meerdere regels van invloed is op hoe ze worden afgehandeld. Een volgende regel mogelijk overschreven door de acties die zijn opgegeven door een vorige regel.
    
3. Voer een naam in de **naam / beschrijving** tekstvak.

4. Identificeer het type van de regel wordt toegepast op aanvragen.  De **altijd** overeenkomst voorwaarde is standaard ingeschakeld.  U moet **altijd** gebruiken voor deze zelfstudie, dus laat die geselecteerd.

    ![CDN overeenkomst voorwaarde](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] Er zijn veel soorten overeenkomst voorwaarden beschikbaar zijn in de vervolgkeuzelijst.  Klikken op het blauwe informatieve pictogram links van de voorwaarde de zoekwaarde, wordt de momenteel geselecteerde voorwaarde gedetailleerd uitgelegd.
    >
    >Zie [regels Engine overeenkomen met voorwaarde en een overzicht van functies](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0)voor de volledige lijst van de zoekwaarde voorwaarden voldoet in detail.

5.  Klik op de **+** knop naast **functies** die u kunt het toevoegen van een nieuwe functie.  Selecteer in de vervolgkeuzelijst aan de linkerkant, **Dwingen interne Max-leeftijd**.  Voer in het tekstvak dat wordt weergegeven, **300**.  Laat de resterende standaardwaarden.

    ![CDN-functie](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] Als als u de voorwaarden van de zoekwaarde, meer informatie over deze functie weergeven te klikken op het blauwe informatieve pictogram links van de nieuwe functie.  In het geval van **Kracht interne Max-leeftijd**, zijn we de **Cache-Control** en **verloopt** kop van het actief aan een besturingselement voor overschrijven wanneer het knooppunt van de rand CDN de activa van de oorsprong worden vernieuwd.  Ons voorbeeld van 300 seconden betekent dat het knooppunt van de rand CDN wordt de activa in cache voor 5 minuten voordat u de activa van de oorspronkelijke vernieuwt.
    >
    >Zie [regels Engine overeenkomst voorwaarde en een overzicht van functies](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1)voor de volledige lijst van functies in detail.

6.  Klik op de knop **toevoegen** om op te slaan van de nieuwe regel.  De nieuwe regel is nu wacht op goedkeuring. Zodra deze is goedgekeurd, verandert de status van **In behandeling XML** in **Actieve XML**.

    >[AZURE.IMPORTANT] Regels wijzigingen kunnen maximaal 90 minuten doorvoeren via de CDN duren.

## <a name="see-also"></a>Zie ook
* [Azure vrijdag: Azure CDN krachtige nieuwe Premium-functies](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)
* [Regels Engine overeenkomst voorwaarde en een overzicht van functies](https://msdn.microsoft.com/library/mt757336.aspx)
