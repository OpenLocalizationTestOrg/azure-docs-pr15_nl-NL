<properties 
   pageTitle="Gebruikersinterface van Azure mobiele betrokkenheid - bereik criterium" 
   description="Informatie over het gebruik van criteria gerichte push campagnes verzenden naar een specifieke subset van de gebruikers met Azure Mobile betrokkenheid" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede"
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Het gebruik van criteria gerichte push campagnes verzenden naar een specifieke subset van uw gebruikers

Hebt u uw doelgroep samengesteld op specifieke criteria met de knop "Nieuwe Criteria" is een van de krachtigste concepten in Azure Mobile betrokkenheid dat helpt u relevante verzendt push-meldingen dat de klanten reageren zullen op in plaats van spam iedereen. Als u beperkingen instellen voor uw publiek op basis van standaard criteria en simuleren gereedschap duwt om te bepalen hoeveel mensen de melding ontvangt.

**Zie ook:**

- [UI - bereik - documentatie nieuwe Push campagne][Link 27]

## <a name="audience-criteria-can-include"></a>Publiek criteria kunnen opnemen:
- **Technicals:** U kunt afstemmen op basis van dezelfde technische informatie u in de secties analyses en Monitor zien kunt. **Zie ook:** [UI-documentatie - analyses] [ Link 15], [UI-documentatie - beeldscherm][Link 16]
- **Locatie:** Toepassingen die gebruikmaken van "realtime locatie rapportage" met geografische hekwerk kunnen geografische locatie gebruiken als criterium een doelgroep van de locatie GPS afstemmen. "Fikse gebied locatie Reporting" oproep ook worden gebruikt voor het afstemmen van een doelgroep van de locatie van de mobiele telefoon ("realtime locatie rapportage" en 'Fikse gebied locatie rapporteren' moet worden geactiveerd uit de SDK). **Zie ook:** [SDK-documentatie - iOS - integratie] [ Link 5], [SDK documentatie - Android - integratie][Link 5]
- **Feedback hebt bereikt:** U kunt uw publiek op basis van hun feedback uit vorige bereik meldingen via bereik feedback uit aankondigingen, Polls en de gegevens worden afstemmen. Hiermee kunt u naar betere doelsite uw publiek na twee of drie campagnes dan mogelijk is de eerste keer hebt bereikt. Dit kan ook worden gebruikt uitfilteren gebruikers die al een melding met soortgelijke inhoud, ontvangen door in te stellen van een campagne niet worden verzonden naar gebruikers met een specifieke vorige campagne al hebt ontvangen. U kunt zelfs voorkomen dat gebruikers een specifieke campagne die nog actief is ontvangen van nieuwe gereedschap duwt die zijn opgenomen. **Zie ook:** [Documentatie UI - hebt bereikt - Push-inhoud][Link 29]
- **Bijhouden installeren:** Informatie op basis van waarop uw gebruikers de App geïnstalleerd, kunt u vinden. **Zie ook:** [UI-documentatie - instellingen][Link 20]
- **Gebruikersprofiel:** U kunt afstemmen op basis van standaard gebruikersgegevens en u kunt doel op basis van de aangepaste app-informatie die u hebt gemaakt. Dit geldt ook voor gebruikers die zich hebt aangemeld en gebruikers die specifieke vragen die u hebt aangegeven dat ze instellen in de app zelf in plaats van alleen hoe ze hebben gereageerd op vorige campagnes beantwoord. Alle van uw App Info gedefinieerd voor uw app weergegeven in deze lijst voorkomt.
- Segmenten: U kunt ook het doeladres op basis van segmenten die u hebt gemaakt op basis van bepaalde gebruikersgedrag met meerdere criteria. Al uw segmenten die is gedefinieerd voor de app weergegeven in deze lijst voorkomt. **Zie ook:** [UI-documentatie - segmenten][Link 18]
- **App Info:** "-Instellingen voor het bijhouden van gebruikersgedrag kan aangepaste App Info codes worden gemaakt. **Zie ook:** [UI-documentatie - instellingen][Link 20]

