#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Een aangepaste domeinnaam voor een Azure website configureren

Wanneer u een website hebt gemaakt, Azure een beschrijvende subdomein biedt aan het domein azurewebsites.net zodat uw gebruikers hebben toegang tot uw website via een URL zoals http://&lt;Mijn site >. azurewebsites.net. Als u uw websites voor gedeeld of standaardmodus configureren, kunt u uw website echter toewijzen aan uw eigen domeinnaam.

Desgewenst kunt u Azure verkeer Manager laden balance '-binnenkomende verkeer naar uw website. Zie voor meer informatie over de werking van verkeer Manager met Websites, [Bepalen Azure websites verkeer met Azure verkeer Manager][trafficmanager].

> [AZURE.NOTE] De procedures in deze taak toepassen op Azure Websites; Zie <a href="/develop/net/common-tasks/custom-dns/">een aangepaste domeinnaam in Azure configureren</a>voor Cloudservices.

> [AZURE.NOTE] De stappen in deze taak moeten u uw websites configureren voor gedeeld of standaardmodus, die mogelijk wijzigen hoeveel u voor uw abonnement zijn gefactureerd. Zie <a href="/pricing/details/web-sites/">Websites prijzen Details</a> voor meer informatie.

In dit artikel:

-   [Informatie over records CNAME en A](#understanding-records)
-   [Uw web-sites voor gedeelde standaardmodus configureren](#bkmk_configsharedmode)
-   [Aan de verkeer-beheer voor uw websites toevoegen](#trafficmanager)
-   [Een CNAME voor uw aangepaste domein toevoegen](#bkmk_configurecname)
-   [Een A-record voor uw aangepaste domein toevoegen](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Meer informatie over records CNAME en A</h2>

CNAME (of aliasrecords) en een records kunnen u een domeinnaam koppelen aan een website, maar deze allemaal anders werken.

###<a name="cname-or-alias-record"></a>CNAME- of Alias record

Een *specifiek* domein, zoals **contoso.com** of **www.contoso.com**, koppelt een CNAME-record aan een canonieke domeinnaam. In dit geval de canonieke domeinnaam is het de ** &lt;Mijntoep >. azurewebsites.net** de naam van het domein van uw website Azure of de ** &lt;Mijntoep >. trafficmgr.com** de naam van het domein van uw profiel verkeer Manager. Zodra u hebt gemaakt, de CNAME Hiermee maakt u een alias voor de ** &lt;Mijntoep >. azurewebsites.net** of ** &lt;Mijntoep >. trafficmgr.com** domeinnaam. De CNAME-vermelding worden opgelost aan het IP-adres van uw ** &lt;Mijntoep >. azurewebsites.net** of ** &lt;Mijntoep >. trafficmgr.com** domeinnaam automatisch, zodat als het IP-adres van de website wijzigt, hoeft u geen actie te ondernemen.

> [AZURE.NOTE] Sommige domeinregistrars dat alleen u subdomeinen toewijzen bij gebruik van een CNAME-record, zoals www.contoso.com, en niet hoofdsite namen, zoals contoso.com. Zie voor meer informatie over de CNAME-records, de documentatie van uw domeinregistrar, <a href="http://en.wikipedia.org/wiki/CNAME_record">de Wikipedia-vermelding in de CNAME-record</a>of het document <a href="http://tools.ietf.org/html/rfc1035">IETF Domain Names - implementatie en specificatie</a> .

###<a name="a-record"></a>Een record

Een A-record een domein, zoals **contoso.com** of **www.contoso.com**, *of een jokertekendomein* , zoals kaarten ** \*. contoso.com**, een IP-adres. In het geval van een Website Azure adres het virtuele IP van de service of een bepaald IP-van die u hebt gekocht voor uw website. Zodat de belangrijkste voordelen van een A-record over een CNAME-record is dat u kunt één item met een jokerteken, zoals * **. contoso.com**, waarin zou verzoeken om meerdere subdomeinen, zoals verwerken * *mail.contoso.com**, * *login.contoso.com**, of * *www.contso.com**.

> [AZURE.NOTE] Aangezien een A-record is toegewezen aan een statische IP-adres, kan deze wijzigingen automatisch naar het IP-adres van uw website omzetten. Een IP-adres voor gebruik met een records weergegeven wanneer u de instellingen voor de aangepaste domeinnaam voor uw website configureren; Deze waarde kan echter wijzigen als u wilt verwijderen en opnieuw maken van uw website of wijzigen van de website-modus naar achteren verplaatsen om vrij te geven.

> [AZURE.NOTE] Een records kunnen niet worden gebruikt voor taakverdeling met verkeer Manager. Zie voor meer informatie, [Bepalen Azure websites verkeer met Azure verkeer Manager][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>Uw websites voor gedeelde standaardmodus configureren</h2>

Een aangepaste domeinnaam instellen op een website is alleen beschikbaar voor de gedeelde en standaard modi voor Azure websites. Voordat u overschakelt naar een website van de gratis website naar de gedeeld of standaard website modus, moet u eerst uitgaven caps op hun plaats staan voor uw Website-abonnement verwijderen. Zie voor meer informatie over gedeelde en standaard modus prijzen, [Details prijzen][PricingDetails].

1. Open de [Beheerportal]in uw browser[portal].
2. Klik op het tabblad **Websites** op de naam van uw site.

    ![][standardmode1]

3. Klik op het tabblad **schaal** .

    ![][standardmode2]


4. Stel de modus website door te klikken op **gedeeld**in het gedeelte **Algemeen** .

    ![][standardmode3]

    > [AZURE.NOTE] Als u verkeer Manager met deze website gebruiken wilt, moet u Selecteer standaardmodus gebruiken in plaats van gedeeld.

5. Klik op **Opslaan**.
6. Wanneer u wordt gevraagd de toename van de kosten voor gedeelde modus (of voor standaardmodus als u kiest u standaard), klikt u op **Ja** als u akkoord gaat.

    <!--![][standardmode4]-->

    **Opmerking**<br />
   Als u een fout "Schaal configureren voor website 'Websitenaam' is mislukt" ontvangt, kunt u de knop details voor meer informatie.

<a name="trafficmanager"></a><h2>(Optioneel) Aan de verkeer-beheer voor uw websites toevoegen</h2>

Als u uw website met verkeer Manager gebruiken wilt, moet u de volgende stappen uitvoeren.

1. Als u nog een profiel verkeer Manager, gebruik de informatie in [een snelle maken met verkeer Manager-profiel maken] [ createprofile] u een account maakt. Opmerking de **. trafficmgr.com** domeinnaam die is gekoppeld aan uw profiel verkeer Manager. Hiermee wordt gebruikt in een volgende stap.

2. Gebruik de informatie in [toevoegen of verwijderen eindpunten] [ addendpoint] naar uw website toevoegen als een eindpunt in uw profiel verkeer Manager.

    > [AZURE.NOTE] Als uw website niet wordt vermeld bij het toevoegen van een eindpunt, controleert u of deze is geconfigureerd voor standaardmodus. U moet standaardmodus voor uw website gebruiken om te kunnen werken met verkeer Manager.

3. Meld u aan bij uw DNS--domeinregistrar website en Ga naar de pagina voor het beheer van DNS. Vindt u koppelingen of gebieden van de site die is gelabeld als **Domeinnaam**, **DNS**of **De naam van Server Management**.

4. Nu kunt u selecteren of voer CNAME-records vinden. Mogelijk moet u het recordtype in een decoratieve omlaag selecteren of gaat u naar een pagina Geavanceerde instellingen. U moet controleren of de woorden **CNAME**, **Alias**of **subdomeinen**.

5. U moet ook de alias domein of subdomein opgeven voor de CNAME. Bijvoorbeeld **www** als u wilt een alias maken voor **www.customdomain.com**.

5. U moet ook een hostnaam die de canonieke domeinnaam voor deze CNAME alias opgeven. Dit is de **. trafficmgr.com** naam voor uw website.

Bijvoorbeeld doorstuurt de volgende CNAME-record al het verkeer van **www.contoso.com** naar **contoso.trafficmgr.com**, de domeinnaam van een website:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias/Host naam/subdomein</strong></td>
<td><strong>Canonieke domein</strong></td>
</tr>
<tr>
<td>www.</td>
<td>Contoso.trafficmgr.com</td>
</tr>
</table>

Een bezoeker van **www.contoso.com** ziet nooit de waar host (contoso.azurewebsite.net), zodat het proces doorschakelen zichtbaar voor de eindgebruiker is.

> [AZURE.NOTE] Als u verkeer Manager met een website gebruikt, hoeft u niet met de stappen in de volgende secties, '**toevoegen een CNAME voor uw aangepaste domein**' en '**toevoegen een A-record voor uw aangepaste domein**'. De CNAME-record hebt gemaakt in de vorige stappen routeert binnenkomende verkeer naar verkeer Manager, dat stuurt vervolgens het verkeer door naar de website endpoint(s).

<a name="bkmk_configurecname"></a><h2>Een CNAME voor uw aangepaste domein toevoegen</h2>

Als u wilt een CNAME-record hebt gemaakt, moet u een nieuwe vermelding in de tabel DNS voor uw aangepaste domein toevoegen met behulp van hulpmiddelen voor verstrekt door uw domeinregistrar. Elk van de domeinregistrar heeft een vergelijkbaar, maar iets anders methode voor het opgeven van een CNAME-record, maar de concepten zijn hetzelfde.

1. Gebruik een van de volgende manieren om te zoeken naar de **. azurewebsite.net** domeinnaam die zijn toegewezen aan uw website.

    * Meld u aan bij de [Beheerportal van Azure][portal], selecteert u uw website **Dashboard**selecteren en zoek het **Site-URL** -fragment in het gedeelte **snel aan te duiden** .

    * Installeren en configureren van [Azure Powershell](/manage/install-and-configure-windows-powershell/)en gebruik vervolgens de volgende opdracht uit:

            get-azurewebsite yoursitename | select hostnames

    * Installeren en configureren van de [Azure Command Line Interface](/manage/install-and-configure-cli/)en gebruik vervolgens de volgende opdracht uit:

            azure site domain list yoursitename

    Deze opslaan **. azurewebsite.net** naam, zoals deze wordt gebruikt in de volgende stappen.

3. Meld u aan bij uw DNS--domeinregistrar website en Ga naar de pagina voor het beheer van DNS. Vindt u koppelingen of gebieden van de site die is gelabeld als **Domeinnaam**, **DNS**of **De naam van Server Management**.

4. Nu kunt u selecteren of voer CNAME-records vinden. Mogelijk moet u het recordtype in een decoratieve omlaag selecteren of gaat u naar een pagina Geavanceerde instellingen. U moet controleren of de woorden **CNAME**, **Alias**of **subdomeinen**.

5. U moet ook de alias domein of subdomein opgeven voor de CNAME. Bijvoorbeeld **www** als u wilt een alias maken voor **www.customdomain.com**. Als u een alias maken voor het hoofddomein wilt, deze wordt mogelijk weergegeven als de '**@**' symbool in uw domeinregistrar de DNS-hulpprogramma's.

5. U moet ook een hostnaam die de canonieke domeinnaam voor deze CNAME alias opgeven. Dit is de **. azurewebsite.net** naam voor uw website.

Bijvoorbeeld doorstuurt de volgende CNAME-record al het verkeer van **www.contoso.com** naar **contoso.azurewebsite.net**, de domeinnaam van een website:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias/Host naam/subdomein</strong></td>
<td><strong>Canonieke domein</strong></td>
</tr>
<tr>
<td>www.</td>
<td>Contoso.azurewebsite.NET</td>
</tr>
</table>

Een bezoeker van **www.contoso.com** ziet nooit de waar host (contoso.azurewebsite.net), zodat het proces doorschakelen zichtbaar voor de eindgebruiker is.

> [AZURE.NOTE] Het bovenstaande voorbeeld geldt alleen voor verkeer bij het subdomein __www__ . Aangezien u kunt geen jokertekens met CNAME-records gebruiken, moet u één CNAME voor elk domein/subdomein maken. Als u wilt verkeer van subdomeinen, zoals *. contoso.com, naar uw azurewebsite.net-mailadres, kunt u de invoer in een __URL omleiden__ of __Doorsturen URL__ in uw DNS-instellingen configureren of een A-record maken.

> [AZURE.NOTE] Het kan even duren voordat uw CNAME-record met doorvoeren via het DNS-systeem. U kunt de CNAME voor de website niet instellen, totdat de CNAME heeft doorgegeven. U kunt een service zoals <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> gebruiken om te bevestigen dat de CNAME beschikbaar is.

###<a name="add-the-domain-name-to-your-website"></a>Voeg de domeinnaam toe aan uw website

Nadat de CNAME-record voor domeinnaam heeft doorgegeven, moet u deze koppelen aan uw website. U kunt de aangepaste domeinnaam die is gedefinieerd door de CNAME-record aan uw website door een van beide de Azure opdrachtregel (Azure CLI) of door de beheerportal Azure toevoegen.

**De domeinnaam van een met de opdrachtregel hulpmiddelen toe te voegen**

Installeren en configureren van de [Interface van Azure-opdrachtregel](/manage/install-and-configure-cli/)en gebruik vervolgens de volgende opdracht uit:

    azure site domain add customdomain yoursitename

Bijvoorbeeld het volgende wordt een aangepaste domeinnaam toevoegen van **www.contoso.com** naar de website **contoso.azurewebsite.net** :

    azure site domain add www.contoso.com contoso

U kunt controleren of de naam van het aangepaste domein is toegevoegd aan de website met behulp van de volgende opdracht uit:

    azure site domain list yoursitename

De lijst die het resultaat van deze opdracht moet bevatten de aangepaste domeinnaam, evenals de standaard **. azurewebsite.net** invoer.

**Een domeinnaam via Azure Management Portal toevoegen**

1. Open in uw browser de [Beheerportal van Azure][portal].

2. Op het tabblad **Websites** , klik op de naam van uw site, selecteert u **Dashboard**, en selecteer vervolgens **Manage Domains** vanaf de onderkant van de pagina.

    ![][setcname2]

6. Typ in het tekstvak **DOMAIN NAMES** de domeinnaam die u hebt geconfigureerd.

    ![][setcname3]

6. Klik op het vinkje om de domeinnaam te accepteren.

Wanneer de configuratie is voltooid, kan de naam van het aangepaste domein, worden vermeld in de sectie **domain names** van de pagina **configureren** van uw website.

<a name="bkmk_configurearecord"></a><h2>Een A-Record voor uw aangepaste domein toevoegen</h2>

Als u wilt een A-record hebt gemaakt, moet u eerst het IP-adres van uw website vinden. Voeg een vermelding in de tabel DNS voor uw aangepaste domein toe door met de hulpmiddelen die is verstrekt door uw domeinregistrar. Elk van de domeinregistrar heeft een vergelijkbaar, maar iets anders methode voor het opgeven van een A-record, maar de concepten zijn hetzelfde. Naast een A-record maakt, moet u ook een CNAME-record die Azure wordt gebruikt om te controleren of de A-record maken.

1. Open in uw browser de [Beheerportal van Azure][portal].

2. Op het tabblad **Websites** , klikt u op de naam van uw site, **Dashboard**selecteren en selecteer vervolgens **Domeinen beheren** vanaf de onderkant van het scherm.

    ![][setcname2]

5. Zoek **Het IP-adres dat wordt gebruikt tijdens het configureren van een records**in het dialoogvenster **aangepaste domeinen beheren** . Kopieer het IP-adres. Hiermee wordt gebruikt bij het maken van de A-record.

5. Opmerking in het dialoogvenster **aangepaste domeinen beheren** de domeinnaam awverify aan het einde van de tekst aan de bovenkant van het dialoogvenster. Moet **awverify.mysite.azurewebsites.net** waar **Mijn site** is de naam van uw website. Kopieer dit, omdat deze de naam van het domein gebruikt bij het maken van de verificatie CNAME-record.

6. Meld u aan bij uw DNS--domeinregistrar website en Ga naar de pagina voor het beheer van DNS. Vindt u koppelingen of gebieden van de site die is gelabeld als **Domeinnaam**, **DNS**of **De naam van Server Management**.

6. Waar kunt u selecteren of voer A en CNAME-records zoeken. Mogelijk moet u het recordtype in een decoratieve omlaag selecteren of gaat u naar een pagina Geavanceerde instellingen.

7. Voer de volgende stappen uit om de A-record te maken:

    1. Selecteer of typ het domein of subdomein die worden gebruikt door de A-record. Selecteer bijvoorbeeld **www** als u wilt een alias maken voor **www.customdomain.com**. Als u opnemen in jokertekens voor alle subdomeinen wilt, voert u '__*__'. Hier wordt uitgelegd hoe alle subdomeinen, zoals **mail.customdomain.com**, **login.customdomain.com**en **www.customdomain.com**.

        Als u maken van een A-record voor het hoofddomein wilt, deze wordt mogelijk weergegeven als de '**@**' symbool in uw domeinregistrar de DNS-hulpprogramma's.

    2. Geef het IP-adres van uw cloudservice in het opgegeven veld. Hiermee wordt de vermelding van het domein gebruikt in de A-record met het IP-adres van uw implementatie cloud-service.

        Bijvoorbeeld de volgende handelingen uit die een record al het verkeer van **contoso.com** doorgestuurd naar de **137.135.70.239**, het IP-adres van onze gedistribueerde toepassing:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>Host naam/subdomein</strong></td>
        <td><strong>IP-adres</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        In dit voorbeeld wordt een A-record voor het hoofddomein maken. Als u opnemen in jokertekens alle subdomeinen bedekt wilt, voert u '__*__' als het subdomein.

7. Maak vervolgens een CNAME-record met een alias van **awverify**en een canonieke domein van **awverify.mysite.azurewebsites.net** die u eerder hebt gekregen.

    > [AZURE.NOTE] Terwijl een alias van awverify voor sommige Registrar werken mogelijk, kunnen anderen vragen om de naam van de volledige alias domein van awverify.www.customdomainname.com of awverify.customdomainname.com.

    Bijvoorbeeld maakt de volgende een CNAME-record die Azure gebruiken kunt om te controleren of de configuratie van de A-record.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Alias/Host naam/subdomein</strong></td>
    <td><strong>Canonieke domein</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.NET</td>
    </tr>
    </table>

> [AZURE.NOTE] Het kan even duren voordat de awverify CNAME-record met doorvoeren via het DNS-systeem. U kunt de aangepaste domeinnaam die is gedefinieerd door de A-record voor de website totdat de awverify CNAME heeft doorgegeven niet instellen. U kunt een service zoals <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> gebruiken om te bevestigen dat de CNAME beschikbaar is.

###<a name="add-the-domain-name-to-your-website"></a>Voeg de domeinnaam toe aan uw website

Nadat de **awverify** CNAME-record voor domeinnaam heeft doorgegeven, kunt u het aangepaste domein gedefinieerd door de A-record aan uw website kunt vervolgens koppelen. U kunt de aangepaste domeinnaam die is gedefinieerd door de A-record aan uw website met behulp van een van beide de Azure-CLI of met behulp van de Azure-beheerportal toevoegen.

**Een domeinnaam de opdrachtregel Azure (Azure CLI) toevoegen**

Installeren en configureren van de [Azure CLI](/manage/install-and-configure-cli/)en gebruik vervolgens de volgende opdracht uit:

    azure site domain add customdomain yoursitename

Bijvoorbeeld het volgende wordt een aangepaste domeinnaam toevoegen van **contoso.com** naar de website **contoso.azurewebsite.net** :

    azure site domain add contoso.com contoso

U kunt controleren of de naam van het aangepaste domein is toegevoegd aan de website met behulp van de volgende opdracht uit:

    azure site domain list yoursitename

De lijst die het resultaat van deze opdracht moet bevatten de aangepaste domeinnaam, evenals de standaard **. azurewebsite.net** invoer.

**Een domeinnaam via Azure Management Portal toevoegen**

1. Open in uw browser de [Beheerportal van Azure][portal].

2. Op het tabblad **Websites** , klik op de naam van uw site, selecteert u **Dashboard**, en selecteer vervolgens **Manage Domains** vanaf de onderkant van de pagina.

    ![][setcname2]

6. Typ in het tekstvak **DOMAIN NAMES** de domeinnaam die u hebt geconfigureerd.

    ![][setcname3]

6. Klik op het vinkje om de domeinnaam te accepteren.

Wanneer de configuratie is voltooid, kan de naam van het aangepaste domein, worden vermeld in de sectie **domain names** van de pagina **configureren** van uw website.

> [AZURE.NOTE] Nadat u de aangepaste domeinnaam die is gedefinieerd door de A-record aan uw website hebt toegevoegd, kunt u de CNAME-record van het awverify met de hulpmiddelen die is verstrekt door uw domeinregistrar kunt verwijderen. Echter als u wilt toevoegen van een andere A record in de toekomst, u moet de awverify opnieuw record voordat u de nieuwe domeinnaam kunt koppelen gedefinieerd door de nieuwe record met de website.

## <a name="next-steps"></a>Volgende stappen

-   [Het beheren van websites](/manage/services/web-sites/how-to-manage-websites/)

-   [Een SSL-certificaat configureren voor websites](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
