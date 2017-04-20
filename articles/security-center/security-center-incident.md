<properties
   pageTitle="Verwerking van beveiligingsincident in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document helpt u bij de mogelijkheden van de Azure Beveiligingscentrum worden afgehandeld-incidenten gebruiken."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="handling-security-incident-in-azure-security-center"></a>Verwerking van beveiligingsincident in Azure Beveiligingscentrum 
Gesorteerd en wordt onderzocht beveiligingsmeldingen kunnen enige tijd duren voor zelfs meest ervaren beveiliging analisten en voor veel is dit niet weet waar u moet beginnen. Met [analytics](security-center-detection-capabilities.md) verbinding maken met de gegevens tussen distinct [beveiligingsmeldingen](security-center-managing-and-responding-alerts.md), Beveiligingscentrum u kunt voorzien van één weergave van een campagne aanval en alle gerelateerde signalen: u kunt snel begrijpen welke acties de hacker hebt gemaakt, en welke resources zijn veranderd.

In dit document wordt beschreven hoe gebruikt u de waarschuwing mogelijkheid beveiliging in Beveiligingscentrum om u te helpen u-incidenten afhandelen.


## <a name="what-is-a-security-incident"></a>Wat is een beveiligingsincident?

In Beveiligingscentrum is een beveiligingsincident een samenvoeging van alle waarschuwingen voor een resource die met patronen [ketting beëindigen overeenkomen](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Incidenten worden weergegeven in de tegel [Beveiligingsmeldingen](security-center-managing-and-responding-alerts.md) en blade. Een Incident, zullen de lijst met verwante waarschuwingen, waarmee u meer informatie over elk exemplaar te verkrijgen.

## <a name="managing-security-incidents"></a>-Incidenten beheren

U kunt uw huidige-incidenten controleren door te kijken op de tegel van beveiliging waarschuwingen. Toegang tot de Portal Azure en volg de onderstaande stappen om meer details over elke beveiligingsincident:

1. Klik op het dashboard Beveiligingscentrum ziet u de tegel **beveiligingsmeldingen** .

    ![Beveiligingsmeldingen-tegel in Beveiligingscentrum](./media/security-center-incident/security-center-incident-fig1.png)

2.  Klik op deze tegel om uit te vouwen en als een beveiligingsincident wordt aangetroffen, wordt deze wordt weergegeven onder de beveiliging waarschuwingen graph zoals hieronder wordt weergegeven:

    ![Beveiligingsincident](./media/security-center-incident/security-center-incident-fig2.png)

3.  U ziet dat de beschrijving van de beveiliging incident een ander pictogram vergeleken met andere meldingen. Klik op meer informatie over deze incident weergeven.

    ![Beveiligingsincident](./media/security-center-incident/security-center-incident-fig3.png)

4.  Klik op het **incident** blade u meer ziet die details over deze beveiligingsincident, inclusief de volledige beschrijving, de ernst (die in dit geval is hoog), de huidige status (in dit geval is nog steeds *actieve*, wat betekent dat de gebruiker is niet een actie uitgevoerd om te *sluiten* deze - dit kunt doen met de rechtermuisknop te klikken op het incident in het blad **beveiligingsmeldingen** ) , de aangevallen resource (in dit geval *VM1*), de remediation stappen voor het incident en klik in het deelvenster onder die u hebt de meldingen die zijn opgenomen in deze incident. Als u meer informatie over elke waarschuwing ophalen wilt, wordt Klik op deze en een andere blade geopend, zoals hieronder wordt weergegeven:

    ![Beveiligingsincident](./media/security-center-incident/security-center-incident-fig4.png)

De gegevens op deze blade zijn afhankelijk van de melding. Lees [beheren en beveiligingswaarschuwingen in Azure Beveiligingscentrum beantwoorden](security-center-managing-and-responding-alerts.md) voor meer informatie over hoe u deze waarschuwingen beheren. Enkele belangrijke punten met betrekking tot deze mogelijkheid:

- Een nieuw filter kunt u de weergave alleen Incident, alleen waarschuwingen of beide kunt aanpassen. 
- De melding voor een dezelfde kan bestaan als onderdeel van een Incident (indien beschikbaar), en alleen zichtbaar als een melding voor een zelfstandig product. 
- Sluiten van een incident, wordt de gerelateerde waarschuwingen niet sluiten.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe gebruikt u de mogelijkheid voor het incident van beveiliging in Beveiligingscentrum. Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md)
- [Mogelijkheden voor detectie van Azure Beveiligingscentrum](security-center-detection-capabilities.md)
- [Azure Beveiligingscentrum plannen en Operations Guide](security-center-planning-and-operations-guide.md)
- [Beheren en beantwoorden van beveiligingsmeldingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md)
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md)--zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/)--zoeken blog over Azure beveiliging en naleving in berichten.