## <a name="example"></a>Voorbeeld: 
Als u een aankondiging push alleen voor de subsite set van de gebruikers die een actie app aankoop hebt uitgevoerd wilt.

1. Ga naar de toepassing instellingen-pagina, selecteer het menu 'App info' en 'Nieuwe app info' selecteren
2. Een nieuwe Booleaanse app info 'inAppPurchase' genoemd registreren
3. Controleer uw toepassing instellen van deze app info op "true" wanneer de gebruiker is een aankoop app uitvoert (met behulp van de sendAppInfo ("inAppPurchase",...) functie)
4. Als u dit doen vanuit uw toepassing niet wilt, kunt u dit doen van uw end met behulp van het apparaat API)
5. Vervolgens hoeft u te maken van uw bericht, met een criterium beperken uw publiek naar gebruikers met 'inAppPurchase' is ingesteld op "true")
 
> Opmerking: Hebt samengesteld op basis van criteria dan app info tags Azure Mobile betrokkenheid om informatie te verzamelen vanaf uw gebruikers apparaten voordat de push wordt verzonden en dus kunnen leiden tot een vertraging vereist. Complexe push-configuratie opties (zoals bijwerken badges) kunnen ook uitstellen verplaatst. Via een campagne "één keer" van de Push-API, is de absolute snelste pushmethode in Azure Mobile betrokkenheid. Alleen de labels van app-info als push-criterium voor een bereik campagne (via de API hebt bereikt of de gebruikersinterface) gebruiken is de volgende snelste methode omdat app info tags zijn opgeslagen op de server. Gebruik van andere doelgroepen criteria voor een push-campagne, is dat de meest flexibele maar laagst mogelijke pushmethode aangezien Azure Mobile betrokkenheid met om query's in de apparaten wilt sturen van de campagne.
 
![Bereik-Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Criterium opties zijn van toepassing op:
- **Technicals**     
- De naam van de firmware: de naam van de Firmware
- Firmwareversie: firmwareversie
- Apparaatmodel: Apparaatmodel
- De apparaatfabrikant van het: de fabrikant van apparaat
- Versie van toepassing: versie van toepassing
- De naam van de Carrier: Carrier de naam niet gedefinieerd
- Carrier land: Carrier land, niet gedefinieerd
- Type netwerk: netwerk type
- Landinstelling: landinstelling
- Grootte van het scherm: grootte van het scherm
- **Locatie**      
- Laatste bekende gebied: land, regio, plaats
- Realtime geografische-fencing: lijst van en nuttige locaties (naam, acties), ronde pijl (naam, breedtegraad, lengtegegevens, straal in meter)
- **Feedback hebt bereikt**     
- Aankondiging van feedback: aankondiging, feedback
- Poll uitvoeren onder feedback: peiling, feedback
- Poll uitvoeren onder antwoord feedback: poll uitvoeren onder antwoord feedback, vraag, keuze
- Gegevens Push feedback: gegevens Push, feedback
- **Bijhouden installeren**     
- Store: Opslaan, niet gedefinieerd
- Bron: Bron, niet gedefinieerd
- **Gebruikersprofiel**     
- Geslacht: man of vrouw, niet gedefinieerd
- Datum die geboren: operator, datum, niet gedefinieerd
- Aanmelden: waar of ONWAAR, ongedefinieerd
- **App Info**      
- Tekenreeks: Tekenreeks niet gedefinieerd
- Datum: de operator, datum, niet gedefinieerd
- Geheel getal: de operator, getal, niet gedefinieerd
- Booleaanse waarde: waar of ONWAAR geven, niet gedefinieerd
- **Segment**    
- Naam van segmenten (in de vervolgkeuzelijst), uitsluiting (Doelgebruikers die geen deel uitmaken van dit segment).

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
