<properties
    pageTitle="Een Account opslag integreren met CDN | Microsoft Azure"
    description="Informatie over het gebruik van de Azure inhoud bezorging netwerk (CDN) voor het leveren van hoge bandbreedte inhoud door caching van BLOB's uit de opslag van Azure."
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


# <a name="integrate-a-storage-account-with-cdn"></a>Een Account opslag integreren met CDN

CDN kan worden ingeschakeld voor inhoud van de cache van uw Azure-opslag. Deze biedt een globale oplossing voor het leveren van hoge bandbreedte inhoud door caching van BLOB's en statische inhoud van de exemplaren die berekeningscluster bij fysieke knooppunten in de Verenigde Staten, Europa, Azië, Australië en Zuid-Amerika ontwikkelaars.


## <a name="step-1-create-a-storage-account"></a>Stap 1: Een opslag-account maken

Gebruik de volgende procedure om een nieuwe opslag-account voor een Azure abonnement te maken. Een account opslagruimte biedt toegang tot Azure storage-services. Het account opslag staat voor het hoogste niveau van de naamruimte voor toegang tot elk van de onderdelen van de opslag van Azure-service: Blob-services, wachtrijservices en services van de tabel. Raadpleeg de [Inleiding tot Microsoft Azure Storage](../storage/storage-introduction.md)voor meer informatie.

Als u wilt een opslag-account hebt gemaakt, moet u de service-beheerder of de beheerder van een collega voor het abonnement gekoppeld.

> [AZURE.NOTE] Er zijn verschillende methoden die u gebruiken kunt om een opslag-account, met inbegrip van de Azure-Portal en Powershell te maken.  Voor deze zelfstudie voortaan we de Portal Azure gebruikt.  

**Een account opslagruimte voor een abonnement op Azure maken**

