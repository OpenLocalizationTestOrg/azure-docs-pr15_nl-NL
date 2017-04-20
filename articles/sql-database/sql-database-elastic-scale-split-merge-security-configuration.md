<properties 
    pageTitle="Gesplitste samenvoegen beveiliging | Microsoft Azure" 
    description="Instellen van x409 certificaten voor versleuteling" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Beveiliging van de gesplitste samenvoegen  

Als u wilt de service gesplitste/samenvoegen gebruikt, moet u correct beveiliging configureren. De service maakt deel uit van de functie elastische schaal van Microsoft Azure SQL-Database. Zie [elastische schaal splitsen en samenvoegen Service zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md)voor meer informatie.

## <a name="configuring-certificates"></a>Certificaten configureren

Certificaten zijn op twee manieren geconfigureerd. 

1. [Naar de SSL-certificaat configureren](#To-Configure-the-SSL#Certificate)
2. [Clientcertificaten configureren](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Certificaten verkrijgen

Certificaten kunnen worden opgehaald uit openbare certificeringsinstanties (CAs) of uit de [Windows-certificaat-Service](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Dit zijn de voorkeur methoden om certificaten te verkrijgen.

Als deze opties niet beschikbaar zijn, kunt u **zelfondertekende certificaten**genereren.
 
## <a name="tools-to-generate-certificates"></a>Hulpmiddelen voor het genereren van certificaten

* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>De hulpmiddelen voor uitvoeren

* Vanuit een ontwikkelaar opdrachtprompt voor visuele Studios, raadpleegt u [Visual Studio-opdrachtprompt](http://msdn.microsoft.com/library/ms229859.aspx) 

    Als u hebt geïnstalleerd, gaat u naar:

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* De WDK uit krijgen [Windows 8.1: Download Kit en hulpprogramma's voor](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>Naar de SSL-certificaat configureren
Een SSL-certificaat is vereist voor het coderen van de communicatie en verifiëren van de server. Kies de meest van toepassing van de onderstaande drie scenario's en alle bijbehorende stappen uitvoeren:

### <a name="create-a-new-self-signed-certificate"></a>Een nieuwe zelfondertekend certificaat maken

1.    [Een zelfondertekend certificaat maken](#Create-a-Self-Signed-Certificate)
2.    [PFX-bestand voor zelfondertekende SSL-certificaat maken](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [SSL-certificaat Service naar cloud uploaden](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [SSL-certificaat Service configuratiebestand bijwerken](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [SSL-certificeringsinstantie importeren](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Gebruik van een bestaand certificaat uit het archief certificaten
1. [SSL-certificaat exporteren vanuit certificaat Store](#Export-SSL-Certificate-From-Certificate-Store)
2. [SSL-certificaat Service naar cloud uploaden](#Upload-SSL-Certificate-to-Cloud-Service)
3. [SSL-certificaat Service configuratiebestand bijwerken](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Gebruik van een bestaand certificaat in een PFX-bestand

1. [SSL-certificaat Service naar cloud uploaden](#Upload-SSL-Certificate-to-Cloud-Service)
2. [SSL-certificaat Service configuratiebestand bijwerken](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>Clientcertificaten configureren
Certificaten zijn vereist om te verifiëren aanvragen voor de service. Kies de meest van toepassing van de onderstaande drie scenario's en alle bijbehorende stappen uitvoeren:

### <a name="turn-off-client-certificates"></a>Certificaten uitschakelen
1.    [Client certificaten gebaseerde verificatie uitschakelen](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Nieuwe zelfondertekend client certificaten
1.    [Een zelfondertekend certificeringsinstantie maken](#Create-a-Self-Signed-Certification-Authority)
2.    [Certificeringsinstantie Service naar cloud uploaden](#Upload-CA-Certificate-to-Cloud-Service)
3.    [Update voor de certificeringsinstantie Service configuratiebestand](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Certificaten van de Client](#Issue-Client-Certificates)
5.    [PFX-bestanden maken voor certificaten](#Create-PFX-files-for-Client-Certificates)
6.    [Clientcertificaat importeren](#Import-Client-Certificate)
7.    [Client-certificaat vingerafdrukken kopiëren](#Copy-Client-Certificate-Thumbprints)
8.    [Toegestane Clients in het configuratiebestand Service configureren](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Bestaande clientcertificaten gebruiken
1.    [Openbare sleutel CA zoeken](#Find-CA-Public Key)
2.    [Certificeringsinstantie Service naar cloud uploaden](#Upload-CA-certificate-to-cloud-service)
3.    [Update voor de certificeringsinstantie Service configuratiebestand](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Client-certificaat vingerafdrukken kopiëren](#Copy-Client-Certificate-Thumbprints)
5.    [Toegestane Clients in het configuratiebestand Service configureren](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Controle op certificaat intrekking Client configureren](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Toegestane IP-adressen

Toegang tot de service-eindpunten worden beperkt tot specifieke bereiken van IP-adressen.

## <a name="to-configure-encryption-for-the-store"></a>Codering voor de winkel configureren

Een certificaat is vereist voor het coderen van de referenties die zijn opgeslagen in het metagegevensarchief. Kies de meest van toepassing van de onderstaande drie scenario's en alle bijbehorende stappen uitvoeren:

### <a name="use-a-new-self-signed-certificate"></a>Gebruik een nieuw zelfondertekend certificaat

1.     [Een zelfondertekend certificaat maken](#Create-a-Self-Signed-Certificate)
2.     [PFX-bestand maken voor zelfondertekende versleutelingscertificaat](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Versleutelingscertificaat Service naar cloud uploaden](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Versleutelingscertificaat Service configuratiebestand bijwerken](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Een bestaand certificaat uit het archief certificaten gebruiken

1.     [Versleutelingscertificaat van Store certificaat exporteren](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Versleutelingscertificaat Service naar cloud uploaden](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Versleutelingscertificaat Service configuratiebestand bijwerken](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Een bestaand certificaat gebruiken in een PFX-bestand

1.     [Versleutelingscertificaat Service naar cloud uploaden](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Versleutelingscertificaat Service configuratiebestand bijwerken](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>De standaardconfiguratie

De standaardconfiguratie al dan niet toestaan van alle toegang tot het HTTP-eindpunt. Dit is de aanbevolen instelling, omdat de aanvragen voor deze eindpunten gevoelige gegevens, zoals de databasereferenties kunnen uitvoeren.
De standaardconfiguratie voor alle toegang tot het HTTPS-eindpunt. Deze instelling is mogelijk verder beperkt.

### <a name="changing-the-configuration"></a>Wijzigen van de configuratie

De groep van regels voor toegang tot die van toepassing en eindpunt zijn geconfigureerd de **<EndpointAcls>** sectie in het **configuratiebestand service**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

De regels in de groep van een access-besturingselement zijn geconfigureerd een <AccessControl name=""> sectie van het configuratiebestand service. 

De opmaak wordt uitgelegd in de documentatie toegangsbeheerlijsten netwerk.
Bijvoorbeeld wilt dat alleen IP-adressen in het bereik 100.100.0.0 naar 100.100.255.255 voor toegang tot het eindpunt HTTPS, er de regels als volgt:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Weigering van servicepreventie

Er zijn twee verschillende regelingen ondersteund om te detecteren en weigering van Service-aanvallen te voorkomen:

*    Beperk het aantal gelijktijdige aanvragen per externe host (standaard uitgeschakeld)
*    Tarief van access per externe host beperken (Klik op standaard)

Deze zijn gebaseerd op de functies die verder beschreven in dynamische IP-beveiliging in IIS. Wanneer deze configuratie wijzigen Houd rekening met de volgende factoren:

* Het gedrag van proxy's en NAT-apparaten over de gegevens van de externe host
* Elk verzoek om alle bronnen in de Webrol wordt beschouwd als (bijvoorbeeld laden scripts, afbeeldingen, enzovoort)

## <a name="restricting-number-of-concurrent-accesses"></a>Aantal gelijktijdige toegang beperken

De instellingen die dit gedrag configureren zijn:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Wijzig DynamicIpRestrictionDenyByConcurrentRequests in true zodat dit beveiliging.

## <a name="restricting-rate-of-access"></a>Tarief van access beperken

De instellingen die dit gedrag configureren zijn:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Het antwoord op een verzoek voor een geweigerd configureren

De volgende instelling configureert het antwoord op een verzoek voor een geweigerd:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Raadpleeg de documentatie voor dynamische IP-beveiliging in IIS voor andere ondersteunde waarden.

## <a name="operations-for-configuring-service-certificates"></a>Bewerkingen voor het configureren van service certificaten
In dit onderwerp is bedoeld voor alleen verwijzing. Volg de configuratiestappen in:

* De SSL-certificaat configureren
* Clientcertificaten configureren

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken
Uitvoeren:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Om aan te passen:

*    -n met de service-URL. Jokertekens ("CN = * .cloudapp .net") en alternatieve namen ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") worden ondersteund.
*    e - met de vervaldatum van het certificaat een sterk wachtwoord maken en geef deze wanneer u wordt gevraagd.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>PFX-bestand voor zelfondertekend SSL-certificaat maken

Uitvoeren:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Wachtwoord Typ en exporteer het certificaat met deze opties:
* Ja, de persoonlijke sleutel exporteren
* Alle uitgebreide eigenschappen exporteren

## <a name="export-ssl-certificate-from-certificate-store"></a>SSL-certificaat exporteren vanuit certificaat store

* Certificaat zoeken
* Klik op Acties -> alle taken -> exporteren...
* Afbeelding invoegen van SharePoint naar een. PFX-bestand met deze opties:
    * Ja, de persoonlijke sleutel exporteren
    * Alle certificaten toevoegen aan het certificeringspad indien mogelijk * alle uitgebreide eigenschappen exporteren

## <a name="upload-ssl-certificate-to-cloud-service"></a>SSL-certificaat uploaden naar cloudservice

Upload-certificaat met de bestaande of gegenereerd. PFX-bestand met een combinatie van de SSL sleutel:

* Voer het wachtwoord voor het beveiligen van de gegevens voor persoonlijke sleutels

## <a name="update-ssl-certificate-in-service-configuration-file"></a>SSL-certificaat service configuratiebestand bijwerken

De vingerafdrukwaarde van de volgende instelling in het configuratiebestand service bijwerken met de vingerafdruk van het certificaat dat is geüpload naar de cloudservice:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>SSL-certificeringsinstantie importeren

Volg deze stappen in alle account/machine die moeten communiceren met de service:

* Dubbelklik op de. CER-bestand in Windows Verkenner
* Klik in het dialoogvenster certificaat op certificaat installeren...
* Certificaat importeren in de hoofdmap certificeringsinstanties vertrouwde archief

## <a name="turn-off-client-certificate-based-authentication"></a>Client certificaten gebaseerde verificatie uitschakelen

Alleen client certificaatverificatie wordt ondersteund en uitschakelt, kan voor openbare toegang tot de service-eindpunten, tenzij andere regelingen op hun plaats staan (bijvoorbeeld Microsoft Azure Virtual Network).

Wijzig de instellingen ONWAAR in het configuratiebestand service de functie om uit te schakelen:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Kopieer vervolgens de dezelfde vingerafdruk als het SSL-certificaat bij de instelling van de certificaat CA:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Een zelfondertekend certificeringsinstantie maken
De volgende stappen uit als u wilt maken van een zelfondertekend certificaat fungeert als een certificeringsinstantie uitvoeren:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Aanpassen

*    e - met de vervaldatum certificering


## <a name="find-ca-public-key"></a>Openbare sleutel CA zoeken

Alle clientcertificaten moeten zijn verleend door een certificeringsinstantie vertrouwde door de service. Zoek de openbare sleutel naar de certificeringsinstantie die certificaten de client die worden gebruikt voor verificatie om te uploaden naar de cloudservice.

Als het bestand met de openbare sleutel niet beschikbaar is, kunt u deze in de store certificaat exporteren:

* Certificaat zoeken
    * Zoeken naar een clientcertificaat dat is uitgegeven door dezelfde certificeringsinstantie
* Dubbelklik op het certificaat.
* Selecteer het tabblad-certificeringspad in het dialoogvenster certificaat.
* Dubbelklik op de vermelding CA in het pad.
* Notities van de certificaateigenschappen van het maken.
* Sluit het dialoogvenster **certificaat** .
* Certificaat zoeken
    * Zoek naar de Certificeringsinstantie hierboven.
* Klik op Acties -> alle taken -> exporteren...
* Afbeelding invoegen van SharePoint naar een. CER met deze opties:
    * **Nee, de persoonlijke sleutel niet exporteren**
    * Indien mogelijk alle certificaten toevoegen aan het pad certificeringsinstantie.
    * Alle uitgebreide eigenschappen exporteren.

## <a name="upload-ca-certificate-to-cloud-service"></a>CA-certificaat uploaden naar cloudservice

Upload-certificaat met de bestaande of gegenereerd. CER-bestand met de openbare sleutel CA.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Update voor de certificeringsinstantie service configuratiebestand

De vingerafdrukwaarde van de volgende instelling in het configuratiebestand service bijwerken met de vingerafdruk van het certificaat dat is geüpload naar de cloudservice:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

De waarde van de volgende instelling bijwerken met de dezelfde vingerafdruk:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Certificaten van de client

Iedereen die toegang hebben tot de service moet een clientcertificaat uitgegeven voor his/hers exclusief gebruiken en his/hers de eigenaar bent van sterk wachtwoord als u wilt beveiligen met bijbehorende persoonlijke sleutel moet kiezen. 

De volgende stappen moeten worden uitgevoerd in dezelfde computer waar het zelfondertekende certificaat van CA is gegenereerd en opgeslagen:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Aan te passen:

* -n met ID voor de client die met dit certificaat geverifieerd
* e - met de vervaldatum van het certificaat
* MyID.pvk en MyID.cer met unieke bestandsnamen voor deze clientcertificaat

Deze opdracht wordt gevraagd om een wachtwoord om te worden gemaakt en klik vervolgens één keer wordt gebruikt. Gebruik een sterk wachtwoord.

## <a name="create-pfx-files-for-client-certificates"></a>PFX-bestanden voor client certificaten maken

Voor elk gegenereerde clientcertificaat uitvoeren:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Aan te passen:

    MyID.pvk and MyID.cer with the filename for the client certificate

Wachtwoord Typ en exporteer het certificaat met deze opties:

* Ja, de persoonlijke sleutel exporteren
* Alle uitgebreide eigenschappen exporteren
* De persoon aan wie dit certificaat wordt uitgegeven moet het wachtwoord exporteren kiezen

## <a name="import-client-certificate"></a>Clientcertificaat importeren

Elke persoon voor wie een clientcertificaat is afgegeven moet de belangrijkste paar gegevenspunten in de machines die hij/zij wordt gebruikt om te communiceren met de service importeren:

* Dubbelklik op de. PFX-bestand in Windows Verkenner
* Certificaat importeren in de persoonlijke opgeslagen met ten minste deze optie:
    * Alle uitgebreide eigenschappen die zijn ingeschakeld

## <a name="copy-client-certificate-thumbprints"></a>Client-certificaat vingerafdrukken kopiëren
Elke persoon voor wie een clientcertificaat is afgegeven moet als volgt te werk om de vingerafdruk van his/hers certificaat dat wordt toegevoegd aan het configuratiebestand service:
* Certmgr.exe uitvoeren
* Selecteer het tabblad Persoonlijk
* Dubbelklik op dit certificaat moet worden gebruikt voor verificatie
* Selecteer het tabblad Details in het dialoogvenster certificaat dat wordt geopend
* Controleer of dat weergeven al wordt weergegeven
* Selecteer het veld met de vingerafdruk naam in de lijst
* Kopieer de waarde van de vingerafdruk **onzichtbare Unicode-tekens vóór het eerste getal verwijderen** verwijderen alle spaties

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Toegestane clients in het configuratiebestand service configureren

De waarde van de volgende instelling in het configuratiebestand service met een door komma's gescheiden lijst met de vingerafdrukken van de clientcertificaten toegang hebben tot de service bijwerken:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Controle op certificaat intrekking client configureren

De standaardinstelling wordt niet gecontroleerd met de certificeringsinstantie voor intrekkingsstatus van client-certificaat. Om in te schakelen controles, als de certificeringsinstantie die de client certificaten deze controle ondersteunt, wijzigt u de volgende instelling met een van de waarden die zijn gedefinieerd in de opsomming X509RevocationMode:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>PFX-bestand voor zelfondertekend versleutelingscertificaten maken

Voor een versleutelingscertificaat uitvoeren:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Aan te passen:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Wachtwoord Typ en exporteer het certificaat met deze opties:
*    Ja, de persoonlijke sleutel exporteren
*    Alle uitgebreide eigenschappen exporteren
*    Moet u het wachtwoord wanneer het certificaat uploaden naar de cloudservice.

## <a name="export-encryption-certificate-from-certificate-store"></a>Versleutelingscertificaat van store certificaat exporteren

*    Certificaat zoeken
*    Klik op Acties -> alle taken -> exporteren...
*    Afbeelding invoegen van SharePoint naar een. PFX-bestand met deze opties: 
  *    Ja, de persoonlijke sleutel exporteren
  *    Alle certificaten indien mogelijk in het certificeringspad opnemen 
*    Alle uitgebreide eigenschappen exporteren

## <a name="upload-encryption-certificate-to-cloud-service"></a>Versleutelingscertificaat uploaden naar cloudservice

Upload-certificaat met de bestaande of gegenereerd. PFX-bestand met een combinatie van de versleuteling sleutel:

* Voer het wachtwoord voor het beveiligen van de gegevens voor persoonlijke sleutels

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Versleutelingscertificaat service configuratiebestand bijwerken

De vingerafdrukwaarde van de volgende instellingen in het configuratiebestand service bijwerken met de vingerafdruk van het certificaat dat is geüpload naar de cloudservice:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Algemene certificaat bewerkingen

* De SSL-certificaat configureren
* Clientcertificaten configureren

## <a name="find-certificate"></a>Certificaat zoeken

Volg deze stappen:

1. Mmc.exe uitvoeren.
2. Bestand -> module toevoegen/verwijderen...
3. Selecteer **certificaten**.
4. Klik op **toevoegen**.
5. Kies de locatie van de store certificaat.
6. Klik op **Voltooien**.
7. Klik op **OK**.
8. **Certificaten**uitvouwen.
9. Vouw certificaat store.
10. Vouw het knooppunt van de onderliggende certificaat.
11. Selecteer een certificaat in de lijst.

## <a name="export-certificate"></a>Certificaat exporteren
Klik in de **Wizard Certificaat exporteren**:

1. Klik op **volgende**.
2. Selecteer **Ja**, klikt u vervolgens **de persoonlijke sleutel exporteren**.
3. Klik op **volgende**.
4. Selecteer de gewenste uitvoer-bestandsindeling.
5. Schakel de gewenste opties.
6. Schakel de **optie wachtwoord**.
7. Voer een sterk wachtwoord en bevestig dit.
8. Klik op **volgende**.
9. Typ of Selecteer een bestandsnaam opslaglocatie van het certificaat (gebruiken een. PFX extensie).
10. Klik op **volgende**.
11. Klik op **Voltooien**.
12. Klik op **OK**.

## <a name="import-certificate"></a>Certificaat importeren

In de Wizard Certificaat importeren:

1. Selecteer de locatie van de store.

    * Selecteer **Huidige gebruiker** als er slechts processen worden uitgevoerd onder huidige gebruiker toegang de service tot heeft
    * Selecteer **Lokale computer** als andere processen in deze computer toegang de service tot heeft
2. Klik op **volgende**.
3. Als uit een bestand importeren, controleert u het pad.
4. Als het importeren van een. PFX-bestand:
    1.     Voer het wachtwoord voor het beveiligen van de persoonlijke sleutel
    2.     Selecteer opties voor importeren
5.     Selecteer "Locatie" certificaten in de volgende store
6.     Klik op **Bladeren**.
7.     Selecteer de gewenste winkel.
8.     Klik op **Voltooien**.
       
    * Als de vertrouwde basiscertificeringsinstantie store is gekozen, klikt u op **Ja**.
9.     Klik op **OK** in alle vensters van het dialoogvenster.

## <a name="upload-certificate"></a>Certificaat uploaden

Klik in de [Portal van Azure](https://portal.azure.com/)

1. Selecteer **Cloudservices**.
2. Selecteer de cloudservice.
3. Klik op het bovenste menu op **certificaten**.
4. Klik op de onderste balk, op **uploaden**.
5. Selecteer het certificaatbestand.
6. Als dit is een. PFX bestand, voert u het wachtwoord voor de persoonlijke sleutel.
7. Als voltooid, kopieert u vingerafdruk van het certificaat van de nieuwe vermelding in de lijst.

## <a name="other-security-considerations"></a>Andere aandachtspunten voor de beveiliging
 
De SSL-instellingen die worden beschreven in dit document versleutelen communicatie tussen de service en de clients wanneer het eindpunt HTTPS wordt gebruikt. Dit is belangrijk sinds referenties op voor toegang tot de database en mogelijk andere gevoelige gegevens zijn opgenomen in de communicatie. Bedenk wel dat de service zich blijft interne status voordoen, referenties op te nemen in de interne tabellen in de Microsoft Azure SQL-database die u hebt opgegeven voor de metagegevensopslag van uw abonnement op Microsoft Azure. Die database is gedefinieerd als onderdeel van de volgende instelling in het configuratiebestand service (. CSCFG-bestand): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Referenties die zijn opgeslagen in deze database zijn versleuteld. Echter aangeraden, zorg ervoor dat zowel web als werknemer rollen van uw service-implementaties up-to-date blijven en veilig als ze beide toegang tot de metagegevensdatabase en het certificaat dat is gebruikt voor het coderen en decoderen van opgeslagen referenties hebt. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
