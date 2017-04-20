<properties 
   pageTitle="Zelfondertekende certificaten maken voor punt-naar-Site virtueel netwerk cross-premises verbindingen met behulp van makecert | Microsoft Azure"
   description="In dit artikel bevat stappen voor het gebruik van makecert zelfondertekende certificaten maken op Windows 10."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Werken met zelfondertekende certificaten voor punt-naar-Site-verbindingen

In dit artikel vindt u een zelfondertekend certificaat met **makecert**maken, en klik vervolgens genereren clientcertificaten. De stappen zijn geschreven voor makecert op Windows 10. MakeCert is gevalideerd als u wilt maken van certificaten die compatibel met P2S verbindingen zijn. 

Voor P2S verbindingen, de voorkeursmethode voor certificaten is via uw bedrijfsoplossing certificaat, ervoor te zorgen dat de client om certificaten te verlenen met de indeling van de algemene waarde 'name@yourdomain.com', in plaats van de notatie 'Domeinnaam\gebruikersnaam NetBIOS'.

Als u een bedrijfsoplossing niet hebt, wordt een zelfondertekend certificaat moet worden P2S clients verbinding maken met een virtueel netwerk. We zijn op de hoogte dat makecert is afgeschaft, maar het is nog steeds een geldige methode voor het maken van zelfondertekende certificaten die compatibel met P2S verbindingen zijn. We werken aan een andere oplossing voor het zelfondertekende certificaten maken, maar op dit moment kan makecert de voorkeursmethode is.

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

MakeCert is een manier om een zelfondertekend certificaat maken. De volgende stappen begeleiden u bij het maken van een zelfondertekend certificaat makecert gebruiken. Deze stappen zijn niet specifiek implementatie-model. Ze zijn geldig voor zowel resourcemanager en klassiek.

### <a name="to-create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

