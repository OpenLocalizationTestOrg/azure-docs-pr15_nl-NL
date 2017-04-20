<properties
 pageTitle="Toepassingssjablonen logica | Microsoft Azure"
 description="Informatie over het gebruik van vooraf gemaakte logica app-sjablonen om u aan de slag te helpen"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>Logica App-sjablonen

## <a name="what-are-logic-app-templates"></a>Wat zijn de logica app-sjablonen

Een sjabloon voor een app logica is een vooraf gedefinieerde logica-app die u kunt snel aan de slag maken van uw eigen werkstroom. 

Deze sjablonen voor zijn een goede manier om te ontdekken verschillende patronen die kunnen worden gemaakt met behulp van logica apps. U kunt deze sjablonen als gebruiken-is of aanpassen aan uw scenario.

## <a name="overview-of-available-templates"></a>Overzicht van beschikbare sjablonen

Zijn er veel beschikbare sjablonen die momenteel zijn gepubliceerd in de logica app-platform. Sommige voorbeeld-categorieën, alsmede het type verbindingslijnen gebruikt in deze hieronder.

### <a name="enterprise-cloud-templates"></a>Ondernemingssjablonen cloud
Sjablonen die kunnen worden geïntegreerd Dynamics CRM, Salesforce vak, Azure Blob en andere connectors voor de behoeften van uw onderneming cloud. Enkele voorbeelden van wat u met deze doen kunt sjablonen bevatten ordenen van potentiële klanten en een back-up van de gegevens van uw bedrijf bestand.

### <a name="enterprise-integration-pack-templates"></a>Ondernemingssjablonen integratie pack
Configuraties van VETER (valideren, extraheren, transformeren, verrijken, routeren) gasbuizen, ontvangen een X12 bewerken document over AS2 en deze naar XML, transformeren als goed als X12 en AS2 bericht verwerkingstijd.

### <a name="protocol-pattern-templates"></a>Protocol patroon sjablonen
Deze sjablonen bestaan uit de logica-apps met protocol patronen zoals aanvraag en respons via HTTP, evenals integraties over FTP- en SFTP. Gebruik deze die zijn, of als basis voor het maken van complexere protocol patronen.  

### <a name="personal-productivity-templates"></a>Persoonlijke productiviteit sjablonen
Patronen om u te helpen verbeteren persoonlijke productiviteit bevatten sjablonen die dagelijkse herinneringen instellen, belangrijke werkitems omzetten in takenlijsten en langdurige taken naar beneden af op één gebruiker goedkeuringsstap automatiseren.

### <a name="consumer-cloud-templates"></a>Sjablonen voor consumenten cloud
Eenvoudige sjablonen die zijn geïntegreerd met sociale mediaservices zoals Twitter, toegestane vertraging en e-mail, sociale media marketing initiatieven versterking uiteindelijk staat. Hierbij wordt ook sjablonen zoals bewolkt kopiëren, die productiviteit verbeteren door op te slaan tijd die is besteed aan traditioneel terugkerende taken. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Het maken van een logica-app met een sjabloon 

Ga in de ontwerper van de app logica om te beginnen met een logica app-sjabloon. Als u de ontwerpfunctie door te openen van een bestaande logica app invoert, wordt de app logica automatisch in uw weergave met de ontwerpfunctie geladen. Als u een nieuwe logica-app maakt, ziet u echter het volgende scherm.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

In dit scherm kunt u op kiezen om te beginnen met een lege logica-app of een vooraf gedefinieerde sjabloon. Als u een van de sjablonen selecteert, beschikt u aanvullende informatie. In dit voorbeeld gebruiken we de sjabloon *wanneer een nieuw bestand is gemaakt in Dropbox, kopieer deze naar OneDrive* .  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Als u besluit om de sjabloon gebruiken, selecteert u alleen de knop *Gebruik deze sjabloon* . U wordt gevraagd te melden bij uw accounts die is gebaseerd op welke verbindingslijnen gebruikmaakt van de sjabloon. Of, als u een verbinding met deze verbindingslijnen eerder hebt ingesteld, kunt u doorgaan met zoals hieronder wordt weergegeven.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Nadat de verbinding tot stand brengen en *gaat u verder*te selecteren, is de logica-app wordt geopend in de weergave met de ontwerpfunctie.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

In het bovenstaande voorbeeld, zoals het geval met veel sjablonen kan sommige velden verplichte eigenschap worden ingevuld binnen de verbindingslijnen; echter enkele nog steeds mogelijk een waarde kan pas correct implementeren de logica-app. Als u probeert te implementeren zonder in te voeren enkele van de ontbrekende velden, wordt u geïnformeerd met een foutbericht wordt weergegeven.

Als u terugkeren naar de sjabloon-viewer wilt, selecteert u de knop *sjablonen* in de bovenste navigatiebalk. Door over te schakelen terug naar de sjabloon-viewer, moet u de voortgang van een niet-opgeslagen verliezen. Voordat u weer aan de sjabloon viewer, ziet u een waarschuwing ziet u hiervan op de hoogte te stellen.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Het implementeren van een logica app gemaakt op basis van een sjabloon

Zodra u hebt geladen van uw sjabloon en de gewenste wijzigingen hebt aangebracht, schakelt u het opslaan van de knop in de linkerbovenhoek. Hiermee worden opgeslagen en uw app logica publiceert.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Als u meer informatie over bewerken hoe u aanvullende stappen in een bestaand app-sjabloon van logica toevoegen of het maken van in het algemeen, lees meer bij het [maken van een app logica](app-service-logic-create-a-logic-app.md)