<properties
    pageTitle="Problemen met Azure CDN eindpunten retourneren 404 status | Microsoft Azure"
    description="Problemen met 404 antwoord codes met Azure CDN-eindpunten."
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
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Problemen oplossen CDN eindpunten 404 statussen retourneren

In dit artikel vindt u problemen oplossen met [CDN eindpunten](cdn-create-new-endpoint.md) 404 fouten worden geretourneerd.

Als u meer hulp op een willekeurige plaats in dit artikel nodig hebt, kunt u contact opnemen met de Azure experts op [het MSDN-Azure en de stapel overloop-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident Azure ondersteuning indienen. Ga naar de [site van Azure-ondersteuning](https://azure.microsoft.com/support/options/) en klik op **Ondersteuning krijgen**.

## <a name="symptom"></a>Symptoom

U kunt een CDN-profiel en een eindpunt hebt gemaakt, maar de inhoud niet compatibel te zijn beschikbaar op de CDN.  Gebruikers die proberen te krijgen tot uw inhoud via de URL CDN ontvangen HTTP 404 statuscodes. 

## <a name="cause"></a>Oorzaak

Er zijn verschillende mogelijke oorzaken, waaronder:

- De oorsprong van het bestand is zichtbaar voor de CDN niet
- Het eindpunt is onjuist geconfigureerd, de oorzaak van het CDN om te zoeken op de verkeerde plaats
- De host is de host van de koptekst van de CDN weigeren
- Het eindpunt nog niet tijd doorgeven overal in de CDN had

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

> [AZURE.IMPORTANT] Na het maken van een CDN-eindpunt, is deze niet direct beschikbaar voor gebruik, zoals het duurt voor de registratie tot en met de CDN doorgeven.  Voor <b>Azure CDN van Akamai</b> profielen, is het doorgeven van gewoonlijk is voltooid binnen één minuut.  Voor <b>Azure CDN uit Verizon</b> profielen doorgeven duurt meestal binnen 90 minuten, maar in sommige gevallen kan langer duren.  Als u de stappen in dit document en u steeds 404 antwoorden krijgt nog, kunt u een paar uur om te controleren opnieuw voordat u opent een ondersteuningsticket wachten.

### <a name="check-the-origin-file"></a>Kijk of het bestand origin

Eerst we moet controleren of de die het bestand dat we in de cache wilt is beschikbaar op onze origin en openbaar toegankelijk is.  De snelste manier om dit is een browser openen in een privé-In- of Incognito sessie en Ga rechtstreeks naar het bestand.  Alleen Typ of plak de URL in het adresvak en kijk als die resulteert in het bestand dat u verwacht.  In dit voorbeeld ik ga gebruik van een bestand dat ik heb in een Azure Storage account, die toegankelijk is bij `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Zoals u ziet, wordt de test met succes doorgegeven.

![Succes!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Dit is de snelste en eenvoudigste manier om te controleren of dat uw bestand openbaar beschikbaar is, kunnen sommige netwerkconfiguraties in uw organisatie kunt u de indruk dat dit bestand openbaar beschikbaar is wanneer deze is, in feite alleen zichtbaar voor gebruikers van uw netwerk (ook als die wordt gehost in Azure wordt aangegeven).  Als u een externe browser waaruit u testen kunt, zoals een mobiel apparaat dat niet met het netwerk van uw organisatie of een virtuele machine in Azure wordt aangegeven verbonden is, is dat is aanbevolen.

### <a name="check-the-origin-settings"></a>Controleer de instellingen origin

Nu dat het bestand zich bevindt op internet openbaar hebt gecontroleerd, moeten we onze origin-instellingen te controleren.  Klik in de [Portal van Azure](https://portal.azure.com)Blader naar uw profiel CDN en klik op het eindpunt dat u bent oplossen.  Klik in het resulterende **eindpunt** blad, op de oorsprong.  

![Eindpunt blade met origin is gemarkeerd](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Het blad **Origin** wordt weergegeven. 

![Origin blade](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Origin type en hostname

Controleer of dat het **type Origin** juist is en controleer of de **oorsprong hostname**.  In mijn voorbeeld `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, het gedeelte hostname van de URL is `cdndocdemo.blob.core.windows.net`.  Zoals u in de schermopname ziet, is dat juist.  Voor Azure opslag, Web App en Cloudservice oorsprong is het veld **Origin hostname** een vervolgkeuzelijst, we hoeft dus niet bang te zijn deze correct spelling.  Als u een aangepaste origin gebruikt, is het echter *absoluut kritieke* die uw hostname juist is gespeld!

#### <a name="http-and-https-ports"></a>HTTP- en HTTPS-poorten

Het andere wat u moet bekijkt u hier is uw **HTTP** en **HTTPS-poorten**.  In de meeste gevallen 80 en 443 juist zijn en u geen wijzigingen vereist.  Echter als de oorspronkelijke server op een andere poort luistert, die moet worden hier weergegeven.  Als u niet precies weet, vindt u op de URL voor uw bestand origin.  De HTTP- en HTTPS-specificaties Geef poort 80 en 443 als de standaardinstellingen. In mijn URL, `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, een poort niet is opgegeven, zodat de standaardinstelling van 443 gebruikt en Mijn instellingen juist zijn.  

Stel echter dat de URL voor uw origin-bestand dat u eerder hebt getest is `http://www.contoso.com:8080/file.txt`.  Opmerking de `:8080` aan het einde van het segment hostname.  Geeft u aan de browser gebruiken poort `8080` verbinding maken met de webserver op `www.contoso.com`, zodat u moet 8080 invoeren in het veld **HTTP poort** .  Het is belangrijk te weten dat deze poortinstellingen alleen van invloed zijn op welke poort gebruikmaakt van het eindpunt gegevens ophalen uit het oorspronkelijke.

> [AZURE.NOTE] **Azure CDN van Akamai** eindpunten toestaan niet het volledige bereik TCP-poorten voor oorspronkelijke bestanden.  Zie [Azure CDN van Akamai toegestane Origin poorten](https://msdn.microsoft.com/library/mt757337.aspx)voor een lijst van origin poorten die niet zijn toegestaan.  
  
### <a name="check-the-endpoint-settings"></a>Controleer de instellingen van het eindpunt

Klik op de knop **configureren** weer op het blad **eindpunt** .

![Eindpunt blade met gemarkeerde knop voor configureren](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Van het eindpunt **configureren** blade wordt weergegeven.

![Blade configureren](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protocollen

Voor **protocollen**, Controleer of het protocol dat wordt gebruikt door de clients is geselecteerd.  Hetzelfde protocol gebruikt door de client wordt worden gebruikt voor toegang tot de oorsprong, dus het is belangrijk dat de poorten origin is geconfigureerd in de vorige sectie.  Het eindpunt luistert alleen op de standaard HTTP en HTTPS-poorten (80 en 443), ongeacht de poorten origin.

Laten we terug naar het hypothetische voorbeeld met `http://www.contoso.com:8080/file.txt`.  Zoals u onthouden kunt, Contoso opgegeven `8080` als hun HTTP poort, maar Stel ook dat ze opgegeven `44300` als hun HTTPS-poort.  Als deze een eindpunt met de naam gemaakt `contoso`, hun CDN eindpunt hostname zou `contoso.azureedge.net`.  Een aanvraag voor `http://contoso.azureedge.net/file.txt` is een HTTP-aanvraag, zodat het eindpunt er HTTP op poort 8080 gebruiken zou om op te halen uit de oorsprong.  Beveiligde aanvragen via HTTPS, `https://contoso.azureedge.net/file.txt`, zou het eindpunt HTTPS gebruik op poort 44300 veroorzaken wanneer bij het ophalen of het bestand in het oorspronkelijke.

#### <a name="origin-host-header"></a>Origin host koptekst

De **oorsprong host koptekst** is de waarde van host koptekst verzonden naar de oorsprong met elk verzoek om.  In de meeste gevallen moet dit hetzelfde als de **Origin hostname** we eerder hebt geverifieerd.  Een onjuiste waarde in dit veld niet in het algemeen toe leiden dat 404 statussen, maar het is waarschijnlijk een andere 4xx statussen die, afhankelijk van wat het oorspronkelijke verwacht.

#### <a name="origin-path"></a>Origin pad

Ten slotte, moeten we onze **Origin pad**verifiëren.  Standaard is deze is leeg.  U moet dit veld alleen gebruiken als u wilt het bereik van de bronnen origin gehoste voor die u beschikbaar wilt maken op de CDN beperken.  

Bijvoorbeeld in mijn eindpunt wil ik alle resources op mijn account opslag kan worden gecontroleerd, zodat ik **Origin pad** leeg blijven.  Dit betekent dat een verzoek om `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` resulteert in een verbinding uit mijn eindpunt naar `cdndocdemo.core.windows.net` die aanvraagt `/publicblob/lorem.txt`.  Daarnaast een aanvraag voor `https://cdndocdemo.azureedge.net/donotcache/status.png` resulteert in het aanvragen van een eindpunt `/donotcache/status.png` vanaf de oorsprong.

Maar wat gebeurt er als ik wil de CDN voor elke pad op mijn origin gebruiken?  Stel dat I alleen wilt laten zien het `publicblob` pad.  Als ik */publicblob* in mijn veld **Origin pad invoert** , waardoor het eindpunt */publicblob* voordat u elk verzoek verwijzingen naar de oorsprong invoegen.  Dit betekent dat de aanvraag voor `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` nu daadwerkelijk duurt het verzoek om deel van de URL, `/publicblob/lorem.txt`, en toe te voegen `/publicblob` naar het begin. Het resultaat is een aanvraag voor `/publicblob/publicblob/lorem.txt` vanaf de oorsprong.  Als dat het pad naar een bestand niet wordt opgelost, retourneert de oorsprong een 404 status.  De juiste URL om op te halen lorem.txt in dit voorbeeld is werkelijk `https://cdndocdemo.azureedge.net/lorem.txt`.  Opmerking die we niet het pad */publicblob* helemaal, bevat omdat het deel van het verzoek van de URL `/lorem.txt` en voegt het eindpunt `/publicblob`, hetgeen in `/publicblob/lorem.txt` het verzoek worden doorgegeven aan de oorsprong.
