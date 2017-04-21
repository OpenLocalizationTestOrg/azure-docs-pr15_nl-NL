# <a name="using-cdn-for-azure"></a>Met behulp van CDN voor Azure

Azure inhoud bezorging netwerk (CDN) biedt ontwikkelaars een globale oplossing om hoge bandbreedte inhoud door de cache BLOB's en statische inhoud van de exemplaren die berekeningscluster bij fysieke knooppunten in de Verenigde Staten, Europa, Azië, Australië en Zuid-Amerika te bieden. Zie voor een huidige lijst met CDN knooppunt locaties, [Azure CDN knooppunt locaties].

Deze taak bevat de volgende stappen uit:

* [Stap 1: Een opslag-account maken](#Step1)
* [Stap 2: Maak een nieuwe CDN-eindpunt voor uw account opslag](#Step2)
* [Stap 3: Toegang tot uw CDN-inhoud](#Step3)
* [Stap 4: Verwijder uw CDN-inhoud](#Step4)

De voordelen van het gebruik van CDN voor Azure-gegevens in de cache zijn onder andere:

-   Betere prestaties en functionaliteit voor eindgebruikers die ver van een inhoudsbron en toepassingen zijn gebruikt waar veel internet reizen zijn vereist voor het laden van inhoud
-   Grote gedistribueerde aanpassing aan beter eventuele momentopname hoge belasting, bijvoorbeeld aan het begin van een gebeurtenis, zoals een productlancering

Bestaande klanten van CDN kunnen nu de CDN Azure gebruiken in de [klassieke Azure-portal]. De CDN is een functie van de invoegtoepassing aan uw abonnement en heeft een apart [Facturering abonnement].

<a id="Step1"> </a>
<h2>Stap 1: Een opslag-account maken</h2>

Gebruik de volgende procedure om een nieuwe opslag-account voor een Azure abonnement te maken. Een account opslagruimte biedt toegang tot Azure storage-services. Het account opslag staat voor het hoogste niveau van de naamruimte voor toegang tot elk van de onderdelen van de opslag van Azure-service: Blob-services, wachtrijservices en services van de tabel. Zie voor meer informatie over de van Azure-opslagservices, [met behulp van de Azure opslag-Services](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Als u wilt een opslag-account hebt gemaakt, moet u de service-beheerder of de beheerder van een collega voor het abonnement gekoppeld.

> [AZURE.NOTE] Zie het onderwerp van de verwijzing [Opslag-Account maken](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) voor informatie over het uitvoeren van deze bewerking met behulp van de Azure-API voor het beheer van Service.

**Een account opslagruimte voor een abonnement op Azure maken**

1.  Meld u aan bij de [portal van Azure klassieke].
2.  Klik in de linkerbenedenhoek op **Nieuw**. Klik in het dialoogvenster **Nieuw** Selecteer **Data Services**, klik **opslag**, klikt u vervolgens **Snelle maken**.

    Het dialoogvenster **Opslag-Account maken** wordt weergegeven.

    ![Opslag-Account maken][create-new-storage-account]

4. Typ in het veld **URL** de subdomeinnaam van een. Dit item kan van 3-24 kleine letters en cijfers bevatten.

    Deze waarde wordt de hostnaam van de in de URI die wordt gebruikt om Blob, wachtrij of tabel resources voor het abonnement. Als u wilt het adres van een resource container in de Blob-service, gebruikt u een URI in de volgende notatie, waar * &lt;StorageAccountLabel&gt; * verwijst naar de waarde die u hebt opgegeven in **een URL Enter**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Belangrijke:** Het URL-label het subdomein van het account opslag URI forms en moet uniek zijn alle gehoste services in Azure wordt aangegeven.

    Deze waarde wordt ook gebruikt als de naam van dit account opslag in de portal of bij het openen van dit account via programmacode.

5.  Selecteer een regio of affiniteit groep voor de opslag-account uit de vervolgkeuzelijst **Regio/affiniteit groep** . Selecteer een groep affiniteit in plaats van een gebied als u wilt dat uw opslagservices in de dezelfde datacenter met andere services van Windows Azure dat u gebruikt. Hiermee kan de prestaties verbeteren en geen kosten egress verbonden zijn.  

    **Notitie:** Als u wilt een affiniteit-groep maken, opent u het gedeelte **Instellingen** van de beheerportal op **affiniteit groepen**en klik vervolgens op **een groep affiniteit toevoegen** of **toevoegen**. U kunt ook groepen maken en beheren affiniteit de Windows Azure-Service Management-API gebruiken. Zie voor meer informatie [bewerkingen op affiniteit groepen].

6. Selecteer in de vervolgkeuzelijst **abonnement** het abonnement dat het opslag-account wordt gebruikt met.
7.  Klik op de **opslag-Account maken**. Het proces voor het maken van het account voor de opslag van kan enkele minuten duren om te voltooien.
8.  Controleer of het account wordt weergegeven in de items weergegeven voor de **opslag** met de status **Online**om te bevestigen dat de opslag-account is gemaakt.

<a id="Step2"> </a>
<h2>Stap 2: Maak een nieuwe CDN-eindpunt voor uw account opslag</h2>

Nadat u u CDN toegang tot een opslag-account of service gehost, worden alle openbaar objecten in aanmerking komen voor caching van CDN rand. Als u een object dat momenteel in cache in de CDN opgeslagen is wijzigt, wordt de nieuwe inhoud pas weer beschikbaar via de CDN totdat de inhoud van de CDN worden vernieuwd wanneer de in de cache opgeslagen inhoud time to live verstreken.

**Een nieuwe CDN-eindpunt voor uw account opslag maken**

1. Klik in de [klassieke Azure-portal]in het navigatiedeelvenster op **CDN**.

2. Klik op het lint op **Nieuw**. Selecteer in het dialoogvenster **Nieuw** **App-Services**, en vervolgens **CDN**Selecteer **Snelle maken**.

3. Selecteer het opslag-account dat u hebt gemaakt in de vorige sectie in de lijst met beschikbare opslagruimte accounts in de vervolgkeuzelijst **Oorspronkelijke domein** . 

4. Klik op de knop **maken** als u wilt maken van het nieuwe eindpunt.

5. Nadat het eindpunt is gemaakt, wordt deze weergegeven in een lijst met eindpunten voor het abonnement. De lijstweergave ziet u de URL die u wilt gebruiken voor toegang tot inhoud in cache, alsmede het oorspronkelijke domein. 

    Het oorspronkelijke domein is de locatie waaruit de CDN in cache inhoud opgeslagen. Het oorspronkelijke domein kan zijn op een opslag-account of een cloudservice; een opslag-account wordt gebruikt voor de toepassing van dit voorbeeld. Edge-servers op basis van een cachebeheer-instelling die u opgeeft, of de standaard-methodiek van het netwerk in de cache opgeslagen opslag inhoud in. 


    > [AZURE.NOTE] De configuratie gemaakt voor het eindpunt wordt niet onmiddellijk beschikbaar zijn; het kan maximaal 60 minuten duren voordat de registratie doorgeven via het netwerk CDN. Gebruikers die u probeert te gebruiken van de domeinnaam CDN onmiddellijk wordt statuscode 400 (ongeldige aanvraag) totdat de inhoud beschikbaar via de CDN is.

<a id="Step3"> </a>
<h2>Stap 3: Access CDN inhoud</h2> 

Voor toegang tot in de cache van de inhoud van de CDN, gebruikt u de CDN-URL die u in de portal. Het adres van een in de cache blob worden ongeveer als volgt uit:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Stap 4: Inhoud verwijderen uit de CDN</h2>

Als u niet langer wilt cache van een object in de Azure inhoud bezorging netwerk (CDN), kunt u een van de volgende stappen uitvoeren:

-   Voor een Azure blob, kunt u de blob verwijderen uit de openbare container.
-   U kunt de container aanbrengen privé in plaats van openbare. Zie [Toegang beperken tot Containers en BLOB's](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) voor meer informatie.
-   U kunt uitschakelen of verwijderen van het CDN-eindpunt met behulp van de Portal Management.
-   U kunt uw gehoste service niet meer reageert op verzoeken om het object wijzigen.

Een object dat al in de cache in de CDN blijft in de cache, totdat de periode time to live voor het object verloopt. Wanneer de time-to-live verstreken, wordt de CDN gecontroleerd of het eindpunt CDN nog geldig is en het object nog steeds anoniem toegankelijk zijn. Als dit nog niet hebt geklikt, klikt u vervolgens het object wordt niet meer in de cache.

De mogelijkheid direct wissen inhoud wordt momenteel niet ondersteund op Azure-beheerportal. Neem contact op met [ondersteuning voor Azure](https://azure.microsoft.com/support/options/) als u wilt verwijderen onmiddellijk inhoud. 

## <a name="additional-resources"></a>Aanvullende informatie

-   [Het maken van een groep affiniteit in Azure wordt aangegeven]
-   [Hoe: opslag Accounts beheren voor een abonnement op Azure]
-   [Over de API voor het servicebeheer]
-   [CDN inhoud toewijzen aan een aangepast domein]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure CDN knooppunt locaties]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure klassieke portal]: https://manage.windowsazure.com/
  [Facturering abonnement]: /pricing/calculator/?scenario=full
  [Het maken van een groep affiniteit in Azure wordt aangegeven]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [Over de API voor het servicebeheer]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [CDN inhoud toewijzen aan een aangepast domein]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
