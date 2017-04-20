<properties
    pageTitle="Een domeinnaam configureren voor uw Blob storage eindpunt | Microsoft Azure"
    description="Informatie over het toewijzen van een aangepaste domein naar het eindpunt Blob storage voor een account Azure opslag in de klassieke Azure-Portal."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Een aangepaste domeinnaam voor uw Blob storage eindpunt configureren

## <a name="overview"></a>Overzicht

U kunt een aangepast domein voor toegang tot blob-gegevens in uw account Azure opslag configureren. De standaardeindpunt voor Blob storage is `<storage-account-name>.blob.core.windows.net`. Als u een aangepast domein en een subdomein zoals **www.contoso.com** naar het eindpunt blob voor uw account voor de opslag en vervolgens uw gebruikers kunnen ook toegang blobgegevens toewijzen in uw opslag-account dat domein gebruiken.

>[AZURE.IMPORTANT] Azure opslag ondersteunt niet nog HTTPS met aangepaste domeinen. We zijn Houd er rekening mee dat klanten geïnteresseerd in deze functie bent en deze beschikbaar in toekomstige versie is.

Er zijn twee manieren uw aangepaste domein verwijzen naar het eindpunt blob voor uw account opslag. De eenvoudigste manier is het opzetten van een toewijzing van uw aangepaste domein en een subdomein naar het eindpunt blob CNAME-record. Een CNAME-record is een DNS-functie die een brondomein naar een doeldomein verwijst. In dit geval het brondomein is voor uw aangepaste domein en subdomein--Houd er rekening mee dat het subdomein altijd is vereist. Het doeldomein is uw Blob service-eindpunt.

