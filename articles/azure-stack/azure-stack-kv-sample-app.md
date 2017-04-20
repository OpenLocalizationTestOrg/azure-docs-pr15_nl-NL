<properties
    pageTitle="Toepassing laten revtrieve Azure stapel toets kluis geheimen | Microsoft Azure"
    description="Een voorbeeld-app gebruiken om te werken met Azure stapel toets kluis"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="run-the-sample-application-for-key-vault"></a>Voer de voorbeeldtoepassing voor sleutel kluis 

In deze handleiding gebruikt u een toepassing voor de steekproef geheimen en wachtwoorden van toets kluis kunnen ophalen.

## <a name="download-the-samples-and-prepare"></a>Download de voorbeelden en voorbereiden

Download de voorbeelden van de client Azure-toets kluis vanaf de [pagina voor voorbeelden van Azure toets kluis client](https://www.microsoft.com/en-us/download/details.aspx?id=45343).

Pak de inhoud van het ZIP-bestand met uw lokale computer.

Lees het **README.md** -bestand (dit is een tekstbestand) en volg de instructies.

## <a name="run-sample-1--hellokeyvault"></a>Voorbeeld #1--HelloKeyVault uitvoeren
HelloKeyVault is een consoletoepassing die u begeleidt bij belangrijke scenario's die worden ondersteund door de sleutel kluis:

  1. Een toets (HSM of software) maken/importeren
  2. Versleutelen met een inhoud sleutel geheim
  3. Laten teruglopen van de inhoud sleutel door een toets kluis te gebruiken
  4. Pak de inhoud-toets
  5. Het geheim ontsleutelen
  6. Stel een geheim

Die consoletoepassing moet zonder wijzigingen, uitvoeren, behalve dat de juiste configuratie-instellingen in App.Config wordt bijgewerkt op basis van de volgende stappen uit:

1. Werk de instellingen voor de app-configuratie in HelloKeyVault\App.config met uw kluis URL, principal toepassings-ID en geheim. De gegevens kan desgewenst worden gegenereerd met **scripts\GetAppConfigSettings.ps1**.
2. De waarden van de verplicht variabelen in GetAppConfigSettings.ps1 bijwerken.
3. Start de Windows PowerShell-venster.
4. Voer het script GetAppConfigSettings.ps1 binnen de PowerShell-venster.
5. De resultaten van het script naar het bestand HelloKeyVault\App.config kopiëren.


## <a name="next-steps"></a>Volgende stappen

[Een VM met een wachtwoord toets kluis implementeren](azure-stack-kv-deploy-vm-with-secret.md)

[Een VM met een certificaat toets kluis implementeren](azure-stack-kv-push-secret-into-vm.md)