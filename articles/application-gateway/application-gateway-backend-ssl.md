<properties
   pageTitle="Inschakelen van SSL-beleid en complete SSL op Gateway-toepassing | Microsoft Azure"
   description="Deze pagina bevat een overzicht van de toepassingsgateway complete SSL ondersteunen."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Inschakelen van SSL-beleid en complete SSL op Gateway-toepassing

## <a name="overview"></a>Overzicht

Toepassingsgateway ondersteunt SSL-beëindiging op de gateway, nadat u niet welke meestal verkeersstromen versleuteld met de backend-servers. Hierdoor endwebservers moeten unburdened van dure ontsleutelen boven. Voor sommige klanten versleutelde communicatie met de backend-servers is echter niet een aanvaardbaar optie. Dit kan vanwege beveiliging/nalevingsvereisten of de toepassing mogelijk alleen beveiligde verbinding accepteren. Voor deze toepassingen ondersteunt toepassingsgateway nu complete SSL-versleuteling.

Einde naar einde SSL, kunt u veilig gevoelige gegevens verzenden naar de backend versleutelde statische profiteert van de voordelen van functies van taakverdeling laag 7 welke toepassingsgateway, zoals cookie affiniteit, op basis van een URL routering, ondersteuning biedt voor routering gebaseerd op sites of de mogelijkheid om toe te voegen van X-doorgestuurde-* kopteksten.

Wanneer geconfigureerd met complete SSL communicatiemodus, wordt toepassingsgateway wordt beëindigd gebruiker SSL-sessies in de gateway en ontsleutelt gebruiker-verkeer is toegestaan. Vervolgens wordt de geconfigureerde regels om te selecteren een exemplaar van de juiste backend toepassingen naar route-verkeer om te toegepast. Toepassingsgateway wordt vervolgens een nieuwe SSL-verbinding met endserver gestart en opnieuw worden gecodeerd gegevens met behulp van de backend-server-certificaat met openbare sleutel voordat het verzoek om op de backend verzendt. Beëindigen om te beëindigen SSL is ingeschakeld door protocol stellen in BackendHTTPSetting in Https, die vervolgens wordt toegepast op een back-end-toepassingen. Elke endserver in de groep backend met complete SSL is ingeschakeld, moet worden geconfigureerd met een certificaat wilt beveiligde communicatie toestaan.

![complete ssl-scenario][1]

In dit voorbeeld zijn aanvragen via TLS1.2 omgeleid naar de backend-servers in Pool1 via complete SSL.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Beëindigen om te beëindigen SSL en whitelisting van certificaten

Toepassingsgateway wordt alleen communiceert met bekende backend-exemplaren die hun certificaat met de toepassingsgateway hebt opgeslagen whitelisted. Om te schakelen whitelisting van certificaten, moet u de openbare sleutel van certificaten voor backend-server uploaden naar de toepassingsgateway (niet de hoofdmap certificaat). Alleen verbindingen met bekende en whitelisted backends zijn vervolgens toegestaan. De resterende backends resultaten in een gateway-fout. Zelfondertekende certificaten zijn geschikt voor testdoeleinden alleen en niet aanbevolen voor productie werkbelasting. Deze certificaten moeten eveneens whitelisted met de toepassingsgateway zoals is beschreven in de voorgaande stappen voordat ze kunnen worden gebruikt.

## <a name="application-gateway-ssl-policy"></a>Toepassing Gateway SSL beleid

Toepassingsgateway ondersteunt configureerbare SSL onderhandelingen gebruikersbeleid, zodat klanten meer controle over SSL-verbindingen in de toepassingsgateway.

1. SSL 2.0 en 3.0 standaard uitgeschakeld voor alle Gateways van toepassing. Ze worden niet geconfigureerd helemaal.
2. SSL-beleid definitie hebt u een optie voor het uitschakelen van de volgende 3 protocollen - TLSv1\_0, TLSv1\_1, TLSv1\_2.
3. Als er geen SSL-beleid is gedefinieerd alle drie (TLSv1\_0, TLSv1\_1, TLSv1_2) zijn ingeschakeld.

## <a name="next-steps"></a>Volgende stappen

Na het raadplegen van einde naar einde SSL en SSL-beleid, Ga naar het [complete SSL op toepassingsgateway inschakelen](application-gateway-end-to-end-ssl-powershell.md) voor het maken van een toepassingsgateway waarbij verkeer verzenden naar backends gecodeerd.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png