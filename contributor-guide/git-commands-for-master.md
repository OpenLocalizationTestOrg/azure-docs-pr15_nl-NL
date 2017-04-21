<properties pageTitle="Cijfer opdrachten voor het maken van een nieuw artikel of een bestaand artikel bijwerken" description="Stappen voor het werken met de technische Azure inhoud GitHub opslagplaatsen." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Cijfer opdrachten voor het maken van een nieuw artikel of een bestaand artikel bijwerken


## <a name="standard-process-working-from-master"></a>Standaard proces (werken van model)
Volg de stappen in dit artikel voor een lokale werken tak maakt op uw computer, zodat u kunt een nieuw artikel voor de sectie technische documentatie van azure.microsoft.com maken of bijwerken van een bestaand artikel.

1. Start cijfer we vaker doen (of het hulpprogramma voor dat u voor cijfer gebruikt).

 **Notitie:** Als u in de openbare opslagplaats werkt, wijzigen in azure-inhoud-prijs azure-inhoud in alle opdrachten.

2. Azure-inhoud-prijs wijzigen:

        cd azure-content-pr
3. Bekijk de basispagina tak:

        git checkout master

4. Maak een nieuwe lokale werken tak afgeleid van de basispagina tak:

        git pull upstream master:<working branch>


5. Verplaatsen naar de nieuwe werken tak:

        git checkout <working branch>

6. Laat uw zich splitsen die u hebt gemaakt met de lokale werken tak weten:

        git push origin <working branch>

7. Uw nieuwe artikel maken of wijzigingen aanbrengen in een bestaand artikel. Gebruik Windows Verkenner te openen en nieuwe korting-bestanden maken en gebruiken van Atom (http://atom.io) als de korting-editor. Nadat u hebt gemaakt of gewijzigd uw artikel en afbeeldingen, gaat u naar de volgende stap.

8. Toevoegen en de aangebrachte wijzigingen doorvoeren:

        git add .
        git commit –m "<comment>"
        
   Of om toe te voegen alleen de specifieke bestanden u gewijzigd:

        git add <file path>
        git commit –m "<comment>"

   Als u bestanden hebt verwijderd, hebt u gebruik deze opdracht:
   
        git add --all
        git commit -m "<comment>"

9. Uw lokale werken tak bijwerken met wijzigingen in boven:

        git pull upstream master

10. De wijzigingen aan uw zich splitsen push op GitHub:

        git push origin <working branch>

12. Wanneer u klaar bent voor het indienen van uw inhoud naar de boven-basispagina vertakking voor tijdelijke, validatie, en/of publiceren, in de UI GitHub maken een verzoek om te halen uit uw zich splitsen aan de basispagina tak.

13. Als u een werknemer bent werken in de persoonlijke bibliotheek, de foutenrapporten wijzigingen worden automatisch gefaseerde en een tijdelijk opslaan koppeling naar de aanvraag halen is geschreven. Controleer uw gefaseerde inhoud en van de opmerkingen van de aanvraag halen door toe te voegen van de opmerking **sign uit #** afmelden.  Hier worden de wijzigingen die moet worden geplaatst live klaar zijn aangegeven.  Als u niet dat het verzoek halen worden geaccepteerd - wilt als u alleen de wijzigingen - zijn tijdelijke moet u de notitie **hold ertoe #** toevoegen aan het verzoek halen.

14. De halen verzoek acceptor beoordeelt uw aanvraag halen, vindt u feedback en/of uw aanvraag halen accepteert. 

15. (Optioneel) Controleer uw gepubliceerde artikel of de wijzigingen op

 *name-of-your-article-without-the-MD-extension* http://Azure.Microsoft.com/Documentation/articles/

## <a name="publishing"></a>Publiceren

- Artikelen zijn gepubliceerd op ongeveer 10:00 AM en 3:00 PM Pacific Time, maandag-vrijdag. Dit kan maximaal 30 minuten duren voordat artikelen online weergegeven na de publicatie. Onthoud dat uw aanvraag halen heeft moeten worden samengevoegd van een revisor van de aanvraag halen voordat de wijzigingen kunnen worden opgenomen in de volgende geplande publicatie uitvoeren. U moet werken met uw aanvraag halen revisor tijd vooraf om ervoor te zorgen dat een verzoek om te halen is samengevoegd voor een specifieke publicatie uitvoeren. Anders worden PRs beoordeeld in de volgorde waarin die ze zijn ingediend.

- Als u een werknemer werken in de persoonlijke bibliotheek, worden alle halen aanvragen zijn onderhevig aan regels voor gegevensvalidatie die worden verholpen moeten voordat het verzoek halen kan worden samengevoegd. 

## <a name="working-with-release-branches"></a>Werken met release vertakkingen

Wanneer u met een release-vertakking werkt, is de beste manier om een lokale werken tak maken van het release-tak deze opdrachtsyntaxis van de gebruiken:

    git checkout upstream/<upstream branch name> -b <local working branch name>

Hiermee maakt u de lokale tak rechtstreeks vanuit het boven tak, voorkomen met een lokale samenvoegen.

