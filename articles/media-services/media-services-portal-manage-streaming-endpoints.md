<properties 
    pageTitle="Streaming eindpunten in de portal van Azure beheren | Microsoft Azure" 
    description="Dit onderwerp wordt uitgelegd hoe u streaming eindpunten in de portal van Azure beheert." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    writer="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="manage-streaming-endpoints-with-the-azure-portal"></a>Streaming eindpunten met de Azure-portal beheren

## <a name="overview"></a>Overzicht

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie. 

In Microsoft Azure Media Services vertegenwoordigt een **Eindpunt Streaming** een streaming service die rechtstreeks naar een speler clienttoepassing, of naar een inhoud bezorging netwerk (CDN) voor verdere distributie inhoud kunt aanbieden. Kunt u bovendien naadloze integratie met CDN Azure Media Services. De uitgaande stroom van een StreamingEndpoint-service kan zijn een live gegevensstroom of een video op aanvraag activa in uw Media Services-account.

Bovendien kunt u bepalen van de capaciteit van de service Endpoint Streaming worden afgehandeld groeiende bandbreedte behoeften door streaming eenheden aan te passen. Het wordt aanbevolen een of meer schaaleenheden voor toepassingen in productieomgeving toewijzen. Streaming eenheden bieden u met speciale egress capaciteit die kan worden aangeschaft in stappen van 200 Mbps zowel extra functionaliteit, waaronder: [dynamische verpakking](media-services-dynamic-packaging-overview.md), CDN-integratie en geavanceerde configuratie.

>[AZURE.NOTE]Afschrijving alleen wanneer uw eindpunt Streaming in staat uitgevoerd.

Dit onderwerp biedt een overzicht van de belangrijkste functies die worden verstrekt door Streaming eindpunten. Het onderwerp ziet u ook het gebruik van de Azure-portal voor het beheren van streaming eindpunten. Zie voor informatie over hoe u de schaal van het eindpunt streaming aanpassen, [in dit](media-services-portal-scale-streaming-endpoints.md) onderwerp.

## <a name="start-managing-streaming-endpoints"></a>Begin streaming eindpunten beheren

Ga als volgt te werk als u wilt beginnen met het beheren van streaming eindpunten voor uw account.

