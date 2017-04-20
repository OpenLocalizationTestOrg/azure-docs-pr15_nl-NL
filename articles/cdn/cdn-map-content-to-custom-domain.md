<properties
     pageTitle="De inhoud van de Azure Contentlevering-netwerk (CDN) toewijzen aan een aangepast domein | Microsoft Azure"
     description="In dit onderwerp wordt getoond hoe u de inhoud van een CDN toewijzen aan een aangepast domein."
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
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Het aangepaste domein toewijzen aan inhoud bezorging netwerk (CDN) eindpunt
U kunt een aangepast domein toewijzen aan een CDN-eindpunt om te kunnen gebruiken van uw eigen domeinnaam in URL's in de cache opgeslagen inhoud in plaats van met behulp van een subdomein van azureedge.net.

Er zijn twee manieren uw aangepaste domein toewijzen aan een CDN-eindpunt:

1. [Een CNAME-record maken bij uw domeinregistrar en in kaart brengen uw aangepaste domein en een subdomein naar het eindpunt CDN](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    Een CNAME-record is een DNS-functie die een brondomein, zoals kaarten `www.contosocdn.com` of `cdn.contoso.com`, naar een doeldomein. In dit geval is het brondomein voor uw aangepaste domein en een subdomein (een subdomein, zoals **www** of **cdn** altijd vereist is). Het doeldomein is uw CDN-eindpunt.  

    Het proces voor het toewijzen van uw aangepaste domein aan uw CDN-eindpunt kan, echter resulteren in een korte termijn van downtime voor het domein terwijl u het domein in de Portal Azure registreert.

2. [Een tussenliggende registratiestap met **cdnverify** toevoegen](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Als uw aangepaste domein is een toepassing met een service level (SLA agreement) die is vereist dat er geen downtime worden momenteel ondersteund, kunt klikt u vervolgens u het subdomein Azure **cdnverify** te geven van een tussenliggende registratiestap, zodat gebruikers kunnen voor toegang tot uw domein bij de DNS-records toewijzing plaatsvindt.  

Nadat u uw aangepaste domein met een van de bovenstaande procedures geregistreerd, zult u om te [bevestigen dat de aangepaste subdomein verwijst naar uw CDN-eindpunt](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] U moet een CNAME-record maken bij uw domeinregistrar van uw domein naar het eindpunt CDN toewijzen. CNAME-records toewijzen specifieke subdomeinen, zoals `www.contoso.com` of `cdn.contoso.com`. Het is niet mogelijk een CNAME-record om aan te wijzen een hoofddomein zoals `contoso.com`.
>    
> Een subdomein kan alleen worden gekoppeld aan één CDN-eindpunt. De CNAME-record die u maakt, worden al het verkeer geadresseerd aan het subdomein naar het opgegeven eindpunt routeert.  Als u koppelen bijvoorbeeld `www.contoso.com` met uw eindpunt CDN vervolgens u kunt geen te koppelen aan andere Azure eindpunten, zoals een eindpunt opslag-account of een cloud-service-eindpunt. U kunt echter verschillende subdomeinen uit hetzelfde domein gebruiken voor verschillende service-eindpunten. U kunt ook verschillende subdomeinen toewijzen aan hetzelfde CDN eindpunt.
>
> Voor de eindpunten van **Azure CDN uit Verizon** (standaard en Premium), houd er rekening mee dat deze in beslag op **90 minuten** voor een aangepast domein wijzigingen doorgeven aan CDN rand knooppunten.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Een aangepast domein voor een eindpunt Azure CDN registreren

1.  Meld u aan bij de [Portal van Azure](https://portal.azure.com/).
2.  Klik op **Bladeren**, klikt u vervolgens **CDN-profielen**, klikt u vervolgens het CDN-profiel met het eindpunt dat u wilt toewijzen aan een aangepast domein.  
3.  Klik in het blad **CDN-profiel** op het CDN-eindpunt dat u wilt koppelen het subdomein.
4.  Klik op de knop **Aangepast domein toevoegen** aan de bovenkant van het blad eindpunt.  Klik in het blad **een aangepast domein hebt toegevoegd** ziet u de hostnaam eindpunt, afgeleid van uw eindpunt CDN, gebruiken om te maken van een nieuwe CNAME-record. De opmaak van het adres van de host-naam wordt weergegeven als ** &lt;EndpointName >. azureedge.net**.  U kunt deze hostnaam gebruiken om te maken van de CNAME-record kopiëren.  
5.  Navigeer naar de website van uw domeinregistrar en zoek naar de sectie voor het maken van DNS-records. U kunt dit mogelijk vinden in een sectie zoals **Domeinnaam**, **DNS**of **De naam van Server Management**.
6.  Zoek de sectie voor het beheer van de CNAME-records. Mogelijk moet u gaat u naar een pagina Geavanceerde instellingen en zoek naar de woorden CNAME Alias of subdomeinen.
7.  Maak een nieuwe CNAME-record waarmee een door u gekozen subdomein (bijvoorbeeld **www** of **cdn**) worden toegewezen aan de hostnaam die beschikbaar zijn in het blad **een aangepast domein hebt toegevoegd** .
8.  Ga terug naar het blad **een aangepast domein toevoegen** en voer uw aangepaste domein, zoals het subdomein, in het dialoogvenster. Voer bijvoorbeeld de domeinnaam in de indeling `www.contoso.com` of `cdn.contoso.com`.   

    Azure wordt Controleer of de CNAME-record voor de domeinnaam die u hebt ingevoerd. Als de CNAME juist is, wordt uw aangepaste domein gevalideerd.  Voor de eindpunten van **Azure CDN uit Verizon** (standaard en Premium) duurt maximaal 90 minuten voor aangepaste domeininstellingen doorgeven aan alle CDN rand knooppunten, echter.  

    Houd er rekening mee dat tijd voor de CNAME-record doorgeven aan naamservers op Internet in sommige gevallen kan duren. Als uw domein niet direct worden gevalideerd en u denkt dat de CNAME-record juist is, wacht enkele minuten en probeer het opnieuw.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Een aangepast domein voor een Azure CDN-eindpunt met behulp van het subdomein tussenliggende cdnverify registreren  

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com/).
2. Klik op **Bladeren**, klikt u vervolgens **CDN-profielen**, klikt u vervolgens het CDN-profiel met het eindpunt dat u wilt toewijzen aan een aangepast domein.  
3. Klik in het blad **CDN-profiel** op het CDN-eindpunt dat u wilt koppelen het subdomein.
4. Klik op de knop **Aangepast domein toevoegen** aan de bovenkant van het blad eindpunt.  Klik in het blad **een aangepast domein hebt toegevoegd** ziet u de hostnaam eindpunt, afgeleid van uw eindpunt CDN, gebruiken om te maken van een nieuwe CNAME-record. De opmaak van het adres van de host-naam wordt weergegeven als ** &lt;EndpointName >. azureedge.net**.  U kunt deze hostnaam gebruiken om te maken van de CNAME-record kopiëren.
5. Navigeer naar de website van uw domeinregistrar en zoek naar de sectie voor het maken van DNS-records. U kunt dit mogelijk vinden in een sectie zoals **Domeinnaam**, **DNS**of **De naam van Server Management**.
6. Zoek de sectie voor het beheer van de CNAME-records. Mogelijk moet u gaat u naar een pagina Geavanceerde instellingen en zoek naar de woorden **CNAME**, **Alias**of **subdomeinen**.
7. Maak een nieuwe CNAME-record en geef de alias van een subdomein dat het subdomein **cdnverify** bevat. Bijvoorbeeld het subdomein dat u opgeeft, worden in de indeling **cdnverify.www** of **cdnverify.cdn**. Geef de hostnaam, dat wil het eindpunt CDN, klikt u in de indeling **zeggen cdnverify.&lt; EndpointName >. azureedge.net**.
8. Ga terug naar het blad **een aangepast domein toevoegen** en voer uw aangepaste domein, zoals het subdomein, in het dialoogvenster. Voer bijvoorbeeld de domeinnaam in de indeling `www.contoso.com` of `cdn.contoso.com`. Houd er rekening mee dat in deze stap geeft u niet hoeft het subdomein met **cdnverify**begint.  

    Azure wordt Controleer of de CNAME-record voor de cdnverify domeinnaam die u hebt ingevoerd.
9. Nu uw aangepaste domein is geverifieerd door Azure, maar het verkeer naar uw domein wordt niet nog gerouteerd naar uw CDN-eindpunt. Nadat wachten lang genoeg toe te staan dat de instellingen van het aangepaste domein doorgeven aan de CDN rand knooppunten (90 minuten voor **Azure CDN uit Verizon**, 1-2 minuten voor **Azure CDN van Akamai**), Ga terug naar de website van uw DNS-domeinregistrar en maak nog een CNAME-record die wordt toegewezen aan een subdomein aan uw CDN-eindpunt. Het subdomein bijvoorbeeld opgeven als **www** of **cdn**en de hostnaam als ** &lt;EndpointName >. azureedge.net**. Met deze stap, zijn de registratie van uw aangepaste domein is voltooid.
10. Tot slot kunt u de CNAME-record die u hebt gemaakt met **cdnverify**, verwijderen, zoals deze nodig alleen als een tussenliggende stap was.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Controleer of het aangepaste subdomein verwijst naar uw CDN-eindpunt

- Nadat u de registratie van uw aangepaste domein hebt voltooid, kunt u toegang tot inhoud die is opgeslagen in de cache op uw CDN-eindpunt via het aangepaste domein.
Eerst zorgt ervoor dat u openbare inhoud die is opgeslagen in de cache op het eindpunt. Als uw CDN-eindpunt gekoppeld aan een account opslag is, slaat het CDN bijvoorbeeld inhoud in openbare blob containers. Als u wilt testen het aangepaste domein, zorg ervoor dat uw container is ingesteld op openbare toegang toestaan en ten minste één blob bevat.
- Ga in uw browser naar het adres van de blob via het aangepaste domein. Als uw aangepaste domein is bijvoorbeeld `cdn.contoso.com`, de URL voor een in de cache blob is vergelijkbaar met de volgende URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Zie ook

[Het inschakelen van het netwerk voor Contentlevering (CDN) voor Azure](./cdn-create-new-endpoint.md)  
