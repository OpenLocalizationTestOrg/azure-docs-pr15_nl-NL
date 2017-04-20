<properties
    pageTitle="Problemen met: Maken en verbinding maken met een Machine Learning-werkruimte | Microsoft Azure"
    description="Oplossingen voor veelvoorkomende problemen bij het maken en verbinding maken met een Azure Machine Learning-werkruimte"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Gids voor probleemoplossing: maken en verbinding maken met een Machine Learning-werkruimte

Deze handleiding bevat oplossingen voor enkele vaak uitdagingen heeft voorgedaan wanneer u deze Azure Machine Learning-werkruimten instelt.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>De eigenaar van de werkruimte

Wanneer u een nieuwe Machine Learning-werkruimte maakt, de ID die u in het veld eigenaar van de WERKRUIMTE invoert moet een geldige Microsoft-account (voorheen Windows Live ID), bijvoorbeeld john-contoso@live.com of john-contoso@hotmail.com. Dit kan een niet-Microsoft-account, zoals uw zakelijke e-mailaccount niet. Als u wilt een gratis Microsoft-account hebt gemaakt, gaat u naar [www.live.com](http://www.live.com).

Houd er rekening mee dat het account waarmee u aanmelden bij de portal Azure bij het maken van de werkruimte is niet automatisch gemachtigd om te *openen* die werkruimte, tenzij u dat account als eigenaar opgeeft. U opent een werkruimte in Machine Learning Studio door u moet zijn aangemeld bij de Microsoft-Account die is gedefinieerd als de eigenaar van de werkruimte, anders moet u een uitnodiging hebben ontvangen van de eigenaar van de deel te nemen aan de werkruimte. Van de Azure klassieke portal kunt u echter *beheren* van de werkruimte, waaronder de mogelijkheid om te wijzigen van de eigenaar en configureren van access.

Zie voor meer informatie over het beheren van een werkruimte [beheren een Azure Machine Learning-werkruimte].

[Een werkruimte Azure Machine Learning beheren]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Toegestane regio 's

Machine Learning is momenteel beschikbaar in een beperkt aantal regio's. Als uw abonnement niet een van deze gebieden bevat, ziet u mogelijk het foutbericht wordt weergegeven, "U hebt geen abonnementen in de toegestane gebieden."

Als u wilt dat een gebied worden toegevoegd aan uw abonnement, selecteer **Contact opnemen met Microsoft ondersteuning** in de klassieke Azure-Portal, kiest u **Facturering** als het probleemtype en volg de aanwijzingen voor het indienen van uw aanvraag kunt invullen.

![Contact opnemen met Microsoft ondersteuning][screen1]

## <a name="storage-account"></a>Opslag-account

Er moet een opslag-account voor de opslag van gegevens voor de Machine Learning-service. U kunt een bestaand opslag-account of u kunt een nieuw account voor de opslag maken wanneer u de nieuwe Machine Learning-werkruimte maken (als u quotum voor een nieuwe opslag-account maken hebt).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Werkruimte maken][screen2]

Nadat het nieuwe Machine Learning-werkruimte is gemaakt, kunt u zich aanmeldt bij Machine Learning Studio met behulp van het Microsoft-account dat u hebt opgegeven als de eigenaar van de werkruimte. Als u het foutbericht "Werkruimte niet gevonden" (vergelijkbaar met de volgende schermafbeelding), gebruikt u de volgende stappen uit om te verwijderen van cookies in de browser.

![Werkruimte niet gevonden][screen3]

**Browsercookies verwijderen**

Als u Internet Explorer gebruikt, klikt u op de knop **hulpmiddelen** in de rechterbovenhoek en selecteer **Internetopties**.  

![Internet-opties][screen4]

Klik onder het tabblad **Algemeen** op **... verwijderen**

![Tabblad Algemeen][screen5]

Zorg ervoor dat **Cookies en websitegegevens** is geselecteerd in het dialoogvenster **Browsegeschiedenis verwijderen** en klik op **verwijderen**.

![Verwijderen van cookies][screen6]

Nadat de cookies zijn verwijderd, start de browser opnieuw en ga vervolgens naar de [Microsoft Azure Machine Learning](https://studio.azureml.net) -pagina. Wanneer u wordt gevraagd om een gebruikersnaam en wachtwoord, typt u hetzelfde Microsoft-account die u hebt opgegeven als de eigenaar van de werkruimte.

Ons doel is om de Machine Learning-ervaring zo weinig mogelijk te voeren. Boek eventuele opmerkingen en problemen bij het [Azure Machine Learning-forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) om ons u beter van dienst te helpen.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