Het proces voor het toewijzen van uw aangepaste domein aan uw eindpunt blob kan, echter resulteren in een korte termijn van downtime voor het domein terwijl u het domein in de [Klassieke Azure-Portal registreert](https://manage.windowsazure.com). Als uw aangepaste domein is een toepassing met een service level (SLA agreement) die is vereist dat er geen downtime worden momenteel ondersteund, kunt klikt u vervolgens u het subdomein Azure **asverify** te geven van een tussenliggende registratiestap, zodat gebruikers kunnen voor toegang tot uw domein bij de DNS-records toewijzing plaatsvindt.

De volgende tabel ziet u voorbeeld-URL's voor toegang tot blob-gegevens in een benoemde **mystorageaccount**opslag-account. Het aangepaste domein is geregistreerd voor het account opslag is **www.contoso.com**:

Resourcetype|URL-indelingen
---|---
Opslag-account|**Standaard URL:** http://mystorageaccount.blob.core.windows.net<p>**Aangepaste domein URL:** http://www.contoso.com</td>
BLOB|**Standaard URL:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**Aangepaste domein URL:** http://www.contoso.com/mycontainer/myblob
Container hoofdsite|**Standaard-URL:** http://mystorageaccount.blob.core.windows.net/myblob of http://mystorageaccount.blob.core.windows.net/$ hoofdmap/myblob<p>**Aangepaste domein URL:** http://www.contoso.com/myblob of http://www.contoso.com/$ hoofdmap/myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Een aangepast domein voor uw account opslag registreren

Gebruik deze procedure voor het registreren van uw aangepaste domein, als u nog geen problemen met het domein kort niet beschikbaar voor gebruikers met, of als uw aangepaste domein is momenteel niet host voor een toepassing.

Als uw aangepaste domein momenteel een toepassing die geen elke uitvaltijd ondersteunt, gebruikt u de procedure in <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">een aangepast domein voor uw opslag-account met behulp van het subdomein tussenliggende asverify registreren</a>.

Als u wilt configureren van een aangepaste domeinnaam, moet u een nieuwe CNAME-record maken bij uw domeinregistrar. Een alias voor een domeinnaam; Hiermee geeft u de CNAME-record in dit geval wordt het adres van uw aangepaste domein naar het eindpunt Blob opslagruimte voor uw account opslagruimte toegewezen.

Elk van de domeinregistrar heeft een vergelijkbaar, maar iets anders methode voor het opgeven van een CNAME-record, maar het concept is dezelfde. Houd er rekening mee dat veel eenvoudige domein registratie-pakketten geen DNS-configuratie, bieden zodat u mogelijk moet u uw domein registratie-pakket bijwerken voordat u de CNAME-record kunt maken.

1.  Ga naar het tabblad **opslag** in de [Klassieke Azure-Portal](https://manage.windowsazure.com).

2.  Klik op het tabblad **opslagruimte** op de naam van de opslag-account waarvoor u wilt toewijzen van het aangepaste domein.

3.  Klik op het tabblad **configureren** .

4.  Klik op **Manage Domain** om weer te geven van het dialoogvenster **Aangepaste domein beheren** onder aan het scherm. In de tekst aan de bovenkant van het dialoogvenster ziet u informatie over het maken van de CNAME-record. Voor deze procedure kunt u de tekst die naar het subdomein **asverify verwijst** negeren.

5.  Meld u aan bij uw DNS--domeinregistrar website en Ga naar de pagina voor het beheer van DNS. U kunt dit mogelijk vinden in een sectie zoals **Domeinnaam**, **DNS**of **Naam Serverbeheer**.

6.  Zoek de sectie voor het beheer van de CNAME-records. Mogelijk moet u gaat u naar een pagina Geavanceerde instellingen en zoek naar de woorden **CNAME**, **Alias**of **subdomeinen**.

7.  Maak een nieuwe CNAME-record en geef een subdomein alias, zoals **www** of **foto's**. Geef een hostnaam, dat wil de Blob service-eindpunt, in de indeling **mystorageaccount.blob.core.windows.net zeggen** ( **mystorageaccount** is waar de naam van uw account opslag). De hostnaam gebruik is opgegeven voor u in de tekst van het dialoogvenster **Aangepaste domein beheren** .

8.  Nadat u de CNAME-record hebt gemaakt, Ga terug naar het dialoogvenster **Aangepaste domein beheren** en voer de naam van uw aangepaste domein, inclusief het subdomein, in het veld **Aangepaste domeinnaam** . Als uw domein **contoso.com is** en een subdomein **www is**, Voer bijvoorbeeld **www.contoso.com**; Als een subdomein **foto's is**, voert u **photos.contoso.com**. Houd er rekening mee dat het subdomein vereist is.

9. Klik op de knop **registreren** om uw aangepaste domein hebt geregistreerd.

    Als de registratie geslaagd is, ziet u het bericht dat **uw aangepaste domein actief is**. Gebruikers kunnen nu weergave blob-gegevens in uw aangepaste domein, zo lang ze de juiste machtigingen hebben.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Een aangepast domein voor uw opslag-account met behulp van het subdomein tussenliggende asverify registreren

Gebruik deze procedure voor het registreren van uw aangepaste domein als uw aangepaste domein momenteel ondersteunt een toepassing met een SLA die is vereist dat er geen downtime. Door een CNAME die verwijst maken op basis van asverify. &lt;subdomein&gt;. &lt;customdomain&gt; naar asverify. &lt;storageaccount&gt;. blob.core.windows.net, kunt u uw domein met Azure vooraf registreren. Vervolgens kunt u een tweede CNAME wijst van maken &lt;subdomein&gt;. &lt;customdomain&gt; naar &lt;storageaccount&gt;. blob.core.windows.net, waarna verkeer naar uw aangepaste domein doorgestuurd naar uw blob-eindpunt.

Het subdomein asverify is een speciale subdomein herkend door Azure. Door prepending **asverify** naar uw eigen subdomein, moet u Azure te herkennen van uw aangepaste domein zonder te wijzigen van de DNS-record voor het domein toestaan. Nadat u wijzigingen in de DNS-record voor het domein aanbrengt, wordt dit naar het eindpunt blob met geen downtime worden toegewezen.

1.  Ga naar het tabblad **opslag** in de [Klassieke Azure-Portal](https://manage.windowsazure.com).

2.  Klik op het tabblad **opslagruimte** op de naam van de opslag-account waarvoor u wilt toewijzen van het aangepaste domein.

3.  Klik op het tabblad **configureren** .

4.  Klik op **Manage Domain** om weer te geven van het dialoogvenster **Aangepaste domein beheren** onder aan het scherm. In de tekst aan de bovenkant van het dialoogvenster ziet u informatie over het maken van de CNAME-record met behulp van het subdomein **asverify** .

5.  Meld u aan bij uw DNS--domeinregistrar website en Ga naar de pagina voor het beheer van DNS. U kunt dit mogelijk vinden in een sectie zoals **Domeinnaam**, **DNS**of **De naam van Server Management**.

6.  Zoek de sectie voor het beheer van de CNAME-records. Mogelijk moet u gaat u naar een pagina Geavanceerde instellingen en zoek naar de woorden **CNAME**, **Alias**of **subdomeinen**.

7.  Maak een nieuwe CNAME-record en geef de alias van een subdomein dat het subdomein asverify bevat. Bijvoorbeeld het subdomein dat u opgeeft, worden in de indeling **asverify.www** of **asverify.photos**. Geef een hostnaam, dat wil de Blob service-eindpunt, in de indeling **asverify.mystorageaccount.blob.core.windows.net zeggen** ( **mystorageaccount** is waar de naam van uw account opslag). De hostnaam gebruik is opgegeven voor u in de tekst van het dialoogvenster **Aangepaste domein beheren** .

8.  Nadat u de CNAME-record hebt gemaakt, Ga terug naar het dialoogvenster **Aangepaste domein beheren** en voer de naam van uw aangepaste domein in het veld **Aangepaste domeinnaam** . Als uw domein **contoso.com is** en een subdomein **www is**, Voer bijvoorbeeld **www.contoso.com**; Als een subdomein **foto's is**, voert u **photos.contoso.com**. Houd er rekening mee dat het subdomein vereist is.

9.  Klik op het selectievakje met de mededeling dat **Geavanceerd: gebruik het subdomein 'asverify' naar Mijn aangepaste domein preregister**.

10. Klik op de knop **registreren** preregister van uw aangepaste domein.

    Als de voorafgaande rapport geslaagd is, ziet u het bericht dat **uw aangepaste domein actief is**.

11. Nu uw aangepaste domein is geverifieerd door Azure, maar het verkeer naar uw domein wordt niet nog gerouteerd naar uw account opslag voor. Ga terug naar de website van uw DNS-domeinregistrar om het te voltooien, en maak nog een CNAME-record die een subdomein wordt toegewezen aan uw Blob service-eindpunt. Geef het subdomein bijvoorbeeld als **www** of **foto's**en de hostnaam als **mystorageaccount.blob.core.windows.net** ( **mystorageaccount** is waar de naam van uw account opslag). Met deze stap, zijn de registratie van uw aangepaste domein is voltooid.

12. Tot slot kunt u de CNAME-record die u hebt gemaakt met **asverify**, verwijderen, zoals deze nodig alleen als een tussenliggende stap was.

Gebruikers kunnen nu weergave blob-gegevens in uw aangepaste domein, zo lang ze de juiste machtigingen hebben.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Controleer of het aangepaste domein verwijst naar uw Blob service-eindpunt

Om te bevestigen dat uw aangepaste domein daadwerkelijk is toegewezen aan uw Blob service-eindpunt, een blob in een openbare container in uw account opslag te maken. Klik in een webbrowser gebruiken een URI in de volgende indeling voor toegang tot de label:

-   http://<*subdomain.customdomain*>/<*mycontainer*>/<*myblob*>

U kunt bijvoorbeeld de volgende URI gebruiken voor toegang tot een webformulier via een aangepaste **photos.contoso.com** -subdomein dat is toegewezen aan een blob in de container **myforms** :

-   http://Photos.contoso.com/myforms/applicationform.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Een aangepast domein van uw account opslag unregister

Als u wilt de registratie van een aangepast domein, als volgt te werk: 

1. Meld u aan bij de [Portal van Azure klassieke](https://manage.windowsazure.com). 

2. Klik in het navigatiedeelvenster op **opslag**. 

3. Klik op de naam van de opslag-account om het dashboard weer te geven op de pagina **opslag** . 

5. Klik op het lint op **Manage Domain**. 

6. Klik op **registratie**in het dialoogvenster **Aangepaste domein beheren** . 


## <a name="additional-resources"></a>Aanvullende informatie

-   [Het aangepaste domein toewijzen aan inhoud bezorging netwerk (CDN) eindpunt](../cdn/cdn-map-content-to-custom-domain.md)
