De Domain Name System (DNS) wordt gebruikt om te zoeken van resources op internet. Bijvoorbeeld wanneer u een webadres van de app in de browser voert, of klikt u op een koppeling op een webpagina, gebruikt het DNS voor het vertalen van het domein in een IP-adres. Het IP-adres is ongeveer zoals een adres, maar het is niet erg menselijke vriendelijke. Het is bijvoorbeeld veel gemakkelijker te onthouden een DNS-naam zoals **contoso.com** dan de moet een IP-adres, zoals 192.168.1.88 of 2001:0:4137:1f67:24a2:3888:9cce:fea3 onthouden.

De DNS-systeem is gebaseerd op *records*. Records koppelen een specifieke *naam*, bijvoorbeeld **contoso.com**, met een IP-adres of een andere DNS-naam. Wanneer een toepassing, zoals een webbrowser, u een naam in DNS zoekt, vindt u de record, en wat deze wijst naar als het adres gebruikt. Als de waarde naar verwijst een IP-adres is, wordt die waarde in de browser gebruiken. Als deze naar een andere DNS-naam verwijst, klikt u vervolgens heeft de toepassing opnieuw moet resolutie. Alle mailnamen omzetten wordt uiteindelijk eindigt op een IP-adres.

Wanneer u een web-app in de App Service maakt, wordt een DNS-naam wordt automatisch toegewezen aan de web-app. Deze naam heeft de vorm van ** &lt;yourwebappname&gt;. azurewebsites.net**. Er is ook een virtueel IP-adres beschikbaar maken voor gebruik bij het maken van DNS-records, zodat u kunt records die naar verwijzen maken de **. azurewebsites.net**, of kunt u verwijzen naar het IP-adres.

> [AZURE.NOTE] Als u verwijderen en opnieuw uw web-app maken of wijzigen van de App Service abonnement-modus op **gratis** nadat deze is ingesteld op **eenvoudige**, **gedeeld**of **standaard**het IP-adres van uw web-app gewijzigd.

Er zijn ook meerdere soorten records, elk voorzien van hun eigen functies en tekortkomingen, maar voor WebApps we alleen om ervoor te zorgen over twee, *A* en *CNAME* -records.

###<a name="address-record-a-record"></a>Adresrecord (een record)

Een A-record een domein, zoals **contoso.com** of **www.contoso.com**, *of een jokertekendomein* , zoals kaarten ** \*. contoso.com**, een IP-adres. In het geval van een WebApp in de App Service adres het virtuele IP van de service of een bepaald IP-van die u voor uw web-app hebt gekocht.

Er zijn de belangrijkste voordelen van een A-record over een CNAME-record:

* U kunt een hoofddomein zoals **contoso.com** toewijzen aan een IP-adres; veel Registrar dat alleen deze nu een records

* U kunt één item met een jokerteken, zoals hebt ** \*. contoso.com**, waarin zou verwerken van aanvragen voor meerdere subdomeinen zoals **mail.contoso.com**, **blogs.contoso.com**of **www.contso.com**.

> [AZURE.NOTE] Aangezien een A-record is toegewezen aan een statisch IP-adres, kan deze wijzigingen automatisch naar het IP-adres van uw web-app omzetten. Een IP-adres voor gebruik met een records weergegeven wanneer u de instellingen voor de aangepaste domeinnaam voor uw web-app configureren; Deze waarde kan echter wijzigen als u verwijderen en opnieuw uw web-app maken of wijzigen van de modus voor het plannen van App Service naar achteren verplaatsen op **gratis**.

###<a name="alias-record-cname-record"></a>Alias record (CNAME-record)

Een *specifieke* DNS-naam, zoals **mail.contoso.com** of **www.contoso.com**, koppelt een CNAME-record aan een andere domeinnaam wordt (canoniek). In het geval van App-Service Web Apps, de canonieke domeinnaam is de ** &lt;yourwebappname >. azurewebsites.net** de naam van het domein van uw web-app. Zodra u hebt gemaakt, de CNAME Hiermee maakt u een alias voor de ** &lt;yourwebappname >. azurewebsites.net** domeinnaam. De CNAME-vermelding worden opgelost aan het IP-adres van uw ** &lt;yourwebappname >. azurewebsites.net** domeinnaam automatisch, zodat als het IP-adres van de web-app wijzigt, hoeft u geen actie te ondernemen.

> [AZURE.NOTE] Sommige domeinregistrars dat alleen u subdomeinen toewijzen bij gebruik van een CNAME-record, zoals **www.contoso.com**, en niet hoofdsite namen, zoals **contoso.com**. Zie voor meer informatie over de CNAME-records, de documentatie van uw domeinregistrar, <a href="http://en.wikipedia.org/wiki/CNAME_record">de Wikipedia-vermelding in de CNAME-record</a>of het document <a href="http://tools.ietf.org/html/rfc1035">IETF Domain Names - implementatie en specificatie</a> .

###<a name="web-app-dns-specifics"></a>Web app DNS-details

Gebruik van een A-record met Web Apps, moet u eerst een van de volgende TXT-records maken:

* **Voor het hoofddomein** - A DNS TXT-record van **@** naar ** &lt;yourwebappname&gt;. azurewebsites.net**.

* **Voor een specifiek onderliggend domein** - A DNS-naam van ** &lt;subdomein >** naar ** &lt;yourwebappname&gt;. azurewebsites.net**. Bijvoorbeeld **blogs** als de A-record is bedoeld voor **blogs.contoso.com**.

* **Voor de jokertekens sub-dodmains** - A DNS TXT-record van *** naar ** &lt;yourwebappname&gt;. azurewebsites.net**.

Deze TXT-record wordt gebruikt om te bevestigen dat u eigenaar bent van het domein dat u wilt gebruiken. Dit is naast het maken van een A-record die wijst naar het virtuele IP-adres van uw web-app.

U vindt het IP-adres en **. azurewebsites.net** namen voor uw web-app door de volgende stappen uit te voeren:

1. Open de [Portal van Azure](https://portal.azure.com)in uw browser.

2. Klik op de naam van uw web-app in het blad **Web Apps** en selecteer vervolgens **aangepaste domeinen** vanaf de onderkant van de pagina.

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. In het blad **aangepaste domeinen** ziet u het virtuele IP-adres. Bewaar deze gegevens, zoals deze wordt gebruikt bij het maken van DNS-records

    ![](./media/custom-dns-web-site/virtual-ip-address.png)

    > [AZURE.NOTE] U aangepaste domeinnamen die niet gebruiken met een **gratis** web-app en de App-serviceplan **gedeeld**, **eenvoudige**, **standaard**of **Premium** laag moet bijwerken. Prijzen niveaus, waaronder over het wijzigen van de prijzen laag van uw web-app, Zie [hoe u de schaal van de WebApps](../articles/web-sites-scale.md)voor meer informatie over de App Service-abonnement.
