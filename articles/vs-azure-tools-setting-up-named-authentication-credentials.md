<properties
   pageTitle="Instellen met de naam verificatiereferenties | Microsoft Azure"
   description="Leer hoe naar referenties op te geven die Visual Studio kunt gebruiken om te verifiëren aanvragen voor Azure publiceren van een toepassing naar Azure Visual Studio of om een bestaande cloudservice te controleren... "
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

# <a name="setting-up-named-authentication-credentials"></a>Benoemde verificatiereferenties instellen

Om een bestaande cloudservice te controleren of een toepassing naar Azure Visual Studio publiceren, moet u de referenties die Visual Studio gebruiken kunt om te verifiëren aanvragen voor Azure opgeven. Er zijn verschillende plaatsen in Visual Studio, waar u zich aanmelden kunt bij deze referenties opgeven. U kunt vanuit de Explorer Server bijvoorbeeld het snelmenu voor het knooppunt **Azure** openen en kies **verbinding maken met Azure**. Wanneer u zich aanmeldt, informatie over het abonnement dat is gekoppeld aan uw Azure-account is beschikbaar in Visual Studio en niets meer hoeft te doen.

Azure extra ondersteunt ook een oudere manier aan te bieden referenties, met behulp van het abonnementsbestand (.publishsettings-bestand). In dit onderwerp worden deze methode, die nog steeds wordt ondersteund in Azure SDK 2.2.

De volgende items zijn vereist voor de verificatie van Azure.

- Uw abonnements-ID

- Een geldige X.509 v3-certificaat

>[AZURE.NOTE] De lengte van de X.509 v3-certificaat sleutel moet ten minste 2048 bits. Azure weigert een certificaat dat niet voldoet aan deze vereiste of die is niet geldig.

Visual Studio gebruikt uw abonnements-ID samen met de certificaatgegevens als referenties. De juiste referenties wordt verwezen in het abonnementsbestand (.publishsettings-bestand), waarin een openbare sleutel voor het certificaat. Het abonnementsbestand kan referenties voor meer dan één abonnement bevatten.

U kunt de abonnementsgegevens in het dialoogvenster **Nieuw/Edit abonnement** kunt bewerken, zoals verderop in dit onderwerp.

Als u een digitaal certificaat maken uzelf wilt, kunt u verwijzen naar de instructies in [maken en uploaden van een certificaat Management voor Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) en het certificaat handmatig bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)te uploaden.

>[AZURE.NOTE] Deze referenties die Visual Studio is vereist voor het beheren van uw cloudservices zijn niet dezelfde referenties die zijn vereist voor de verificatie van een nieuw vergaderverzoek ten opzichte van de Azure storage-services.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Wijzigen of verificatiereferenties in Visual Studio exporteren

U kunt ook instellen, wijzigen of exporteren van uw verificatiereferenties in het dialoogvenster **Nieuw abonnement** , die wordt weergegeven als u een van de volgende handelingen uitvoeren:

- In **Server Explorer**, opent u het snelmenu voor het knooppunt **Azure** , kiest u **Abonnementen beheren**, kies het tabblad **certificaten** en klikt u op de knop **Nieuw** of **bewerken** .

- Wanneer u een Azure cloudservice van de wizard **Publiceren Azure-toepassing** publiceert, **beheren** in de lijst **Kies uw abonnement** te kiezen en kies het tabblad certificaten en kies vervolgens de knop **Nieuw** of **bewerken** .

De volgende procedure wordt ervan uitgegaan dat het dialoogvenster **Nieuw abonnement** geopend is.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Voor het instellen van verificatiereferenties in Visual Studio

1. Kies in het **selecteren van een bestaand certificaat** voor verificatielijst, een certificaat.

1. Kies de knop **kopiëren van het volledige pad** . Het pad voor het certificaat (.cer-bestand) wordt gekopieerd naar het Klembord.

    >[AZURE.IMPORTANT] Als u wilt publiceren de Azure toepassing Visual Studio, moet u dit certificaat uploaden naar de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Het certificaat uploaden naar de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885):

    1. Kies de koppeling Azure-Portal.

         De [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885) wordt geopend.

    1. Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)en kies vervolgens de knop **Cloudservices** .

    1. Kies de cloudservice die u interesseert.

        De pagina voor de service wordt geopend.

    1. Kies de knop **uploaden** op het tabblad **certificaten** .

    1. Plak het volledige pad van het .cer-bestand dat u zojuist hebt gemaakt en voer het wachtwoord dat u hebt opgegeven.
