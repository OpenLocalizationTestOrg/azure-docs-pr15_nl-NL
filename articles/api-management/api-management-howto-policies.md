<properties 
    pageTitle="Beleid voor het beheer van Azure API ter | Microsoft Azure" 
    description="Informatie over het maken, bewerken en beleid voor het beheer van de API ter configureren." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


#<a name="policies-in-azure-api-management"></a>Beleid voor het beheer van Azure API ter

Beheer van Azure API zijn beleidsregels voor een krachtige videomogelijkheden van het systeem waarmee de uitgever die u wilt wijzigen van het gedrag van de API tot en met de configuratie. Beleidsregels zijn antwoord van een API of een verzameling instructies die opeenvolging op het verzoek worden uitgevoerd. Instructies voor populaire conversie van bestandsindeling van XML-naar JSON opnemen en tarief beperken als u wilt beperken van de hoeveelheid binnenkomende oproepen van een ontwikkelaar bellen. Beleidsregels voor veel meer nog beschikbaar zijn van het vak.

Zie het [Beleidsverwijzing][] voor een volledige lijst van beleid-instructies en de bijbehorende instellingen.

In de gateway dat wordt weergegeven tussen de API-consumenten en de beheerde API het beleid wordt toegepast. De gateway ontvangt van alle aanvragen en deze naar de onderliggende API ongewijzigd meestal doorstuurt. Een beleid kunt echter wijzigingen toepassen op de binnenkomende aanvraag en de uitgaande antwoord.

Beleid expressies kunnen worden gebruikt als kenmerkwaarden of tekst in een van de API beleidsregels voor, tenzij anderszins Hiermee geeft u het beleid. Bepaalde beleidsinstellingen zoals het beleid [besturingsstroom][] en [stelt u variabele][] zijn gebaseerd op beleid expressies. Zie [Geavanceerde beleidsregels][] en [beleid expressies][]voor meer informatie.

## <a name="scopes"> </a>Beleidsregels configureren
Beleid kunnen worden geconfigureerd globaal of het bereik van een [Product][], de [API][] of de [bewerking][]. Een beleid wilt configureren, navigeer naar de beleidsregels editor in de publisher-portal.

![Menu beleid][policies-menu]

De editor beleidsregels bestaat uit drie gedeelten: de reikwijdte van het beleid (boven), de definitie van het beleid waar beleid (links) worden bewerkt en de beweringen lijst (rechts):

![Beleid-editor][policies-editor]

Om te beginnen met het configureren van een beleid selecteert u eerst het bereik waarop het beleid wilt gebruiken. In de schermafbeelding onder de **Starter** is product ingeschakeld. Houd er rekening mee dat het symbool voor de vierkante naast de naam van het beleid wordt aangegeven dat een beleid al is toegepast op dit niveau.

![Bereik][policies-scope]

Aangezien een al is toegepast, worden de configuratie wordt weergegeven in de weergave definitie.

![Configureren][policies-configure]

Het beleid wordt alleen-lezen weergegeven bij de eerste. Als u wilt bewerken de definitie klikt u op de actie **Beleid configureren** .

![Bewerken][policies-edit]

De definitie van het beleid is een eenvoudige XML-document waarmee een reeks binnenkomende en uitgaande instructies beschreven. De XML kan worden bewerkt rechtstreeks in het venster definiëren. Een lijst met instructies naar rechts wordt geleverd en instructies voor het huidige bereik zijn ingeschakeld en is gemarkeerd; zoals u door de instructie **Limiet bellen tarief** in de bovenstaande schermopname.

Te klikken op een verklaring omtrent het ingeschakeld, wordt de juiste XML toevoegen op de locatie van de cursor in de weergave definitie. 

>[AZURE.NOTE] Als het beleid dat u wilt toevoegen niet is ingeschakeld, moet u ervoor zorgen dat u bezig bent met het juiste bereik voor dat beleid. Elke verklaring omtrent het informatiebeheerbeleid is ontworpen voor gebruik in bepaalde bereiken en het beleid secties. Als u wilt bekijken van het beleid secties en bereiken voor een beleid, kijkt u in de sectie **Gebruik** beleid in de [Verwijzing beleid][]voor.

