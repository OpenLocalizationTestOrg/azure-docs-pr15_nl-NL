<properties 
   pageTitle="Problemen met een uitgevouwen StorSimple-apparaat | Microsoft Azure"
   description="Wordt beschreven hoe automatisch opsporen en oplossen van fouten die zich op een apparaat StorSimple die momenteel is geïmplementeerd en operationele."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Problemen met een operationele StorSimple-apparaat

## <a name="overview"></a>Overzicht

In dit artikel bevat nuttige voor probleemoplossing richtlijnen voor het oplossen van problemen met de configuratie dat u tegenkomen kunt nadat uw apparaat StorSimple geïmplementeerd en operationele is. Hierin worden veelvoorkomende problemen, mogelijke oorzaken en aanbevolen stappen waarmee u problemen kunt oplossen die optreden mogelijk wanneer u Microsoft Azure StorSimple uitvoert. Deze informatie geldt voor zowel het StorSimple on-premises implementatie fysiek en de StorSimple virtuele apparaat.

Aan het einde van dit artikel vindt u een lijst met foutcodes die tijdens Microsoft Azure StorSimple optreden kunnen, evenals stappen kunt u ondernemen om de fouten op te lossen. 

## <a name="setup-wizard-process-for-operational-devices"></a>Wizard installatieprocedure voor operationele apparaten

U gebruikt de wizard setup ([Roep HcsSetupWizard][1]) om te controleren van de configuratie van het apparaat en corrigeer indien nodig.

Wanneer u de wizard setup op een apparaat eerder geconfigureerde en operationele uitvoert, wordt de processtroom verschilt. U kunt alleen de volgende gegevens wijzigen:

- IP-adres, subnet invoermasker en gateway
- Primaire DNS-server
- Primaire NTP-server
- Configuratie van proxy optioneel web

De wizard setup voert de bewerkingen die betrekking hebben op wachtwoord verzamelen en apparaat registratie geen.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Fouten tijdens het volgende wordt uitgevoerd van de wizard setup

De volgende tabel beschrijft de fouten die u optreden kan wanneer u de installatiewizard op een operationele-apparaat, de mogelijke oorzaken van de fouten uitvoeren en aanbevolen acties op te lossen. 

| Nee. | Foutbericht wordt weergegeven of voorwaarde | Mogelijke oorzaken | Aanbevolen actie |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | Fout 350032: Dit apparaat is al uitgeschakeld. | U kunt deze fout wordt weergegeven als u de wizard setup uitvoert op een apparaat dat is gedeactiveerd. | [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor de volgende stappen. Een gedeactiveerd apparaat kan niet worden geplaatst in de service. Het opnieuw instellen van een fabriek mogelijk nodig voordat het apparaat opnieuw kan worden geactiveerd. |
|  2  | Roepen-HcsSetupWizard: ERROR_INVALID_FUNCTION (uitzondering van HRESULT: 0x80070001) | Het bijwerken van DNS-server is verbroken. DNS-instellingen zijn algemene instellingen en worden toegepast op alle ingeschakelde netwerkinterfaces. | De interface inschakelen en opnieuw toepassen van de DNS-instellingen. Dit kan het netwerk voor andere ingeschakeld interfaces verstoort omdat deze instellingen globale zijn. |
|  3  | Het apparaat wordt weergegeven in de serviceportal StorSimple Manager online zijn, maar wanneer u probeert de minimale setup voltooit en opslaan van de configuratie, de bewerking is mislukt. | Tijdens de eerste installatie is de webproxy niet geconfigureerd, hoewel er een werkelijke proxyserver op hun plaats staan is. | Gebruik de [cmdlet Test-HcsmConnection] [ 2] om te zoeken van de fout. [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) als u niet het probleem te verhelpen. |
|  4  | Roepen-HcsSetupWizard: Waarde valt niet binnen het verwachte bereik. | Een invoermasker onjuiste subnet, levert deze fout. Mogelijke oorzaken zijn: <ul><li> Het subnetmasker is ontbreekt of is leeg.</li><li>De indeling van de voorvoegsel Ipv6 is onjuist.</li><li>De interface is cloud is ingeschakeld, maar de gateway is ontbrekende of onjuiste.</li></ul>Houd er rekening mee dat gegevens 0 automatisch cloud ingeschakelde is indien geconfigureerd de wizard setup gaat gebruiken. | Om te bepalen wat het probleem, subnet 0.0.0.0 of 256.256.256.256 gebruik en kijkt u naar de uitvoer. Geef juiste waarden voor de subnetmasker, gateway en IPv6-voorvoegsel, indien nodig. |
 
## <a name="error-codes"></a>Foutcodes

Fouten worden weergegeven in numerieke volgorde.

|Foutnummer|Fouttekst of beschrijving|Aanbevolen gebruikersactie|
|:---|:---|:---|
|10502|Er is een fout opgetreden bij het openen van uw account opslag.|Wacht enkele minuten en probeer het opnieuw. Als de fout zich blijft voordoen, neemt u contact opnemen met Microsoft ondersteuning voor de volgende stappen.|
|40017|De back-up is mislukt tijdens een volume dat is opgegeven in het back-beleid niet op het apparaat gevonden is.|Probeer de back-up betrekking heeft, als de fout zich blijft voordoen, neem contact op met Microsoft Support. voor de volgende stappen.|
|40018|De back-up is mislukt als geen van de opgegeven in het back-beleid hoeveelheden op het apparaat gevonden. |Probeer de back-up betrekking heeft, als de fout zich blijft voordoen, neem contact op met Microsoft Support. voor de volgende stappen.|
|390061|Het systeem is bezet of niet beschikbaar.|Wacht enkele minuten en probeer het opnieuw. Als de fout zich blijft voordoen, neemt u contact opnemen met Microsoft ondersteuning voor de volgende stappen.|
|390143|Er is een fout opgetreden met foutcode 390143. (Onbekende fout).|Als de fout zich blijft voordoen, neemt u contact op met Microsoft Support voor de volgende stappen.|

## <a name="next-steps"></a>Volgende stappen

Als u niet voor het oplossen van het probleem, [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor hulp. 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
