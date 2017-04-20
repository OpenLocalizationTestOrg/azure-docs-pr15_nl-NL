<properties 
   pageTitle="Wijzig de gegevens 0 instellingen op een apparaat StorSimple | Microsoft Azure"
   description="Leer hoe u Windows PowerShell voor StorSimple gebruiken om te configureren, de interface van de gegevens 0 netwerk op uw apparaat StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-device"></a>De gegevens 0 netwerk interface-instellingen op uw apparaat StorSimple wijzigen

## <a name="overview"></a>Overzicht

Het apparaat Microsoft Azure StorSimple heeft zes netwerkinterfaces, van gegevens 0 tot en met 5 van de gegevens. De gegevens 0 interface altijd via de Windows PowerShell-interface of de seriële console is geconfigureerd, en wordt automatisch door de cloud ingeschakeld. Houd er rekening mee dat u gegevens 0 netwerkinterface via de portal van Azure klassieke geen configureren. 

De gegevens 0 interface eerst de wizard setup gaat gebruiken tijdens heeft de eerste installatie van het apparaat StorSimple. Wanneer het apparaat in een operationele modus is, moet u mogelijk opnieuw configureren gegevens 0 instellingen. Deze zelfstudie bestaat uit twee methoden voor het wijzigen van gegevens 0 netwerk beide tot en met Windows PowerShell voor StorSimple-instellingen.

Na het lezen van deze zelfstudie, wordt u zichtbaar mogen zijn:

- Wijzigen van gegevens 0 netwerk instelling de wizard setup gaat gebruiken
- Instellingen voor het netwerk van 0 tot en met gegevens wijzigen de `Set-HcsNetInterface` cmdlet



## <a name="modify-data-0-network-settings-through-setup-wizard"></a>GEGEVENS 0 netwerkinstellingen wizard setup gaat gebruiken wijzigen
U kunt gegevens 0 netwerkinstellingen opnieuw configureren door verbinding maakt met de Windows PowerShell-interface van uw apparaat StorSimple en een sessie van de wizard setup starten. Voer de volgende stappen uit als u wilt wijzigen van gegevens 0 instellingen:

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>Voor het wijzigen van gegevens 0 netwerkinstellingen wizard setup gaat gebruiken

1. Kies optie 1, **aanmelden met de volledige toegang**in het menu seriële console. Wanneer u wordt gevraagd het **apparaat beheerderswachtwoord**opgeven. Het standaardwachtwoord is `Password1`.

2. Typ bij de opdrachtprompt:

    `Invoke-HcsSetupWizard`

3. Een installatiewizard wordt weergegeven waarmee u kunt de gegevens 0 configureren interface van uw apparaat. Nieuwe waarden opgeven voor het IP-adres, gateway en netmask.

> [AZURE.NOTE] De vaste controllers IP-adressen moet worden geconfigureerd door de pagina **configureren** met het apparaat StorSimple in de portal van Azure klassieke. Ga naar het [netwerk via interfaces zoals wijzigen](storsimple-modify-device-config.md#modify-network-interfaces)voor meer informatie.


## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>GEGEVENS 0 netwerkinstellingen tot en met de cmdlet Set-HcsNetInterface wijzigen
Een andere manier om te configureren, gegevens 0 netwerkinterface door middel van is de `Set-HcsNetInterface` cmdlet. De cmdlet wordt uitgevoerd vanuit de Windows PowerShell-interface van uw apparaat StorSimple. Wanneer u met deze procedure, kan de controller vaste IP-adressen ook hier worden geconfigureerd. De volgende stappen uitvoeren om te wijzigen van de gegevens 0 instellingen: 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>Voor het wijzigen van gegevens 0 netwerkinstellingen tot en met de cmdlet Set-HcsNetInterface

1. Kies optie 1, **aanmelden met de volledige toegang**in het menu seriële console. Wanneer u wordt gevraagd het wachtwoord van de beheerder apparaat opgeven. Het standaardwachtwoord is `Password1`.

2. Typ bij de opdrachtprompt:

    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
    
    Typ de volgende waarden voor gegevens 0 in de punthaken:
                                            
    - IPv4-adres
    
    - IPv4-gateway
    
    - IPv4-subnet invoermasker
    
    - Vaste IPv4-adres voor Controller 0

    - Vaste IPv4-adres voor Controller-1

    Ga naar de [Windows PowerShell voor verwijzing naar StorSimple-cmdlets](https://technet.microsoft.com/library/dn688161.aspx)voor meer informatie over het gebruik van deze cmdlet.

## <a name="next-steps"></a>Volgende stappen

- Als u wilt configureren via netwerkinterfaces zoals dan gegevens 0, kunt u de [pagina in de portal van Azure klassieke configureren](storsimple-modify-device-config.md). 

- Als er problemen optreden tijdens het configureren van uw netwerkinterfaces, raadpleegt u [problemen met de implementatie van problemen](storsimple-troubleshoot-deployment.md).

