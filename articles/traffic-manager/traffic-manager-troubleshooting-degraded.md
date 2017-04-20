<properties
    pageTitle="Probleemoplossing status op Azure verkeer Manager niet beschikbaar is weergegeven"
    description="Het verkeer Manager profielen oplossen wanneer deze wordt weergegeven als niet beschikbaar is weergegeven status."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Probleemoplossing staat op Azure verkeer Manager niet beschikbaar is weergegeven

In dit artikel wordt beschreven hoe problemen met een Azure verkeer Manager profiel dat een anders werken status wordt weergegeven. In dit scenario kunt u een deel van uw services cloudapp.net gehost aan te wijzen verkeer Manager-profiel hebt geconfigureerd. Wanneer u de status van uw manager verkeer controleren, ziet u dat de Status niet beschikbaar is wordt weergegeven.

![anders werken staat](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

Wanneer u naar het tabblad van de eindpunten van dat profiel gaat, ziet u een of meer van de eindpunten met een offlinestatus van:

![offline](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## <a name="understanding-traffic-manager-probes"></a>Controleert of lidmaatschap verkeer Manager

- Verkeer Manager acht een eindpunt moet ONLINE zijn alleen wanneer de test een antwoord HTTP 200 terug van het pad test ontvangt. Een ander niet-200 antwoord is een fout.
- Een omleiding 30 x mislukt, zelfs als de omgeleide URL geeft als een 200 resultaat.
- Voor HTTPs bemonsteringssondes worden certificaatfouten genegeerd.
- De inhoud van het pad test maakt niet uit, zo lang maken als een 200 wordt geretourneerd. Zoeken naar een URL naar bepaalde statische inhoud zoals "/ favicon.ico" is een algemene techniek. Dynamische inhoud, zoals de ASP-pagina's, kan niet altijd 200 retourneren zelfs wanneer de toepassing in orde is.
- Er is een goede gewoonte voor het instellen van het pad test aan een item dat voldoende logica om te bepalen is of de site omhoog of omlaag is. In het vorige voorbeeld, door het pad op te stellen "/ favicon.ico ', alleen die w3wp.exe test reageert. Deze test geeft wellicht niet dat uw webtoepassing in orde is. De optie voor een betere zou, zoals een pad naar een iets instellen "/ Probe.aspx" die logica om te bepalen de status van de site heeft. U kan bijvoorbeeld gebruik prestatiemeteritems naar CPU-gebruik of het aantal mislukte aanvragen meten. Of u kunt proberen te krijgen tot database resources of status van de sessie om ervoor te zorgen dat de webtoepassing werkt.
- Als alle eindpunten in een profiel zijn niet beschikbaar is weergegeven, klikt u vervolgens verkeer Manager wordt verwerkt alle eindpunten orde en routes verkeer naar alle eindpunten. Dit probleem zorgt ervoor dat problemen met de zoek om niet in een volledige storing van uw service resulteren.

## <a name="troubleshooting"></a>Problemen oplossen

Om op te lossen een test is mislukt, moet u eerst een hulpmiddel met de HTTP-statuscode afzender is van de URL van de test. Zijn er veel hulpprogramma's waarin u het onbewerkte HTTP-antwoord kunt zien.

* [Fiddler](http://www.telerik.com/fiddler)
* [omslaan](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Bovendien kunt u het tabblad netwerk van de hulpmiddelen F12 foutopsporing in Internet Explorer de HTTP-antwoorden weergeven.

In dit voorbeeld we wilt zien van de reactie van onze test-URL: http://watestsdp2008r2.cloudapp.net:80/test. Het volgende PowerShell-voorbeeld ziet u het probleem.

```powershell
    Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Voorbeeld van de uitvoer:

```text
    StatusCode StatusDescription
    ---------- -----------------
            301 Moved Permanently
```

Zoals u ziet dat we een omleidings-antwoord hebt ontvangen. Zoals al eerder aangegeven, een StatusCode anders dan 200, wordt beschouwd als een fout. De Eindpuntstatus verkeer Manager gewijzigd Offline. Controleer de website-configuratie om ervoor te zorgen dat de juiste StatusCode kan worden geretourneerd van het pad Test het probleem op te lossen. De configuratie van het verkeer Manager-test zodat deze verwijzen naar een pad dat geeft als een 200 resultaat.

Als uw test het HTTPS-protocol wordt gebruikt, moet u mogelijk certificaat controleren SSL/TLS om fouten te voorkomen tijdens uw test uitschakelen. De volgende PowerShell-instructies uitschakelen certificaatverificatie voor de huidige PowerShell-sessie:

```powershell
    add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    public class TrustAllCertsPolicy : ICertificatePolicy {
        public bool CheckValidationResult(
        ServicePoint srvPoint, X509Certificate certificate,
        WebRequest request, int certificateProblem) {
        return true;
        }
    }
    "@
    [System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Volgende stappen

[Over verkeer Manager verkeer routeren methoden](traffic-manager-routing-methods.md)

[Wat is verkeer Manager](traffic-manager-overview.md)

[Cloudservices](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure WebApps](https://azure.microsoft.com/documentation/services/app-service/web/)

[Bewerkingen op verkeer Manager (REST API overzicht)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure verkeer Manager-Cmdlets][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
