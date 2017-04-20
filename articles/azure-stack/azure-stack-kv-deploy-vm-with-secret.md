<properties
    pageTitle="Een VM met een wachtwoord die zijn opgeslagen in Azure stapel toets kluis implementeren | Microsoft Azure"
    description="Informatie over het implementeren van een VM met een wachtwoord die zijn opgeslagen in Azure stapel toets kluis"
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

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>Een VM door op te halen, het wachtwoord die zijn opgeslagen in de sleutel kluis implementeren

Wanneer u een beveiligde waarde (zoals een wachtwoord) als een parameter doorgeven tijdens de implementatie moet, kunt u die waarde opslaan als een geheim in een stapel Azure belangrijke kluis en verwijzen naar de waarde in andere resourcemanager Azure-sjablonen. U opnemen alleen een verwijzing naar het geheim in uw sjabloon, zodat het geheim nooit worden weergegeven. U hoeft niet te handmatig Voer de waarde in voor het geheim telkens wanneer die u de resources implementeren. U opgeven welke gebruikers of service principes toegang het geheim tot hebben.

## <a name="reference-a-secret-with-static-id"></a>Verwijzen naar een geheim met statische-ID

U verwijzen naar het geheim uit vanuit een parameterbestand, waarin waarden worden doorgegeven aan de sjabloon. U verwijzen naar het geheim door door te geven van de resource-id van de belangrijkste kluis en de naam van het geheim. Het belangrijkste kluis geheim moet al bestaan in dit voorbeeld. U gebruikt een statische waarde voor de resource-ID.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]De parameter weer die het geheim accepteert moet een *securestring*.

## <a name="next-steps"></a>Volgende stappen
[Een voorbeeld-app met de toets kluis implementeren](azure-stack-kv-sample-app.md)

[Een VM met een certificaat toets kluis implementeren](azure-stack-kv-push-secret-into-vm.md)

