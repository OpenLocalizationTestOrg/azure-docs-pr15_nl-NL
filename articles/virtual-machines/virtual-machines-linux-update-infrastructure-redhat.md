<properties
   pageTitle="Rode rol Update infrastructuur (RHUI) | Microsoft Azure"
   description="Meer informatie over rood rol Update infrastructuur (RHUI) voor op aanvraag Red rol Enterprise Linux exemplaren in Microsoft Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Rode rol Update infrastructuur (RHUI) voor het op aanvraag rode rol Enterprise Linux VMs in Azure wordt aangegeven

Virtuele machines gemaakt op basis van het op aanvraag rood rol Enterprise Linux (RHEL) afbeeldingen beschikbaar zijn in Azure Marketplace zijn geregistreerd voor toegang tot de rood rol Update infrastructuur (RHUI) geïmplementeerd in Azure wordt aangegeven.  Het op aanvraag RHEL exemplaren hebben toegang naar een bibliotheek regionale yum en kan incrementele updates ontvangen.

De lijst van de bibliotheek yum, die wordt beheerd door RHUI, is geconfigureerd in uw exemplaar RHEL tijdens het inrichten. U hoeft te doen extra configuratie - uitvoeren `yum update` nadat uw RHEL exemplaar klaar is om aan de meest recente updates.

> [AZURE.NOTE] Azure RHUI-infrastructuur is onlangs bijgewerkt (September 2016) en wijzigingen in de configuratie van uw bestaande RHEL exemplaren vereist voor ononderbroken toegang tot de RHUI Azure. Raadpleeg de sectie RHUI Azure infrastructuur-Update voor meer informatie.


