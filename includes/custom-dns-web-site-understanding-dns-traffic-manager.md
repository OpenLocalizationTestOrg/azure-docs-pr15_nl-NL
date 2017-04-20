De Domain Name System (DNS) wordt gebruikt om te zoeken van zaken op internet. Bijvoorbeeld wanneer u een adres in uw browser invoert, of klik op een koppeling op een webpagina, gebruikt het DNS voor het vertalen van het domein in een IP-adres. Het IP-adres is ongeveer zoals een adres, maar het is niet erg menselijke vriendelijke. Het is bijvoorbeeld veel gemakkelijker te onthouden een DNS-naam zoals **contoso.com** dan te onthouden een IP-adres, zoals 192.168.1.88 of 2001:0:4137:1f67:24a2:3888:9cce:fea3.

De DNS-systeem is gebaseerd op *records*. Records koppelen een specifieke *naam*, bijvoorbeeld **contoso.com**, met een IP-adres of een andere DNS-naam. Wanneer een toepassing, zoals een webbrowser, u een naam in DNS zoekt, vindt u de record, en wat deze wijst naar als het adres gebruikt. Als de waarde naar verwijst een IP-adres is, wordt die waarde in de browser gebruiken. Als deze naar een andere DNS-naam verwijst, klikt u vervolgens heeft de toepassing opnieuw moet resolutie. Alle naamresolutie wordt uiteindelijk eindigt op een IP-adres.

Wanneer u een Azure-Website hebt gemaakt, wordt een DNS-naam wordt automatisch toegewezen aan de site. Deze naam heeft de vorm van ** &lt;yoursitename&gt;. azurewebsites.net**. Wanneer u uw website als een eindpunt Azure verkeer Manager toevoegen, uw website is, klikt u vervolgens toegankelijk is via de ** &lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domein.

> [AZURE.NOTE] Als uw website is geconfigureerd als een eindpunt verkeer Manager, gebruikt u de **. trafficmanager.net** adres bij het maken van DNS-records.

> U kunt CNAME-records met verkeer Manager alleen gebruiken

Er zijn ook meerdere soorten records, elk voorzien van hun eigen functies en tekortkomingen, maar voor websites die zijn geconfigureerd voor het als verkeer Manager eindpunten, wordt alleen om ervoor te zorgen over een bepaald; *CNAME* -records.

###<a name="cname-or-alias-record"></a>CNAME- of Alias record

Een *specifieke* DNS-naam, zoals **mail.contoso.com** of **www.contoso.com**, koppelt een CNAME-record aan een andere domeinnaam wordt (canoniek). In geval van Azure Websites met verkeer Manager, de canonieke domeinnaam is de ** &lt;Mijntoep >. trafficmanager.net** de naam van het domein van uw profiel verkeer Manager. Zodra u hebt gemaakt, de CNAME Hiermee maakt u een alias voor de ** &lt;Mijntoep >. trafficmanager.net** domeinnaam. De CNAME-vermelding worden opgelost aan het IP-adres van uw ** &lt;Mijntoep >. trafficmanager.net** domeinnaam automatisch, zodat als het IP-adres van de website wijzigt, hoeft u geen actie te ondernemen.

Zodra het verkeer binnenkomt bij verkeer Manager, stuurt deze vervolgens het verkeer naar uw website, met de methode die is geconfigureerd voor van taakverdeling. Dit is volledig transparant voor bezoekers van uw website. Zien zij alleen de aangepaste domeinnaam in hun browser.

> [AZURE.NOTE] Sommige domeinregistrars dat alleen u subdomeinen toewijzen bij gebruik van een CNAME-record, zoals **www.contoso.com**, en niet hoofdsite namen, zoals **contoso.com**. Zie voor meer informatie over de CNAME-records, de documentatie van uw domeinregistrar, <a href="http://en.wikipedia.org/wiki/CNAME_record">de Wikipedia-vermelding in de CNAME-record</a>of het document <a href="http://tools.ietf.org/html/rfc1035">IETF Domain Names - implementatie en specificatie</a> .
