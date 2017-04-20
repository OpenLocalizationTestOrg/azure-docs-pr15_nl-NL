<properties 
   pageTitle="Toegang krijgen tot privé Azure wolken met Visual Studio | Microsoft Azure"
   description="Leer hoe u toegang tot privé cloud resources met behulp van Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Toegang krijgen tot privé Azure wolken met Visual Studio

##<a name="overview"></a>Overzicht

Standaard ondersteunt Visual Studio openbare Azure cloud REST eindpunten. Dit is een probleem, maar als u Visual Studio met een privé Azure cloud gebruikt. U kunt certificaten gebruiken voor het configureren van Visual Studio voor toegang tot privé Azure cloud REST eindpunten. U kunt deze ophalen certificaten via uw Azure instellingenbestand publiceren.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Voor toegang tot een persoonlijke Azure cloud in Visual Studio

1. Klik in de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885) voor de privé cloud downloaden van uw instellingenbestand publiceren of contact op met uw beheerder voor een bestand publiceren-instellingen. Klik op de openbare versie van Azure is de koppeling om te downloaden dit [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (U moet een extensie .publishsettings hebben voor het bestand dat u hebt gedownload.)

1. In **Server Explorer** in Visual Studio, kiest u het knooppunt **Azure** en kies op het snelmenu de opdracht **Abonnementen beheren** .

    ![De opdracht abonnementen beheren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. Kies het tabblad **certificaten** in het dialoogvenster **Microsoft Azure-abonnementen beheren** en kies vervolgens de knop **importeren** .

    ![Azure certificaten importeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. Blader naar de map waar u het publiceren instellingen-bestand hebt opgeslagen en kies het bestand en kies vervolgens de knop **importeren** in het dialoogvenster **Importeren Microsoft Azure-abonnementen** . Hiermee wordt de certificaten in het instellingenbestand publiceren geïmporteerd in Visual Studio. U moet nu mogelijk om te communiceren met uw privé cloud-resources.

    ![Publicatie-instellingen importeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Volgende stappen

[Publiceren naar een Azure Cloudservice van Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Hoe u: downloaden en importeren publiceren instellingen en informatie over abonnement](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)

