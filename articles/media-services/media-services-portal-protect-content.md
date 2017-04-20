<properties 
    pageTitle="Inhoudsbeveiliging beleid met behulp van de Azure portal configureren | Microsoft Azure" 
    description="Dit artikel wordt beschreven hoe u met de portal van Azure inhoudsbeveiliging beleidsregels configureren. Het artikel ziet u ook het inschakelen van dynamische versleuteling voor de activa." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
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

# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Inhoudsbeveiliging beleid met behulp van de Azure portal configureren

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie.

## <a name="overview"></a>Overzicht

Microsoft Azure Media Services (AMS) kunt u uw media vanaf het moment vanaf uw computer via opslag, verwerking en bezorging secure. Media-Services kunt u voor het leveren van uw inhoud versleuteld dynamisch met geavanceerde versleuteling Standard (AES) (via 128-bit versleuteling toetsen), met PlayReady en/of Widevine DRM en Apple FairPlay algemene versleuteling (CENC). 

AMS biedt een service om DRM licenties te bieden en AES toetsen met geautoriseerde clients wissen. De portal van Azure kunt u één **toets/licentie beleid** voor alle typen coderingen maken.

In dit artikel wordt beschreven hoe inhoudsbeveiliging beleidsregels configureren in de portal van Azure. Het artikel ziet ook hoe u dynamische versleuteling toepassen op uw activa.