1. Vanaf een computer met Windows 10, downloadt en installeert u de [Windows Software Development Kit (SDK) voor Windows 10](https://dev.windows.com/en-us/downloads/windows-10-sdk).

2. Na de installatie, kunt u het hulpprogramma makecert.exe onder dit pad vinden: C:\Program Files (x86) \Windows Kits\10\bin\<boog >. 
        
    Voorbeeld:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Maak vervolgens en een certificaat in het archief persoonlijk certificaat dat is op uw computer installeren. Het volgende voorbeeld wordt een bijbehorende *.cer* -bestand dat u naar Azure uploaden tijdens het configureren van P2S. De volgende opdracht als administrator uitvoeren. *ARMP2SRootCert* en *ARMP2SRootCert.cer* vervangen door de naam die u wilt gebruiken voor het certificaat.<br><br>Het certificaat, bevindt zich in uw certificaten - Huidige gebruiker\Persoonlijk\Certificaten.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="rootpublickey"></a>De openbare sleutel

Als onderdeel van de VPN Gateway-configuratie voor verbindingen punt-naar-Site, wordt de openbare sleutel voor het certificaat hoofdsite geüpload naar Azure.

1. Als u een .cer-bestand van het certificaat, open **certmgr.msc**. Met de rechtermuisknop op de hoofdmap zelfondertekend certificaat, klikt u op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. Klik in de Wizard op **volgende**, selecteert u **Nee, de persoonlijke sleutel niet exporteren**en klik vervolgens op **volgende**.

3. Klik op de pagina **Bestandsindeling voor Export** Selecteer **Base-64 encoded X.509 (. CER).** Klik vervolgens op **volgende**. 

4. Klik op het **bestand te exporteren**, **Blader** naar de locatie waarnaar u wilt exporteren van het certificaat. Geef het certificaatbestand voor de **bestandsnaam**. Klik vervolgens op **volgende**.

5. Klik op **Voltooien** als het certificaat wilt exporteren.

 
### <a name="export-the-self-signed-certificate-optional"></a>Exporteer het zelfondertekende certificaat (optioneel)

U kunt het zelfondertekend certificaat exporteren en veilig opslaan. Als nodig zijn, u kunt later installeren op een andere computer en meer clientcertificaten genereren of een ander .cer-bestand exporteren. Elke computer met een clientcertificaat geïnstalleerd en die is ook geconfigureerd met de juiste VPN-client instellingen met uw netwerk virtuele via P2S verbinden kunnen. Om die reden dan ook, hebt u gewenste om ervoor te zorgen dat clientcertificaten worden gegenereerd en alleen wanneer u nodig hebt geïnstalleerd en dat het zelfondertekend certificaat veilig is opgeslagen.

Als u wilt exporteren de zelfondertekend certificaat als een .pfx, selecteer het certificaat van de hoofdmap en gebruikt u dezelfde stappen zoals beschreven in het [exporteren van een clientcertificaat](#clientkey) als u wilt exporteren.

## <a name="create-and-install-client-certificates"></a>Maken en installeren van certificaten

U kunt het zelfondertekend certificaat niet installeren rechtstreeks op de clientcomputer. U moet een clientcertificaat vanuit het zelfondertekend certificaat te genereren. U en vervolgens exporteren en installeer het clientcertificaat op de clientcomputer. De volgende stappen zijn niet specifiek implementatie-model. Ze zijn geldig voor zowel resourcemanager en klassiek.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>Deel 1 – een clientcertificaat vanuit een zelfondertekend certificaat genereren

De volgende stappen begeleid u bij een manier om te genereren van een clientcertificaat van een zelfondertekend certificaat. U kunt meerdere clientcertificaten mogelijk genereren van hetzelfde certificaat. Elk clientcertificaat worden vervolgens geëxporteerd en geïnstalleerd op de clientcomputer. 

1. Open een opdrachtprompt als administrator op dezelfde computer waarmee u de zelfondertekend certificaat maken.

2. In dit voorbeeld wordt 'ARMP2SRootCert' verwijst naar het zelfondertekende certificaat dat u gegenereerd. 
    - *"ARMP2SRootCert"* op de naam van de zelfondertekend hoofdsite die u het clientcertificaat van genereert wijzigen. 
    - Wijzig *ClientCertificateName* in de naam die u een clientcertificaat wilt te genereren. 


    Wijzigen en uitvoeren van de steekproef om te genereren van een clientcertificaat. Als u het volgende voorbeeld uitvoeren zonder het te wijzigen, is het resultaat een clientcertificaat met de naam ClientCertificateName in uw winkel persoonlijk certificaat dat is gegenereerd vanuit de hoofdsite certificaat ARMP2SRootCert.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Alle certificaten zijn opgeslagen in uw 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten' hebt opgeslagen op uw computer. U kunt meerdere certificaten zo nodig op basis van deze procedure genereren.

### <a name="clientkey"></a>Deel 2: een certificaat exporteren

1. Als u wilt exporteren een clientcertificaat, open **certmgr.msc**. Met de rechtermuisknop op de clientcertificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. In de Wizard op **volgende**, en vervolgens Selecteer **Ja, de persoonlijke sleutel exporteren**en klik vervolgens op **volgende**.

3. Klik op de pagina **Bestandsindeling exporteren** laat u de standaardinstellingen die zijn geselecteerd. Klik vervolgens op **volgende**. 
 
4. Klik op de pagina **beveiliging** moet u de persoonlijke sleutel beveiligen. Als u een wachtwoord gebruikt, moet u opnemen of het wachtwoord dat u hebt ingesteld voor dit certificaat moet opslaan. Klik vervolgens op **volgende**.

5. Klik op het **bestand te exporteren**, **Blader** naar de locatie waarnaar u wilt exporteren van het certificaat. Geef het certificaatbestand voor de **bestandsnaam**. Klik vervolgens op **volgende**.

6. Klik op **Voltooien** als het certificaat wilt exporteren.  

### <a name="part-3---install-a-client-certificate"></a>Deel 3 – een clientcertificaat installeren

Elke client die u verbinden met uw virtuele netwerk wilt met behulp van een verbinding punt-naar-Site moet een clientcertificaat is geïnstalleerd. Dit certificaat is naast het vereiste VPN configuratie-pakket. De volgende stappen begeleid u bij het clientcertificaat handmatig installeren.

1. Zoek en de *.pfx* -bestand kopiëren naar de clientcomputer. Dubbelklik op de *.pfx* -bestand te installeren op de clientcomputer. Laten staan als de **Huidige gebruiker**de **Locatie van de Store** en klik op **volgende**.

2. Klik op het **bestand** te importeren pagina, breng de wijzigingen niet. Klik op **volgende**.

3. Klik op de pagina **beveiliging van persoonlijke sleutel** het wachtwoord voor het certificaat als invoer als u een, gebruikt of controleert u of de beveiligings-principal die het certificaat is installeert klopt en klik op **volgende**.

4. Klik op de pagina **Certificaat Store** laat de standaardlocatie en klik vervolgens op **volgende**.

5. Klik op **Voltooien**. Klik op de **Beveiligingswaarschuwing weergegeven** voor de installatie van het certificaat, klikt u op **Ja**. Het certificaat is nu geïmporteerd.

## <a name="next-steps"></a>Volgende stappen

Gaat u verder met de configuratie van het punt-naar-Site. 

- Zie voor **Resourcemanager** implementatie model stappen [configureren een punt-naar-Site-verbinding met een VNet via PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md). 
- Zie [een punt-naar-Site configureren VPN-verbinding met een VNet met behulp van de klassieke portal](vpn-gateway-point-to-site-create.md)voor stappen in de **klassieke** implementatie-model.