<properties 
    pageTitle="Cloud Services en management certificaten | Microsoft Azure" 
    description="Meer informatie over het maken en gebruiken van certificaten met Microsoft Azure" 
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Certificaten voor Azure Cloud Services-overzicht
Certificaten worden gebruikt in Azure wordt aangegeven voor cloudservices ([service certificaten](#what-are-service-certificates)) en voor de verificatie van verbindingen met het beheer API ([management certificaten](#what-are-management-certificates) bij gebruik van de Azure klassieke portal en niet op ARM). In dit onderwerp biedt een algemeen overzicht van beide certificaattypen [maken](#create) en [implementeren](#deploy) naar Azure ze.

Certificaten die worden gebruikt in Azure wordt aangegeven zijn x.509 v3-certificaten en kunnen worden ondertekend door een andere vertrouwd certificaat of dit steekt nogal zelfondertekend. Een zelfondertekend certificaat is ondertekend door een eigen creator en reden, niet worden vertrouwd al dan niet standaard. De meeste browsers kunnen dit negeren. Zelfondertekende certificaten mag alleen worden gebruikt door uzelf wanneer ontwikkelen en testen van uw cloudservices. 

Certificaten die worden gebruikt door Azure kunnen bevatten een persoonlijk of een openbare sleutel. Certificaten hebben een vingerafdruk die kan worden in een unieke manier kunt identificeren. Deze vingerafdruk wordt gebruikt in het Azure [configuratiebestand](cloud-services-configure-ssl-certificate.md) om aan te geven welke certificaat een cloudservice moet gebruiken. 

## <a name="what-are-service-certificates"></a>Wat zijn de certificaten service?
Service certificaten zijn gekoppeld cloud services en beveiligde communicatie van en naar de service te schakelen. Bijvoorbeeld als u een Webrol geïmplementeerd, zou u willen leveren een certificaat dat een weergegeven HTTPS-eindpunt kan verifiëren. Certificaten van de service, gedefinieerd in de servicedefinitie van uw, worden automatisch geïmplementeerd in de virtuele machine die een exemplaar van uw rol wordt uitgevoerd. 

U kunt service certificaten uploaden naar Azure klassieke portal met behulp van de Azure klassieke portal of met behulp van de API voor het beheer van Service. Service certificaten zijn die is gekoppeld aan een specifieke cloudservice en die zijn toegewezen aan een implementatie in het definitiebestand service.

Service certificaten afzonderlijk kunnen worden beheerd met uw services en kunnen worden beheerd door verschillende personen. Een ontwikkelaar kan bijvoorbeeld een servicepakket die verwijst naar een certificaat dat een IT-beheerder heeft eerder hebt geüpload naar Azure uploaden. Een IT-beheerder kan beheren en dat de configuratie van de service wijzigen zonder dat u nodig hebt voor het uploaden van een nieuwe servicepakket certificaat verlengen. Dit is mogelijk omdat de logische naam op voor het certificaat en de naam van de store en de locatie zijn opgegeven in het definitiebestand service terwijl vingerafdruk van het certificaat is opgegeven in het configuratiebestand service. Als u wilt bijwerken het certificaat, is het alleen moet u een nieuw certificaat uploaden en wijzig de vingerafdrukwaarde in het configuratiebestand service.

## <a name="what-are-management-certificates"></a>Wat zijn de certificaten management?
Beheer van certificaten kunnen u met de API voor het beheer van Service verstrekt door Azure klassieke wordt geverifieerd. Veel programma's en hulpprogramma's (zoals Visual Studio of de SDK Azure) Gebruik deze certificaten automatiseren configuratie en implementatie van verschillende Azure services. Deze niet echt betreffen cloud services. 

>[AZURE.WARNING] Wees voorzichtig! Dit soort van certificaten dat alle gebruikers met hen verifieert voor het beheren van het abonnement dat ze zijn gekoppeld. 

### <a name="limitations"></a>Beperkingen
Er is een limiet van 100 management certificaten per abonnement. Er is ook een limiet van 100 management certificaten voor alle abonnementen onder een specifieke servicebeheerder gebruikers-ID. Als de gebruikers-ID voor de accountbeheerder van het al gebruikt is om toe te voegen 100 management certificaten en een nodig is voor meer certificaten, kunt u de beheerder van een collega om toe te voegen van de aanvullende certificaten kunt toevoegen. 

Voordat u meer dan 100 certificaten toevoegt, Zie als u een bestaand certificaat opnieuw kunt gebruiken. CO-beheerders gebruiken, wordt mogelijk overbodige complexiteit toegevoegd aan uw certificaat management proces.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Een nieuwe zelfondertekend certificaat maken
U kunt een hulpprogramma beschikbaar voor het maken van een zelfondertekend certificaat zo lang maken als ze aan deze instellingen voldoen gebruiken:

* Een x.509-certificaat.
* Een persoonlijke sleutel bevat.
* Gemaakt voor belangrijke exchange (.pfx-bestand).
* Onderwerpnaam moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de cloudservice. 
    > U kunt geen in het bezit van een SSL-certificaat voor de cloudapp.net (of voor elke Azure gerelateerd)-domein. naam van de onderwerp van het certificaat moet overeenkomen met de naam van het aangepaste domein gebruikt voor toegang tot uw toepassing. Bijvoorbeeld **contoso.net**, niet **contoso.cloudapp.net**.
* Minimum van 2048-bit versleuteling gebruikt.
* **Alleen service-certificaat**: aan de clientzijde certificaat moet zich bevinden in het *persoonlijke* archief.

Er zijn twee eenvoudige manieren om te maken van een certificaat op Windows, met de `makecert.exe` hulpprogramma of IIS.

### <a name="makecertexe"></a>MakeCert.exe

Dit hulpprogramma is afgeschaft en niet meer Hier wordt beschreven. Zie [dit artikel MSDN](https://msdn.microsoft.com/library/windows/desktop/aa386968) voor meer informatie.

### <a name="powershell"></a>PowerShell

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Als u het certificaat gebruiken met een IP-adres in plaats van een domein wilt, gebruikt u het IP-adres in de parameter - DNS-naam.


Als u dit [certificaat met de beheerportal](../azure-api-management-certs.md)gebruiken wilt, kunt u deze naar een **.cer** -bestand exporteren:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet informatieservices (IIS)

Zijn er veel pagina's op internet die betrekking op hoe u dit met IIS. [Hier](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) is een uitstekende ik gevonden dat ik denk dat wordt in deze ook uitgelegd. 

### <a name="java"></a>Java
Java kunt u [een digitaal certificaat maken](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[In dit](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) artikel wordt beschreven hoe certificaten maken met SSH.

## <a name="next-steps"></a>Volgende stappen

[Uw servicecertificaat bij de portal voor Azure klassieke uploaden](cloud-services-configure-ssl-certificate.md) (of de [portal van Azure](cloud-services-configure-ssl-certificate-portal.md)).

Een [certificaat voor management-API](../azure-api-management-certs.md) uploaden naar de portal van Azure klassieke.

>[AZURE.NOTE] De portal van Azure gebruikt geen management certificaten voor toegang tot de API, maar in plaats daarvan gebruikt gebruikersaccounts.
