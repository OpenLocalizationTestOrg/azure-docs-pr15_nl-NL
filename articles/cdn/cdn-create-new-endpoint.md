<properties
     pageTitle="Gebruik van Azure CDN | Microsoft Azure"
     description="Dit onderwerp wordt uitgelegd hoe u het netwerk van de inhoud bezorging (CDN) voor Azure inschakelen. De zelfstudie begeleidt bij het maken van een nieuw CDN-profiel en eindpunt."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Azure CDN gebruiken  

In dit onderwerp worden doorlopen het inschakelen van Azure CDN door een nieuwe CDN profiel en eindpunt te maken.

>[AZURE.IMPORTANT] Zie voor een inleiding tot CDN werking, evenals een lijst met functies, [CDN overzicht](./cdn-overview.md).

## <a name="create-a-new-cdn-profile"></a>Een nieuw CDN-profiel maken

Een profiel CDN is een verzameling CDN-eindpunten.  Elk profiel bevat een of meer CDN-eindpunten.  U kunt meerdere profielen gebruiken om te organiseren van uw eindpunten CDN op internet domein, webtoepassing of andere criteria.

> [AZURE.NOTE] Standaard is een abonnement voor één Azure beperkt tot acht CDN-profielen. Elk CDN-profiel is beperkt tot tien CDN-eindpunten.
>
> CDN prijzen wordt toegepast op het niveau van de CDN-profiel. Als u een mengeling van Azure CDN prijzen niveaus gebruiken wilt, moet u meerdere CDN-profielen.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Maak een nieuwe CDN-eindpunt

**Om een nieuwe CDN-eindpunt te maken**

1. Ga in de [Portal van Azure](https://portal.azure.com)aan uw profiel CDN.  U mogelijk hebt vastgemaakt deze aan het dashboard in de vorige stap.  Als u niet, u vinden kunt door te **Bladeren**en vervolgens **CDN-profielen**klikken en te selecteren in het profiel dat u van plan om uw eindpunt aan toe.

    Het blad CDN-profiel wordt weergegeven.

    ![CDN-profiel][cdn-profile-settings]

2. Klik op de knop **Eindpunt toevoegen** .

    ![Knop eindpunt toevoegen][cdn-new-endpoint-button]

    Het blad **toevoegen een eindpunt** wordt weergegeven.

    ![Eindpunt blade toevoegen][cdn-add-endpoint]

3. Voer een **naam** voor dit CDN-eindpunt.  Deze naam wordt gebruikt voor toegang tot uw in de cache bronnen van het domein `<endpointname>.azureedge.net`.

4. Selecteer het type origin in de vervolgkeuzelijst **Origin type** .  Selecteer **opslagruimte** voor een Azure Storage-account, **cloudservice** voor een Azure-Cloudservice, **Web App** voor een Azure Web App of **aangepaste origin** voor alle andere openbaar web server origin (gehost in Azure wordt aangegeven of ergens anders).

    ![CDN origin type](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. Selecteer in de vervolgkeuzelijst **Origin hostname** of typ van uw oorspronkelijke domein.  De vervolgkeuzelijst wordt een lijst met alle beschikbare oorsprong van het type die u hebt opgegeven in stap 4.  Als u *aangepaste origin* als uw **Origin Typ**hebt geselecteerd, wordt u typt in het oorspronkelijke domein van uw aangepaste.

6. Voer in het tekstvak **Origin pad** het pad naar de bronnen die u wilt cache of niets toe te staan dat cache voor elke resource in het domein dat u hebt opgegeven in stap 5.

7. In de **koptekst van Origin host**, voert u de host kop die u wilt dat de CDN met elk verzoek om een moeten worden verzonden of laat de standaardwaarde.

    > [AZURE.WARNING] Sommige typen oorspronkelijke bestanden, zoals Azure opslag- en Web-Apps, zijn de koptekst host zodat deze overeenkomt met het domein van de oorsprong vereist. Tenzij u een oorsprong die moeten worden verschillende uit het domein op de kop van een host hebt, laat u de standaardwaarde.

8. Geef de protocollen en poorten gebruikt voor toegang tot uw resources op de oorsprong voor **Protocol** en **Origin poort**.  Ten minste één protocol (HTTP of HTTPS) moet zijn ingeschakeld.
    
    > [AZURE.NOTE] De **oorsprong poort** geldt alleen voor wat de doeleinden eindpunt gegevens ophalen uit het oorspronkelijke poort.  Het eindpunt zelf is alleen beschikbaar voor end-clients in de standaard HTTP en HTTPS-poorten (80 en 443), ongeacht de **oorsprong poort**.  
    >
    > **Azure CDN van Akamai** eindpunten toestaan niet het volledige bereik TCP-poorten voor oorspronkelijke bestanden.  Zie [Azure CDN van Akamai toegestane Origin poorten](https://msdn.microsoft.com/library/mt757337.aspx)voor een lijst van origin poorten die niet zijn toegestaan.  
    >
    > Toegang krijgen tot het CDN heeft met behoud van HTTPS de volgende beperkingen:
    > 
    > - U kunt het SSL-certificaat dat is verstrekt door de CDN moet gebruiken. Certificaten van derden worden niet ondersteund.
    > - Moet u het domein geleverde CDN (`<endpointname>.azureedge.net`) tot access HTTPS-inhoud. HTTPS-ondersteuning is niet beschikbaar voor aangepaste domeinnamen (CNAME-records), omdat de CDN aangepaste certificaten op dit moment niet ondersteunt.

9. Klik op de knop **toevoegen** om te maken van het nieuwe eindpunt.

10. Nadat het eindpunt is gemaakt, wordt deze weergegeven in een lijst met eindpunten voor het profiel. De lijstweergave ziet u de URL die u wilt gebruiken voor toegang tot inhoud in cache, alsmede het oorspronkelijke domein.

    ![CDN-eindpunt][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Het eindpunt is niet direct beschikbaar voor gebruik, zoals het duurt voor de registratie tot en met de CDN doorgeven.  Voor <b>Azure CDN van Akamai</b> profielen wordt meestal doorgeven voltooid binnen één minuut.  Voor <b>Azure CDN uit Verizon</b> profielen doorgeven duurt meestal binnen 90 minuten, maar in sommige gevallen kan langer duren.
    >    
    > Gebruikers die u probeert te gebruiken van de domeinnaam CDN voordat de eindpuntconfiguratie heeft doorgegeven aan de POP's ontvangt HTTP 404 antwoord codes.  Als het enkele uren is sinds u uw eindpunt hebt gemaakt en u bent nog steeds 404 antwoorden ontvangt, raadpleegt u [problemen oplossen CDN eindpunten 404 statussen retourneren](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Zie ook
- [Beheren in de cache gedrag van aanvragen met querytekenreeksen met de](cdn-query-string.md)
- [CDN inhoud toewijzen aan een aangepast domein](cdn-map-content-to-custom-domain.md)
- [Vooraf geladen activa op een Azure CDN-eindpunt](cdn-preload-endpoint.md)
- [Een eindpunt Azure CDN verwijderen](cdn-purge-endpoint.md)
- [Problemen oplossen CDN eindpunten 404 statussen retourneren](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
