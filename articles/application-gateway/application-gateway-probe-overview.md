

<properties
   pageTitle="Servicestatus bewaken overzicht voor Azure-toepassingsgateway | Microsoft Azure"
   description="Meer informatie over de controlefuncties in Azure-toepassingsgateway"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="application-gateway-health-monitoring-overview"></a>Toepassing Gateway systeemstatus controleren-overzicht

Azure-toepassingsgateway al dan niet standaard wordt de status van alle resources in de groep back-enddatabase en worden automatisch verwijderd elke resource beschouwd als beschadigd uit de groep. Toepassingsgateway blijft de beschadigde exemplaren volgen en weer aan de orde back-enddatabase-groep toevoegt zodra ze beschikbaar en op servicestatus sondes reageren. Toepassingsgateway wordt gestuurd controleert of de status met dezelfde poort die is gedefinieerd in de back-enddatabase HTTP-instellingen. Dit zorgt ervoor dat de test testen van de poort die klanten verbinding maken met de backend gebruikt.

![Voorbeeld van toepassing gateway-test][1]

Naast standaard test statuscontrole, kunt u ook de status-test aan de vereisten van uw toepassing aanpassen. In dit artikel, aangepaste status sondes en standaard vallen.

## <a name="default-health-probe"></a>Standaard systeemstatus-test

Een toepassingsgateway wordt automatisch geconfigureerd in een standaard-test voor status wanneer u een aangepaste test-configuratie niet ingesteld. Het gedrag van de controle werkt door een HTTP-aanvraag aanbrengt in de IP-adressen geconfigureerd voor de groep back-enddatabase. Voor standaard sondes als de backend http-instellingen zijn geconfigureerd voor HTTPS, de test gebruikt https ook status van de backends test.

Bijvoorbeeld: U uw toepassingsgateway back-end-servers A, B en C gebruiken voor het ontvangen van HTTP-netwerkverkeer op poort 80 configureren. De standaard-statuscontrole Hiermee test u de drie servers elke 30 seconden voordat een correct HTTP-reactie. Een correct HTTP-antwoord heeft een [statuscode](https://msdn.microsoft.com/library/aa287675.aspx) tussen 200 en 399.

Als de standaard-test controleren voor server A mislukt, de toepassingsgateway verwijderd uit de groep back-enddatabase en netwerkverkeer stopt die doorloopt naar deze server. De standaard-test niet is opgelost wilt controleren op de server een elke 30 seconden. Wanneer de server A reageert succes op een verzoek om van een standaard-test voor status, deze weer als correct is toegevoegd aan de groep back-enddatabase en verkeer wordt gestart die doorloopt naar de server opnieuw.

### <a name="default-health-probe-settings"></a>Standaardinstellingen voor status-test

|Eigenschap zoeken | Waarde | Beschrijving|
|---|---|---|
| URL zoeken| http://127.0.0.1:\<poort\>/ | URL-pad |
| Interval | 30 | Interval in seconden zoeken |
| Time-out  | 30 | Time-out voor zoeken in seconden |
| Drempelwaarde voor beschadigd | 3 | Aantal nieuwe pogingen onderzoeken. De back-end-server is gemarkeerd omlaag nadat de opeenvolgende test mislukt telling de beschadigde drempel bereikt. |

> [AZURE.NOTE] De poort zijn altijd dezelfde poort als de backend-HTTP-instellingen.

De standaard-test kijkt alleen naar http://127.0.0.1:\<poort\> om te bepalen de status van de servicestatus. Als u de systeemstatus-test moet voor gaat u naar een aangepaste URL of het wijzigen van de andere instellingen configureren, moet u aangepaste sondes gebruiken zoals beschreven in de volgende stappen uit.

## <a name="custom-health-probe"></a>Aangepaste status-test

Aangepaste sondes kunnen u een uitgebreider controle over de statuscontrole hebt. Wanneer u een aangepaste sondes gebruikt, kunt u het interval test, de URL en pad om te testen en het aantal mislukte antwoorden accepteren voordat het exemplaar van de back-enddatabase toepassingen als beschadigd gemarkeerd.

### <a name="custom-health-probe-settings"></a>Aangepaste status test-instellingen

De volgende tabel bevat definities voor de eigenschappen van een aangepaste status-test.

|Eigenschap zoeken| Beschrijving|
|---|---|
| Naam | Naam van de test. Deze naam wordt gebruikt om te verwijzen naar de test in back-enddatabase HTTP-instellingen. |
| Protocol | Het protocol dat wordt gebruikt om te verzenden de test. De test wordt het protocol dat is gedefinieerd in de instellingen van de HTTP-back-enddatabase gebruiken |
| Host |  Hostnaam verzenden de test. Van toepassing alleen als er meerdere site is geconfigureerd op de Gateway-toepassing, anders gebruiken '127.0.0.1'. Dit is anders dan VM hostnaam. |
| Pad | Relatieve pad naar de test. Het geldige pad begint van '/'. |
| Interval | Onderzoeken interval in seconden. Dit is het interval tussen twee opeenvolgende sondes.|
| Time-out | Time-out voor zoeken in seconden. De test is gemarkeerd als mislukt als een geldig antwoord niet binnen deze periode time-out wordt ontvangen. |
| Drempelwaarde voor beschadigd | Aantal nieuwe pogingen onderzoeken. De back-end-server is gemarkeerd omlaag nadat het aantal opeenvolgende test fouten de beschadigde drempel bereikt. |

> [AZURE.IMPORTANT] Als de toepassingsgateway is geconfigureerd voor één site, al dan niet standaard de Host moet naam worden opgegeven als '127.0.0.1', tenzij anderszins geconfigureerd in aangepaste test.
Voor de verwijzing een aangepaste test wordt verzonden naar \<protocol\>://\<host\>:\<poort\>\<pad\>. De poort is dezelfde poort zoals gedefinieerd in de back-enddatabase HTTP-instellingen.

## <a name="next-steps"></a>Volgende stappen

Na het leren werken met statuscontrole van de Gateway-toepassing, kunt u een [aangepaste status test](application-gateway-create-probe-portal.md) in de portal van Azure of een [aangepaste status test](application-gateway-create-probe-ps.md) met PowerShell en het implementatiemodel resourcemanager Azure.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png