Een volledige lijst met beleid-instructies en de bijbehorende instellingen zijn beschikbaar in de [Verwijzing beleid][].

Bijvoorbeeld als u wilt toevoegen van een nieuwe instructie om te verzoeken voor oproepen beperken tot opgegeven IP-adressen, plaats de cursor net binnen de inhoud van de `inbound` XML-element en klik op de instructie **beperken beller IP-adressen** .

![Beleidsregels voor beperking][policies-restrict]

Kunt u hieraan een XML-fragment naar de `inbound` element dat bevat richtlijnen voor het configureren van de instructie.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Beperkingen instellen voor binnenkomende aanvragen en accepteren alleen die uit een IP-adres van 1.2.3.4 de XML-code als volgt wijzigen:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Opslaan][policies-save]

Wanneer voltooit configureren van de instructies voor het beleid, klikt u op **Opslaan** en de wijzigingen worden doorgegeven aan de API Management gateway van onmiddellijk.

##<a name="sections"> </a>Lidmaatschap beleidsconfiguratie

Een beleid is een reeks instructies die worden uitgevoerd om een nieuw vergaderverzoek en een reactie. De configuratie in correct is verdeeld `inbound`, `backend`, `outbound`, en `on-error` secties zoals wordt weergegeven in de volgende configuratie.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Als er een fout tijdens het verwerken van een aanvraag kunt invullen, eventuele overige stappen in de `inbound`, `backend`, of `outbound` secties worden overgeslagen en worden uitgevoerd springt naar de instructies in de `on-error` sectie. Door te plaatsen beleid instructies in de `on-error` sectie kunt u de fout bekijken met behulp van de `context.LastError` eigenschap, controleren en aanpassen van de fout antwoord gebruiken de `set-body` beleid, en wat gebeurt er als er een fout optreedt configureren. Zijn er foutcodes voor ingebouwde stappen en voor fouten die tijdens de verwerking van beleid-instructies optreden kunnen. Zie [foutverwerking in beleidsregels voor informatiebeheer API](https://msdn.microsoft.com/library/azure/mt629506.aspx)voor meer informatie.

Aangezien beleid kunnen worden opgegeven op verschillende niveaus (hoofdbeheerder, product, api en bewerking) kunt de configuratie u de volgorde waarin de instructies van het beleid definition met betrekking tot het beleid van de bovenliggende uitvoeren. 

Beleid bereiken worden in de volgende volgorde geëvalueerd.

1. Globaal bereik
2. Product-bereik
3. API-bereik
4. Bewerkingsbereik

De passages in ze worden geëvalueerd op basis van de positie van de `base` element, indien aanwezig.

Bijvoorbeeld als u een beleid op het niveau van de globale en een beleid is geconfigureerd voor een API hebt, klikt u vervolgens wanneer dat bepaalde API wordt gebruikt beide beleidsregels gelden. API Management kunt voor het bestellen van deterministic van gecombineerde beleid beweringen via het grondtal element. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

In het bovenstaande voorbeeld beleid definitie de `cross-domain` instructie zou worden uitgevoerd voordat een hogere beleidsregels die op zijn beurt worden gevolgd door de `find-and-replace` beleid.

Als u hetzelfde beleid tweemaal in de verklaring omtrent het informatiebeheerbeleid wordt weergegeven, is het meest recent geëvalueerde beleid wordt toegepast. U kunt dit gebruiken om te negeren beleidsregels die zijn gedefinieerd op een hoger bereik. Als u wilt zien van het beleid in het huidige bereik in de editor, klikt u op **herberekenen effectief beleid voor het geselecteerde bereik**.

Houd er rekening mee dat globaal beleid geen bovenliggende beleid heeft en het gebruik van de `<base>` element erin heeft geen effect. 

## <a name="next-steps"></a>Volgende stappen

Bekijk de video op beleid expressies te volgen.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Beleidsverwijzing]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Bewerking]: api-management-howto-add-operations.md

[Geavanceerde beleidsregels]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Besturing]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variabele]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Beleid expressies]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
