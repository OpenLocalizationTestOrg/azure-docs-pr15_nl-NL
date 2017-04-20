<properties
    pageTitle="Toegangsbeheer op basis van access besturingselement probleemoplossing | Microsoft Azure"
    description="Hulp bij problemen of vragen over rol gebaseerd toegangsbeheer resources."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Rolgebaseerd toegangsbeheer problemen oplossen

## <a name="introduction"></a>Inleiding

[Toegangsbeheer op basis van een rol](role-based-access-control-configure.md) is een krachtige functie waarmee u fijnmazige Gemachtigdentoegang aan resources in Azure. Dit betekent dat u kunt zo te zijn dat er zeker van te zijn toekennen van een bepaalde persoon recht gebruik precies wat ze nodig hebt en niet meer. Echter kan soms het model resource voor Azure resources ingewikkeld en kan het lastig kunnen begrijpen precies wat u verleent machtigingen als u wilt.

In dit document zal u laten weten wat u kunt verwachten als u met enkele van de rollen in de portal van Azure. Deze drie rollen hebben betrekking op alle typen:

- Eigenaar  
- Inzender  
- Lezer  

Eigenaren en inzenders hebt volledige toegang tot de management-ervaring, maar een inzender kan geen toegang geven tot de andere gebruikers of groepen. Dingen krijgen iets interessanter met de rol van de lezer, dus die waar we enige tijd hebt besteed. Zie het [Rolgebaseerd toegangsbeheer gestart get artikel](role-based-access-control-configure.md) voor meer informatie over het verlenen van toegang.

## <a name="app-service-workloads"></a>App-service werkbelasting

### <a name="write-access-capabilities"></a>Schrijftoegang mogelijkheden

Als u een gebruiker alleen-lezen toegang tot een enkel web-app geven, worden sommige functies uitgeschakeld die u niet zou verwachten. De volgende beheermogelijkheden **schrijven** toegang tot een web-app (medewerker of eigenaar) nodig hebben en niet beschikbaar in een scenario voor alleen-lezen is.

- Opdrachten (bijvoorbeeld starten, stoppen, enz.)
- Wijzigen van de instellingen zoals algemene configuratie, schaalinstellingen instellingen back-ups en controle-instellingen
- Toegang tot publicerende referenties en andere geheimen zoals app-instellingen en verbindingstekenreeksen
- Logboeken streaming
- Diagnostische logboeken configuratie
- Console (opdrachtprompt)
- Actieve en recente implementaties (voor lokale cijfer continue-implementatie)
- Geschatte besteedt
- Web tests
- Virtuele netwerk (alleen zichtbaar voor een reader als een virtueel netwerk eerder is geconfigureerd door een gebruiker met schrijftoegang).

Als u geen toegang deze tegels tot, moet u de beheerder vragen voor Inzender toegang tot de web-app.

### <a name="dealing-with-related-resources"></a>Omgaan met verwante resources

WebApps zijn ingewikkeld door de aanwezigheid van een paar andere bronnen die wisselwerking. Hier volgt een typisch resourcegroep met een paar websites:

![Resourcegroep web-app](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Als een resultaat, als u iemand toegang tot alleen de WebApp veel van de functionaliteit voor op het blad website in de portal van Azure wordt uitgeschakeld verlenen.

Deze items vereisen **schrijven** toegang tot het **App-serviceplan** die met uw website overeenkomt:  

- Weergeven van de web-app bevindt zich prijzen laag (gratis of standaard)  
- Configuratie van de schaal (aantal exemplaren, VM grootte, de instellingen automatisch schalen)  
- Quota's (opslag, bandbreedte, CPU)  

Deze items vereisen **schrijven** toegang tot de hele **resourcegroep** waartoe uw website:  

- SSL-certificaten en bindingen (dit is omdat SSL-certificaten tussen sites in dezelfde resourcegroep en geografische locatie kunnen worden gedeeld)  
- Waarschuwingsregels  
- Instellingen voor automatisch schalen  
- Toepassingsonderdelen inzichten  
- Web tests  

## <a name="virtual-machine-workloads"></a>VM werkbelasting

Net zoals met WebApps, sommige functies op het blad VM vereist schrijftoegang tot de virtuele machine, of tot andere bronnen in de resourcegroep.

Virtuele machines betrekking hebben op domein namen, virtuele netwerken opslag accounts en waarschuwingsregels.

Deze items vereisen **schrijven** toegang tot de **VM**:

- Eindpunten  
- IP-adressen  
- Schijven  
- Uitbreidingen  

Deze nodig **schrijven** toegang tot de **virtuele machine**en de **resourcegroep** (samen met de naam van het domein) die in:  

- Beschikbaarheid instellen  
- Laden gebalanceerde instellen  
- Waarschuwingsregels  

Als u geen toegang deze tegels tot, moet u de beheerder vragen voor Inzender toegang tot de resourcegroep.

## <a name="see-more"></a>Zie voor meer informatie
- [Rol gebaseerd toegangsbeheer](role-based-access-control-configure.md): aan de slag met RBAC in de portal van Azure.
- [Ingebouwde rollen](role-based-access-built-in-roles.md): informatie over de rollen die worden standaard in RBAC geleverd.
- [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md): informatie over het maken van aangepaste rollen aan uw behoeften voldoet van access.
- [Maken een access-geschiedenisrapport wijzigen](role-based-access-control-access-change-history-report.md): veranderende roltoewijzingen in RBAC bijhouden van.
