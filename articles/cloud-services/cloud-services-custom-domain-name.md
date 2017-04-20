<properties
    pageTitle="Een aangepaste domeinnaam configureren in de Cloud Services | Microsoft Azure"
    description="Leer hoe u uw Azure toepassing of de gegevens voor een aangepast domein worden door DNS-instellingen te configureren."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Een aangepaste domeinnaam voor een Azure cloudservice configureren

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-custom-domain-name-portal.md)
- [Azure klassieke portal](cloud-services-custom-domain-name.md)


Wanneer u een Cloudservice maakt, wordt het in Azure toegewezen aan een subdomein van cloudapp.net. Bijvoorbeeld: als uw Cloudservice heet "contoso", uw gebruikers kunnen voor toegang tot uw toepassing op een URL als http://contoso.cloudapp.net. Azure toegewezen ook een virtueel IP-adres.

U kunt echter ook uw toepassing worden op de naam van uw eigen domein, zoals contoso.com. In dit artikel wordt uitgelegd hoe reserveren of een aangepaste domeinnaam voor Cloudservice web rollen configureren.

Wilt u al undestand welke records CNAME en A zijn? [Sprong voorbij de verklaring](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Aan de slag het sneller! Gebruik de Azure [, interactieve rondleiding](http://support.microsoft.com/kb/2990804). Het maakt koppelen van een aangepaste domeinnaam en een mum van tijd voor communicatie (SSL) met Azure-Cloudservices of Azure Websites beveiligen.

<p/>

> [AZURE.NOTE]
> De procedures in deze taak toepassen op Azure-Cloudservices. Voor App-Services, raadpleegt u [deze](../app-service-web/web-sites-custom-domain-name.md). Zie voor opslag-accounts [deze](../storage/storage-custom-domain-name.md).


## <a name="understand-cname-and-a-records"></a>Meer informatie over records CNAME en A

CNAME (of aliasrecords) en een records kunnen u een domeinnaam koppelen aan een specifieke server (of service-in dit geval) echter ze werken anders. Er zijn ook enkele specifieke overwegingen wanneer u een records met Azure cloudservices waarmee u rekening houden moet voordat u besluit waarop u wilt gebruiken.

### <a name="cname-or-alias-record"></a>CNAME- of Alias record

Een *specifiek* domein, zoals **contoso.com** of **www.contoso.com**, koppelt een CNAME-record aan een canonieke domeinnaam. In dit geval de canonieke domeinnaam is de **[Mijntoep] .cloudapp .net** domeinnaam van uw Azure gehost toepassing. Zodra u hebt gemaakt, de CNAME Hiermee maakt u een alias voor de **[Mijntoep] .cloudapp .net**. De CNAME-vermelding worden opgelost aan het IP-adres van uw **[Mijntoep] .cloudapp .net** service-automatisch, zodat als het IP-adres van de cloudservice wijzigt, u hoeft geen actie te ondernemen.

> [AZURE.NOTE]
> Sommige domeinregistrars dat alleen u subdomeinen toewijzen bij gebruik van een CNAME-record, zoals www.contoso.com, en niet hoofdsite namen, zoals contoso.com. Zie voor meer informatie over de CNAME-records, de documentatie van uw domeinregistrar, [de Wikipedia-vermelding in de CNAME-record](http://en.wikipedia.org/wiki/CNAME_record)of het document [IETF Domain Names - implementatie en specificatie](http://tools.ietf.org/html/rfc1035) .

### <a name="a-record"></a>Een record

Een A-record een domein, zoals **contoso.com** of **www.contoso.com**, *of een jokertekendomein* , zoals kaarten ** \*. contoso.com**, een IP-adres. In het geval van een Azure-Cloudservice, het virtuele IP van de service. Zodat de belangrijkste voordelen van een A-record over een CNAME-record is dat u kunt één item met een jokerteken, zoals \* **. contoso.com**, waarin verzoeken om meerdere subdomeinen zoals **mail.contoso.com**, **login.contoso.com**of **www.contso.com**wilt verwerken.

> [AZURE.NOTE]
> Aangezien een A-record is toegewezen aan een statische IP-adres, kan deze wijzigingen automatisch naar het IP-adres van uw Cloudservice omzetten. Het IP-adres dat is gebruikt door uw Cloudservice is toegewezen aan de eerste keer dat u dashboard implementeren naar een lege slot (productie of tijdelijke.) Als u de implementatie voor de slot verwijdert, wordt het IP-adres door Azure wordt uitgebracht en eventuele toekomstige implementaties naar de slot krijgt een nieuwe IP-adres.
>
> Het IP-adres van een slot opgegeven implementatie (productie of tijdelijke) is gemakkelijk behouden bij het wisselen tussen tijdelijke en productie implementaties of het uitvoeren van een in-place upgrade van een bestaande implementatie. Zie voor meer informatie over het uitvoeren van deze acties, [het beheren van cloudservices](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Een CNAME-record voor uw aangepaste domein toevoegen

Als u wilt een CNAME-record hebt gemaakt, moet u een nieuwe vermelding in de tabel DNS voor uw aangepaste domein toevoegen met behulp van de hulpmiddelen die is verstrekt door uw domeinregistrar. Elk van de domeinregistrar heeft een vergelijkbaar, maar iets anders methode voor het opgeven van een CNAME-record, maar de concepten zijn hetzelfde.

1. Gebruik een van de volgende manieren om te zoeken naar de **. cloudapp.net** domeinnaam die zijn toegewezen aan uw cloudservice.

    * Meld u bij de [portal van Azure klassieke], selecteert u uw cloudservice **Dashboard**selecteren en zoek het **Site-URL** -fragment in het gedeelte **snel aan te duiden** .
    
        ![snel aan te duiden sectie met de URL voor de site][csurl]
    
        **OF-BEWERKING**  
    
    * Installeren en configureren van [Azure Powershell](../powershell-install-configure.md)en gebruik vervolgens de volgende opdracht uit:
        
        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Sla de domeinnaam die wordt gebruikt in de URL die het resultaat van een methode, moet u dit bij het maken van een CNAME-record.

1.  Meld u aan bij uw DNS--domeinregistrar website en Ga naar de pagina voor het beheer van DNS. Vindt u koppelingen of gebieden van de site die is gelabeld als **Domeinnaam**, **DNS**of **De naam van Server Management**.

2.  Nu kunt u selecteren of voert u de CNAME vinden. Mogelijk moet u het recordtype in een decoratieve omlaag selecteren of gaat u naar een pagina Geavanceerde instellingen. U moet controleren of de woorden **CNAME**, **Alias**of **subdomeinen**.

3.  U moet ook de alias domein of subdomein opgeven voor het CNAME, zoals **www** , als u wilt een alias maken voor **www.customdomain.com**. Als u een alias maken voor het hoofddomein wilt, deze wordt mogelijk weergegeven als de '**@**' symbool in uw domeinregistrar de DNS-hulpprogramma's.

4. Vervolgens moet u een canonieke de hostnaam van uw toepassing **cloudapp.net** domein in dit geval opgeven.

Bijvoorbeeld doorstuurt de volgende CNAME-record al het verkeer van **www.contoso.com** naar **contoso.cloudapp.net**, de naam van het aangepaste domein van uw gedistribueerde toepassing:

| Alias/Host naam/subdomein | Canonieke domein     |
| ------------------------- | -------------------- |
| www.                       | Contoso.cloudapp.NET |

Een bezoeker van **www.contoso.com** ziet nooit de waar host (contoso.cloudapp.net), zodat het proces doorschakelen zichtbaar voor de eindgebruiker is.

> [AZURE.NOTE]
> Het bovenstaande voorbeeld geldt alleen voor verkeer bij het subdomein **www** . Aangezien u kunt geen jokertekens met CNAME-records gebruiken, moet u één CNAME voor elk domein/subdomein maken. Als u wilt verkeer van subdomeinen, zoals \*. contoso.com, naar uw cloudapp.net-mailadres, kunt u de invoer in een **URL omleiden** of **Doorsturen URL** in uw DNS-instellingen configureren of een A-record maken.


## <a name="add-an-a-record-for-your-custom-domain"></a>Een A-record voor uw aangepaste domein toevoegen

Als u wilt een A-record hebt gemaakt, moet u eerst het virtuele IP-adres van uw cloudservice vinden. Voeg een nieuwe vermelding in de tabel DNS voor uw aangepaste domein toe door met de hulpmiddelen die is verstrekt door uw domeinregistrar. Elk van de domeinregistrar heeft een vergelijkbaar, maar iets anders methode voor het opgeven van een A-record, maar de concepten zijn hetzelfde.

1. Gebruik een van de volgende methoden om het IP-adres van uw cloudservice.
    
    * Meld u bij de [portal van Azure klassieke], selecteert u uw cloudservice **Dashboard**selecteren en zoek het **adres van openbare virtuele IP-(VIP)** -fragment in het gedeelte **snel aan te duiden** .
    
        ![snel aan te duiden sectie met de VIP][vip]
    
        **OF-BEWERKING**  
    
    * Installeren en configureren van [Azure Powershell](../powershell-install-configure.md)en gebruik vervolgens de volgende opdracht uit:
    
        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Als er meerdere eindpunten die is gekoppeld aan uw cloudservice, ontvangt u meerdere regels met het IP-adres, maar alle hetzelfde adres moet worden weergegeven.
    
    Sla het IP-adres, zoals u dit moet bij het maken van een A-record.

1.  Meld u aan bij uw DNS--domeinregistrar website en Ga naar de pagina voor het beheer van DNS. Vindt u koppelingen of gebieden van de site die is gelabeld als **Domeinnaam**, **DNS**of **De naam van Server Management**.

2.  Nu kunt u selecteren of invoeren van een record zoeken. Mogelijk moet u het recordtype in een decoratieve omlaag selecteren of gaat u naar een pagina Geavanceerde instellingen.

3. Selecteer of typ het domein of subdomein die worden gebruikt door deze A-record. Selecteer bijvoorbeeld **www** als u wilt een alias maken voor **www.customdomain.com**. Als u opnemen in jokertekens voor alle subdomeinen wilt, voert u '__*__'. Hier wordt uitgelegd hoe alle subdomeinen, zoals **mail.customdomain.com**, **login.customdomain.com**en **www.customdomain.com**.

    Als u maken van een A-record voor het hoofddomein wilt, deze wordt mogelijk weergegeven als de '**@**' symbool in uw domeinregistrar de DNS-hulpprogramma's.

4. Geef het IP-adres van uw cloudservice in het opgegeven veld. Hiermee wordt de vermelding van het domein gebruikt in de A-record met het IP-adres van uw implementatie cloud-service.

Bijvoorbeeld de volgende handelingen uit die een record al het verkeer van **contoso.com** doorgestuurd naar de **137.135.70.239**, het IP-adres van uw gedistribueerde toepassing:

| Host naam/subdomein | IP-adres     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |



In dit voorbeeld wordt een A-record voor het hoofddomein maken. Als u opnemen in jokertekens alle subdomeinen bedekt wilt, voert u '__*__' als het subdomein.

>[AZURE.WARNING]
>IP-adressen in Azure zijn standaard dynamisch. Wordt wilt u waarschijnlijk een [gereserveerde IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md) gebruiken om ervoor te zorgen dat uw IP-adres niet wordt gewijzigd.

## <a name="next-steps"></a>Volgende stappen

* [Het beheren van Cloudservices](cloud-services-how-to-manage.md)
* [CDN inhoud toewijzen aan een aangepast domein](../cdn/cdn-map-content-to-custom-domain.md)
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure.md).
* Leer hoe u [een cloudservice](cloud-services-how-to-create-deploy.md).
* [Ssl-certificaten](cloud-services-configure-ssl-certificate.md)configureren.




[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure klassieke portal]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
 