1. Selecteer in de [portal van Azure](https://portal.azure.com/)uw Azure Media Services-account.
2. Selecteer in het venster **Instellingen** **Streaming eindpunten**.

    ![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##<a name="adddelete-a-streaming-endpoint"></a>Een streaming-eindpunt toevoegen/verwijderen

Ga als volgt te werk als u wilt toevoegen/verwijderen streaming eindpunt met behulp van de Azure portal:

1. Als u wilt een streaming-eindpunt hebt toegevoegd, klikt u op het **+ -eindpunt** boven aan de pagina. 
2. Als u wilt een streaming eindpunt verwijderen, drukt u op de knop **verwijderen** . 

    De standaardinstelling streaming eindpunt kan niet worden verwijderd.
2. Klik op de knop **Start** om te beginnen het streaming-eindpunt.

    ![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

U kunt maximaal twee streaming eindpunten hebben al dan niet standaard. Als u nodig hebt om aan te vragen meer, raadpleegt u [quota en beperkingen](media-services-quotas-and-limitations.md).
    
##<a id="configure_streaming_endpoints"></a>Het Streaming eindpunt configureren

Eindpunt streaming, kunt u de volgende eigenschappen configureren als er ten minste 1 eenheid voor tijdschaal: 

- Toegangsbeheer
- Cache-besturingselement
- Cross site-beleid

Zie [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)voor gedetailleerde informatie over deze eigenschappen.

U kunt streaming eindpunt configureren door het volgende te doen:

1. Selecteer het streaming-eindpunt dat u wilt configureren.
1. Klik op **Instellingen**.
  
Een korte beschrijving van de velden volgt.

![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Maximale cache beleid: gebruikt voor het configureren van cache levensduur van activa served via deze streaming eindpunt. Als geen waarde is ingesteld, wordt de standaardwaarde gebruikt. De standaardwaarden kunnen ook worden gedefinieerd rechtstreeks in Azure opslag. Als Azure CDN is ingeschakeld voor het streaming eindpunt, moet u de waarde van het beleid cache niet ingesteld op minder dan 600 seconden.  

2. Toegestane IP-adressen: waarmee wordt bepaald IP-adressen die mogen verbinding maken met de gepubliceerde streaming eindpunt. Als geen IP-adressen is opgegeven, is er zou elk IP-adres verbinding kunnen maken. IP-adressen kunnen worden opgegeven als één IP-adres (bijvoorbeeld ' 10.0.0.1'), een IP-bereik met een IP-adres en een CIDR subnetmasker (bijvoorbeeld ' 10.0.0.1/22') of een IP-bereik IP-adres en een stippellijn decimaal subnetmasker (bijvoorbeeld ' 10.0.0.1(255.255.255.0)').

3. Configuratie voor Akamai handtekening koptekst verificatie: gebruikt om op te geven hoe verzoek om verificatie van handtekening-koptekst van Akamai servers is geconfigureerd. Verlooptijd is in UTC.



##<a id="enable_cdn"></a>Azure CDN-integratie inschakelen

U kunt opgeven om in te schakelen van de Azure CDN-integratie voor een eindpunt Streaming (deze is standaard uitgeschakeld.)

Aan de integratie van Azure CDN ingesteld op waar:

- Het streaming eindpunt moet ten minste één streaming per eenheid. Als u later schaaleenheden ingesteld op 0 wilt, moet u eerst de integratie CDN uitschakelen. Wanneer u een nieuwe streaming maakt is eindpunt één streaming eenheid standaard automatisch ingesteld.

- Het streaming eindpunt moet zich in een gestopt staat. Zodra de CDN wordt ingeschakeld, kunt u het streaming eindpunt starten. 

Het duurt maximaal 90 min voor de integratie Azure CDN ingeschakeld.  Het duurt maximaal twee uur totdat de wijzigingen in alle de CDN POP's actief zijn.

CDN-integratie is ingeschakeld in alle Azure datacenters: ons West, ons Oost Noord Europa, West Europa, Japan West, Japan Oost, Zuid-Oost-Azië en Oost-Azië.

Zodra deze is ingeschakeld, wordt de configuratie **Beheren in Access** wordt uitgeschakeld.

![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] Azure Media Services-integratie met Azure CDN is geïmplementeerd op **Azure CDN van Verizon**.  Als u gebruiken van **Azure CDN van Akamai** voor Azure Media Services wilt, moet u [het eindpunt handmatig configureren](../cdn/cdn-create-new-endpoint.md).  Zie voor meer informatie over de functies van Azure CDN [CDN overzicht](../cdn/cdn-overview.md).

###<a name="additional-considerations"></a>Aanvullende overwegingen

- Wanneer CDN is ingeschakeld voor een streaming eindpunt, niet clients verzoeken om inhoud rechtstreeks vanuit de oorsprong. Als u de mogelijkheid om te testen van uw inhoud met of zonder CDN nodig hebt, kunt u een andere streaming-eindpunt dat niet ingeschakeld CDN is maken.
- Uw streaming eindpunt hostname blijft dezelfde na het inschakelen van CDN. U hoeft niet te wijzigen voor uw werkstroom van de services media na CDN is ingeschakeld. De exacte dezelfde hostnaam wordt bijvoorbeeld gebruikt als uw streaming eindpunt hostname strasbourg.streaming.mediaservices.windows.net, na het inschakelen van CDN.
- Voor nieuwe streaming eindpunten, kunt u CDN inschakelen door gewoon te maken van een nieuwe eindpunt; voor bestaande streaming eindpunten moet u eerst stopt het eindpunt en schakelt het CDN.
 

Zie voor meer informatie, [aankondigen Azure Media Services-integratie met Azure CDN (inhoud bezorging netwerk)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
