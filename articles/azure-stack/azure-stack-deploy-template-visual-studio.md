<properties
    pageTitle="Sjablonen met Visual Studio Azure gestapelde implementeren | Microsoft Azure"
    description="Leer hoe u sjablonen met Visual Studio Azure gestapelde implementeren."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Sjablonen in Azure stapel en gebruik Visual Studio implementeren

Gebruik Visual Studio om Azure resourcemanager sjablonen implementeren naar de Azure stapel Haalbaarheidstest.

Resourcemanager sjablonen implementeren en inrichten van alle bronnen voor uw toepassing een eenmalige, gecoördineerde betrekking heeft.

1.  Open Visual Studio 2015 Update 1.

2.  Klik op **bestand**, klikt u op **Nieuw**en klik in het dialoogvenster **Nieuw Project** op **Azure resourcegroep**.

3.  Voer een **naam** voor het nieuwe project en klik vervolgens op **OK**.

4.  In het dialoogvenster **Azure-sjabloon selecteren** , klikt u op **virtuele Windows-computer**en klik vervolgens op **OK**.

  In het nieuwe project ziet u een lijst met de sjablonen die beschikbaar door uit te vouwen het knooppunt **sjablonen** in het deelvenster **Oplossingverkenner** .

5.  Klik in het deelvenster **Solution Explorer** met de rechtermuisknop op de naam van uw project, klik op **Deploy**, klik op **Nieuwe implementatie**.

6.  Selecteer in het dialoogvenster **Deploy aan resourcegroep** , in de vervolgkeuzelijst **abonnement** uw abonnement op Microsoft Azure stapel.

7.  Klik in de lijst **Resourcegroep** kiest u een bestaande resourcegroep of een nieuw account te maken.

8.  Kies een locatie in de lijst **Resource groep locatie** en klik vervolgens op **Deploy**.

9.  Klik in het dialoogvenster **Parameters bewerken** Voer waarden in voor de parameters (die is afhankelijk van sjabloon) en klik vervolgens op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

[Sjablonen met de opdrachtregel implementeren](azure-stack-deploy-template-command-line.md)