> [AZURE.NOTE]  Als u de portal van Azure klassieke beveiliging-beleid maken hebt gebruikt, is het mogelijk dat het beleid niet in de [portal van Azure](https://portal.azure.com/). Echter alle de oude beleid wordt nog steeds bestaan. U kunt ze via Azure Media Services .NET SDK of het hulpprogramma [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) controleren (als u wilt zien van het beleid, met de rechtermuisknop op de activa-gegevens (F4) -> Weergave > Klik op het tabblad inhoud sleutels -> Klik op de toets). 
> 
> Als u versleutelen met een nieuw beleid van uw activa wilt, configureert u deze met de portal van Azure, klikt u op Opslaan en opnieuw toepassen dynamische versleuteling. 

## <a name="start-configuring-content-protection"></a>Beginnen met het configureren van inhoudsbeveiliging

Ga als volgt te werk om te beginnen met het configureren van inhoudsbeveiliging, globale aan uw account AMS, gebruiken voor de portal:

1. Selecteer in de [portal van Azure](https://portal.azure.com/)uw Azure Media Services-account.
2. Selecteer **Instellingen** > **inhoudsbeveiliging**.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection001.png)
 

## <a name="keylicense-authorization-policy"></a>Toets/licentie autorisatiebeleid

AMS ondersteunt meerdere manieren gebruikers die toets of welke licentie van aanvragen te verifiëren. Het autorisatiebeleid inhoud belangrijke moet worden geconfigureerd door u en voldaan door de klant in de volgorde van de sleutel/licentie om te worden delived naar de klant. Het autorisatiebeleid belangrijke voor inhoud kan zijn een of meer autorisatie-beperkingen: **openen** of **token** beperking.

De portal van Azure kunt u één **toets/licentie beleid** voor alle typen coderingen maken.

###<a name="open"></a>Openen 

Open beperking betekent dat het systeem de sleutel voor iedereen die een belangrijke aanvraag gaat geven. Deze beperking kan handig zijn voor testdoeleinden. 

### <a name="token"></a>Token

Het beleid voor token beperkte vergezeld van een token dat is uitgegeven door een Secure Token Service (STS). Media Services ondersteunt tokens in de eenvoudige Web Tokens (SWT) en indeling van JSON Web Token (JWT). Media Services biedt geen Secure Token Services. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS naar probleem tokens. De STS moeten worden geconfigureerd voor het maken van een token ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie token beperking. De belangrijkste bezorgingsservice Media Services wordt de gevraagde sleutel (of de licentie) terug naar de client als het token geldig is en de claims in de token overeenkomen die geconfigureerd voor het sleutel (of de licentie).

Wanneer het beleid voor het configureren van de token worden beperkt, moet u de primaire verificatie-toets, uitgever en publiek parameters. De primaire verificatie-sleutel bevat de sleutel die het token is ondertekend met, uitgever is de beveiligde token service die het token problemen. Het publiek (ook wel bereik genoemd) worden de bedoeling het token of de resource het token gemachtigd voor toegang tot. De belangrijkste bezorgingsservice Media Services is gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady rechtensjabloon

Zie voor gedetailleerde informatie over de rechtensjabloon PlayReady, [Media Services PlayReady licentie sjabloon overzicht](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Permanente niet

Als u een licentie hebt geconfigureerd als tijdelijke, is het alleen gehouden in het geheugen terwijl de licentie die wordt gebruikt door de speler.  

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Permanente

Als u de licentie als permanente configureert, wordt deze opgeslagen in permanente opslagruimte op de client.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine rechtensjabloon

Zie voor gedetailleerde informatie over de rechtensjabloon Widevine, [Widevine licentie sjabloon overzicht](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Eenvoudige

Wanneer u **eenvoudige**selecteert, wordt de sjabloon gemaakt met alle standaardwaarden waarden.

### <a name="advanced"></a>Geavanceerde

Zie voor meer informatie over de optie vooraf van Widevine configuraties, [in dit](media-services-widevine-license-template-overview.md) onderwerp.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay configuratie

Om te schakelen FairPlay versleuteling, moet u de App-certificaat en de toepassing geheim sleutel (ASK) tot en met de optie FairPlay configuratie opgeven. Zie [Dit](media-services-protect-hls-with-fairplay.md) artikel voor gedetailleerde informatie over de vereisten en FairPlay configuratie.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Dynamische versleuteling toepassen op uw activa

Om te profiteren van dynamische versleuteling, moet u als volgt te werk:

- Coderen het bronbestand in een reeks geavanceerde-bitrate MP4-bestanden.
- Ten minste één op aanvraag streaming eenheid voor het streaming eindpunt waaruit u van plan bent om uit te voeren van uw inhoud downloaden. Lees [hoe u het op aanvraag streaming gereserveerde eenheden schalen](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Selecteer een actief dat u wilt versleutelen

Al uw activa, selecteert u **Instellingen** > **activa**.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Versleutelen met AES of DRM

Nadat u op **versleutelen** op activa drukt, verschijnt er met twee keuzes: **AES** of **DRM**. 

#### <a name="aes"></a>AES

AES wissen belangrijke versleuteling worden ingeschakeld op alle streaming protocollen: vloeiende Streaming, HLS en MPEG-streepje.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM

Wanneer u het tabblad DRM selecteert, verschijnt er verschillende mogelijkheden van inhoudsbeveiliging beleid (die u moet nu hebt geconfigureerd) + een set protocollen voor streaming.

- **PlayReady en Widevine met MPEG-streepje** -, worden uw stream MPEG-streepje met PlayReady en Widevine DRMs dynamisch gecodeerd.
- **PlayReady en Widevine met MPEG-streepje + FairPlay met HLS** - wordt dynamisch versleutelt u MPEG-streepje stream met PlayReady en Widevine DRMs. Uw gegevensstromen HLS met FairPlay wordt ook worden versleuteld.
- **PlayReady alleen voor gebruik met vloeiende Streaming, HLS en MPEG-streepje** - wordt dynamisch versleutelt u vloeiende Streaming, HLS, MPEG-streepje gegevensstromen met PlayReady DRM.
- **Widevine alleen voor gebruik met MPEG-streepje** -, u MPEG-streepje met Widevine DRM dynamisch worden gecodeerd.
- **FairPlay alleen voor gebruik met HLS** - wordt dynamisch versleutelt u de stream HLS FairPlay.

Om te schakelen FairPlay versleuteling, moet u het certificaat van de App en de toepassing geheim sleutel (ASK) verstrekt via de optie FairPlay configuratie van het blad inhoudsbeveiliging-instellingen.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Nadat u de selectie versleuteling maakt, drukt u op **toepassen**.

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