1.  Meld u aan bij de [Portal van Azure](https://portal.azure.com).
2.  Selecteer in de linkerbovenhoek, de optie **Nieuw**. Klik in het dialoogvenster **Nieuw** Selecteer **gegevens + opslagruimte**, klik **opslag-account**.

    Het blad **opslag-account maken** wordt weergegeven.

    ![Opslag-Account maken][create-new-storage-account]

4. Typ in het veld **naam** de subdomeinnaam van een. Dit item kan bevatten 3-24 kleine letters en cijfers.

    Deze waarde wordt de hostnaam van de in de URI die wordt gebruikt om Blob, wachtrij of tabel resources voor het abonnement. Als u wilt het adres van een resource container in de Blob-service, gebruikt u een URI in de volgende notatie, waar * &lt;StorageAccountLabel&gt; * verwijst naar de waarde die u hebt opgegeven in **een URL Enter**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Belangrijke:** Het URL-label het subdomein van het account opslag URI formulieren en moet uniek zijn alle gehoste services in Azure wordt aangegeven.

    Deze waarde wordt ook gebruikt als de naam van dit account opslag in de portal of bij het openen van dit account via programmacode.

5. Laat de standaardinstellingen voor **implementatiemodel**, **type Account**, **prestaties**en **herhaling**. 

6. Selecteer het **abonnement** dat het opslag-account wordt gebruikt met.

7. Selecteer of een **Resourcegroep**maken.  Zie [overzicht van de Azure resourcemanager](azure-resource-manager/resource-group-overview.md#resource-groups)voor meer informatie over resourcegroepen.

8. Selecteer een locatie voor uw account opslag.

8. Klik op **maken**. Het proces voor het maken van het account voor de opslag van kan enkele minuten duren om te voltooien.


## <a name="step-2-create-a-new-cdn-profile"></a>Stap 2: Een nieuw CDN-profiel maken

Een profiel CDN is een verzameling CDN-eindpunten.  Elk profiel bevat een of meer CDN-eindpunten.  U kunt meerdere profielen gebruiken om te organiseren van uw eindpunten CDN op internet domein, webtoepassing of andere criteria.

> [AZURE.TIP] Als u al een CDN-profiel dat u wilt gebruiken voor deze zelfstudie hebt, gaat u verder met [stap 3](#step-3-create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Stap 3: Maak een nieuwe CDN-eindpunt

**Een nieuwe CDN-eindpunt voor uw account opslag maken**

1. Ga in de [Beheerportal van Azure](https://portal.azure.com)aan uw profiel CDN.  U mogelijk hebt vastgemaakt deze aan het dashboard in de vorige stap.  Als u niet, u vinden kunt door te **Bladeren**en vervolgens **CDN-profielen**klikken en te selecteren in het profiel dat u van plan om uw eindpunt aan toe.

    Het blad CDN-profiel wordt weergegeven.

    ![CDN-profiel][cdn-profile-settings]

2. Klik op de knop **Eindpunt toevoegen** .

    ![Knop eindpunt toevoegen][cdn-new-endpoint-button]

    Het blad **toevoegen een eindpunt** wordt weergegeven.

    ![Eindpunt blade toevoegen][cdn-add-endpoint]

3. Voer een **naam** voor dit CDN-eindpunt.  Deze naam wordt gebruikt voor toegang tot uw in de cache bronnen van het domein `<endpointname>.azureedge.net`.

4. Selecteer in de vervolgkeuzelijst **Origin type** *opslag*.  

5. Selecteer in de vervolgkeuzelijst **Origin hostname** uw opslag-account.

6. Laat de standaardinstellingen voor **Origin pad**, **Origin host kop**en **Protocol/Origin poort**.  U moet ten minste één protocol (HTTP of HTTPS) opgeven.

    > [AZURE.NOTE] Deze configuratie kan alle uw publiek zichtbaar containers in uw account opslag voor in cache opslaan in de CDN.  Als u beperken het bereik aan een enkele container wilt, gebruikt u **Origin pad**.  Opmerking dat de container de zichtbaarheid openbaar moet hebben.

7. Klik op de knop **toevoegen** om te maken van het nieuwe eindpunt.

8. Nadat het eindpunt is gemaakt, wordt deze weergegeven in een lijst met eindpunten voor het profiel. De lijstweergave ziet u de URL die u wilt gebruiken voor toegang tot inhoud in cache, alsmede het oorspronkelijke domein.

    ![CDN-eindpunt][cdn-endpoint-success]

    > [AZURE.NOTE] Het eindpunt wordt onmiddellijk pas weer beschikbaar voor gebruik.  Het kan maximaal 90 minuten duren voordat de registratie via het netwerk CDN doorgeven. Gebruikers die u probeert te gebruiken van de domeinnaam CDN onmiddellijk wordt statuscode 404 totdat de inhoud beschikbaar via de CDN is.


## <a name="step-4-access-cdn-content"></a>Stap 4: Access CDN inhoud

Voor toegang tot in de cache van de inhoud van de CDN, gebruikt u de CDN-URL die u in de portal. Het adres van een in de cache blob worden ongeveer als volgt uit:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Nadat u u CDN toegang tot een opslag-account of service gehost, worden alle openbaar objecten in aanmerking komen voor caching van CDN rand. Als u een object dat momenteel in cache in de CDN opgeslagen is wijzigt, wordt de nieuwe inhoud pas weer beschikbaar via de CDN totdat de inhoud van de CDN worden vernieuwd wanneer de in de cache opgeslagen inhoud time to live verstreken.

## <a name="step-5-remove-content-from-the-cdn"></a>Stap 5: Inhoud verwijderen uit de CDN

Als u niet langer wilt cache van een object in de Azure inhoud bezorging netwerk (CDN), kunt u een van de volgende stappen uitvoeren:

-   U kunt de container aanbrengen privé in plaats van openbare. Zie [anonieme leestoegang tot containers en BLOB's beheren](../storage/storage-manage-access-to-resources.md) voor meer informatie.
-   U kunt uitschakelen of verwijderen van het CDN-eindpunt met behulp van de Portal Management.
-   U kunt uw gehoste service niet meer reageert op verzoeken om het object wijzigen.

Een object dat al in de cache in de CDN blijft in de cache totdat de periode time to live voor het object is verlopen of het eindpunt wordt verwijderd. Wanneer de time-to-live verstreken, wordt de CDN gecontroleerd of het eindpunt CDN nog geldig is en het object nog steeds anoniem toegankelijk zijn. Als dit nog niet hebt geklikt, klikt u vervolgens het object wordt niet meer in de cache.


## <a name="additional-resources"></a>Aanvullende informatie

-   [CDN inhoud toewijzen aan een aangepast domein](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