## <a name="rhui-azure-infrastructure-update"></a>RHUI Azure infrastructuur-Update
Azure heeft vanaf September 2016, een nieuwe set rood rol Update infrastructuur (RHUI)-servers. Deze servers zijn geïmplementeerd met [Azure verkeer Manager]( https://azure.microsoft.com/services/traffic-manager/) , zodat een enkelvoudig eindpunt (rhui-1.micrsoft.com) kan worden gebruikt door een VM ongeacht regio. Ze ook een SSL-certificaat dat is geschakeld naar een bekende certificeringsinstantie (Baltimore root) gebruiken. Deze update automatische waardoor zou schadelijke voor sommige klanten met ACL's of aangepaste routeren tabellen voor de servers van de update RHUI zodat deze update is "opt-in." Handmatige stappen voor het onboarding met deze nieuwe servers zijn beschikbaar op deze pagina en een volledige script voor onboarding geautomatiseerd voortgezet (na de verificatie van de afzonderlijke stappen). De nieuwe RHEL PAYG afbeeldingen in de Azure Marketplace (versies gedateerd September 2016 of hoger) wordt automatisch verwijzen naar de nieuwe Azure RHUI-servers en geen extra maatregelen vereist.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>De nieuwe Azure RHUI infrastructuur onboarding tijdlijn

| Datum | Opmerking |
| --- | --- |
|22 september 2016 | RHUI servers en installatie-instructies beschikbaar maken voor gebruik. Met behulp van de nieuwe (September 2016 gedateerd is) geïmplementeerd VMs RHEL PAYG marketplace afbeeldingen worden automatisch de nieuwe RHUI-servers gebruikt, maar bestaande VMs zijn "opt-in"
|1 november 2016 | Oudere RHEL PAYG VM-afbeeldingen (waarmee de oude Azure RHUI-servers) worden verwijderd uit de galerie met Azure Marketplace
|16 januari 2017 | Wordt de oude Azure RHUI-servers buiten gebruik worden gesteld. Alle van de desbetreffende PAYG RHEL VMs bijwerken door nu met toegang tot Azure RHUI onderhouden

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Het IP-adressen voor de nieuwe RHUI contentlevering servers zijn

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Handmatig bijwerken procedure voor het gebruik van de nieuwe Azure RHUI-servers

Download (via krul) van de openbare sleutel-handtekening

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Controleer of de gedownloade-toets

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Controleer de uitvoer, controleert u of `keyid` en `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

De openbare sleutel installeren

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Downloaden, verifiëren en RPM-Client installeren

Download: Voor RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Voor RHEL 7

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Controleer of:

```
rpm -Kv azureclient.rpm
```

Schakel in de uitvoer die handtekening van het pakket is het OK

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

De RPM installeren

```
sudo rpm -U azureclient.rpm
```

Controleer of u Azure RHUI formulier de VM kunt openen na voltooiing,

### <a name="all-in-one-script-for-automating-the-above-task"></a>Alles in één script voor het automatiseren van de bovenstaande taak
Gebruik het volgende script zo nodig om de taak van het desbetreffende VMs bijwerken met de nieuwe Azure RHUI-servers te automatiseren.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>RHUI overzicht
[Rood rol Update infrastructuur](https://access.redhat.com/products/red-hat-update-infrastructure) biedt een zeer scalable oplossing yum opslagplaats inhoud voor Red rol Enterprise Linux cloud exemplaren die worden gehost door rood rol gecertificeerd cloud providers beheren. Op basis van het boven Pulp project, kunt RHUI cloud-providers lokaal gespiegelde rood rol gehoste opslagplaats inhoud, aangepaste opslagplaatsen met hun eigen inhoud maken en deze opslagplaatsen beschikbaar maken voor een grote groep eindgebruikers via een systeem inhoud bezorging van taakverdeling.

## <a name="regions-where-rhui-is-available"></a>Regio's waar RHUI beschikbaar is
RHUI is beschikbaar in alle regio's waarin RHEL op aanvraag afbeeldingen beschikbaar zijn. Het bevat momenteel alle openbare regio's die worden vermeld op de dashboardpagina [Azure status](https://azure.microsoft.com/status/) en de overheid van Azure ons regio's. RHUI toegang voor deze is ingericht vanuit RHEL op aanvraag afbeeldingen VMs is opgenomen in hun prijs. Aanvullende land/nationale cloud beschikbaarheid wordt bijgewerkt terwijl we RHEL op aanvraag beschikbaar in de toekomst uitvouwen.

> [AZURE.NOTE] Toegang tot RHUI Azure gehoste is beperkt tot de VMs binnen [Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="get-updates-from-another-update-repository"></a>Updates ophalen van een andere update opslagplaats

Als u nodig hebt voor updates van een andere update opslagplaats (in plaats van Azure gehoste RHUI) moet u naar uw exemplaren van RHUI unregister en opnieuw wilt registreren met de gewenste update-infrastructuur (zoals rood rol satelliet of rood rol klant Portal CDN). U moet juiste rood rol-abonnementen voor deze services en de registratie voor [Rood rol Cloudtoegang in Azure wordt aangegeven](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).

RHUI unregister en registreren naar uw update infrastructuur volgen de onderstaande stappen te volgen.

1.  /Etc/yum.repos.d/rh-cloud.repo bewerken en wijzigen van alle `enabled=1` naar `enabled=0`. Bijvoorbeeld:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  /Etc/yum/pluginconf.d/rhnplugin.conf bewerken en wijzig `enabled=0` naar `enabled=1`.
3.  Klik met de gewenste infrastructuur, zoals rood rol Customer Portal registreren. Rood rol oplossing handleiding volgen voor [het registreren en een systeem bij de Portal rood rol klant abonneren](https://access.redhat.com/solutions/253273).

> [AZURE.NOTE] Toegang tot de RHUI Azure gehoste is opgenomen in de prijs van de afbeelding RHEL Pay-As-You-Go (PAYG). Afmelden van een PAYG RHEL VM uit de RHUI Azure gehoste wordt niet geconverteerd de virtuele machine naar voren-uw-eigenaar bent van-licentie (BYOL) type VM en dus u mogelijk worden dat dubbele kosten als u de dezelfde VM met een andere bron van updates registreert. 
> 
> Als u consistente moet gebruiken een update-infrastructuur anders dan het Azure gehoste RHUI kunt maken en implementeren van uw eigen afbeeldingen (BYOL-type) zoals beschreven in [maken en uploaden rood rol virtuele machine voor Azure](virtual-machines-linux-redhat-create-upload-vhd.md) artikel.

## <a name="next-steps"></a>Volgende stappen
Een rood rol Enterprise Linux VM maken van Azure Marketplace Pay-As-You-Go afbeelding en middelen Azure gehoste RHUI gaat u naar [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). U wordt wel gebruik `yum update` in uw exemplaar RHEL zonder eventuele aanvullende instellingen.