<properties
    pageTitle="Cloud Services Veelgestelde vragen over | Microsoft Azure"
    description="Veelgestelde vragen over Cloudservices."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Cloud Services Veelgestelde vragen
In dit artikel vindt u antwoorden op enkele veelgestelde vragen over Microsoft Azure-Cloudservices. U kunt ook de [Azure ondersteunen Veelgestelde vragen](http://go.microsoft.com/fwlink/?LinkID=185083) voor algemene Azure prijzen en ondersteuning voor informatie. U kunt ook de [Cloud Services VM grootte pagina](cloud-services-sizes-specs.md) raadplegen voor informatie over de grootte.

## <a name="certificates"></a>Certificaten

### <a name="where-should-i-install-my-certificate"></a>Waar moet ik mijn certificaat installeren?

- **Mijn**  
Certificaat van de toepassing met persoonlijke sleutel (\*.pfx, \*.p12).

- **CERTIFICERINGSINSTANTIE**  
Uw tussenliggende certificaten gaat u in dit archief (beleid en Sub CAs).

- **HOOFDMAP**  
De basiscertificeringsinstantie opslaan, zodat uw belangrijkste hoofdsite CA-certificaat moet gaat.

### <a name="i-cant-remove-expired-certificate"></a>Ik kan geen verlopen certificaat verwijderen

Azure voorkomt dat u een certificaat verwijderen terwijl deze gebruikt wordt. U moet de implementatie waarin het certificaat Verwijder of de implementatie bijwerken met een ander of vernieuwde certificaat.

### <a name="delete-an-expired-certificate"></a>Een verlopen certificaat verwijderen

Zo lang maken als het certificaat niet gebruikt wordt, kunt u de PowerShell-cmdlet [Verwijderen AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) een certificaat verwijderen.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Ik zijn certificaten benoemde beheer van Windows Azure-Service voor extensies verlopen

Deze certificaten worden gemaakt wanneer een uitbreiding wordt toegevoegd aan de cloudservice zoals de extensie extern bureaublad. Deze certificaten worden alleen gebruikt voor het coderen en decoderen de privé configuratie van de extensie. Het maakt niet uit als u deze certificaten verloopt. De vervaldatum is niet ingeschakeld.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Ik heb verwijderd certificaten terugkomen

Deze terugkomen waarschijnlijk vanwege een hulpmiddel die u, zoals Visual Studio gebruikt. Wanneer u opnieuw verbinding met een programma dat een certificaat gebruikt maakt, wordt deze opnieuw worden geüpload naar Azure.

### <a name="my-certificates-keep-disappearing"></a>Mijn certificaten te behouden verdwijnen

Wanneer het exemplaar VM wordt herhaald, gaan alle lokale wijzigingen verloren. Gebruik een [opstarten taak](cloud-services-startup-tasks.md) voor het installeren van certificaten op de virtuele machine telkens wanneer die de rol wordt gestart.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Ik kan mijn certificaten management niet vinden in de portal

[Beheer van certificaten](..\azure-api-management-certs.md) zijn alleen beschikbaar in de klassieke Azure-Portal. De huidige Azure portal gebruikt geen management certificaten. 

### <a name="how-can-i-disable-a-management-certificate"></a>Hoe kan ik een certificaat management uitschakelen?

[Beheer van certificaten](..\azure-api-management-certs.md) kan niet worden uitgeschakeld. U verwijdert u deze via het klassieke Azure-Portal als u niet wilt dat ze meer moet worden gebruikt.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Hoe maak ik een SSL-certificaat voor een specifiek IP-adres?

Volg de aanwijzingen in het [maken van een certificaat zelfstudie](cloud-services-certs-create.md). Het IP-adres gebruiken als de DNS-naam.

## <a name="security"></a>Beveiliging

### <a name="disable-ssl-30"></a>SSL 3.0 uitschakelen

Als u wilt uitschakelen 3.0 SSL en TLS-beveiliging, opstarten op een taak maken die wordt beschreven in dit blogbericht: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>De schaal van een cloudservice aanpassen

### <a name="i-cannot-scale-beyond-x-instances"></a>Ik kan geen schalen voorbij X exemplaren

Uw abonnement Azure geldt een limiet van het aantal cores die u kunt gebruiken. Schaalbaarheid werkt niet als u alle beschikbare cores hebt gebruikt. Bijvoorbeeld, als er een limiet van 100 cores, betekent dit dat u 100 A1 grootte VM exemplaren kan hebben voor uw cloudservice, of 50 A2 grootte VM exemplaren.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Ik kan geen een IP in een cloudservice multi-VIP reserveren

Controleer eerst of de VM-instantie waarmee u probeert te reserveren de IP voor is ingeschakeld. Controleer vervolgens of dat u gereserveerde IP-adressen voor bAndere de ontwikkel- en -implementaties gebruikt. De instellingen **niet** worden wijzigen tijdens het bijwerken van de implementatie.

