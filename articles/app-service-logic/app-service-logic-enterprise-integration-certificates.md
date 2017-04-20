
<properties
    pageTitle="Gebruik van certificaten met Enterprise Integration Pack | Microsoft Azure"
    description="Informatie over het gebruik van certificaten met de Enterprise-integratie Inpakken en logica Apps"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Meer informatie over certificaten en Enterprise-integratie Pack

## <a name="overview"></a>Overzicht
Enterprise-integratie gebruikt certificaten B2B communicatie beveiligen. U kunt twee typen certificaten gebruiken in uw bedrijfs-apps voor integratie:

- Openbare certificaten, waarmee moeten worden aangeschaft bij een certificeringsinstantie (CA).
- Persoonlijke certificaten, waarmee u zelf kunt geven. Deze certificaten worden ook wel zelfondertekende certificaten genoemd.


## <a name="what-are-certificates"></a>Wat zijn de certificaten?
Certificaten zijn digitale documenten die de identiteit van de deelnemers in elektronische communicatie bevestigen en die ook elektronische communicatie beveiligen.

## <a name="why-use-certificates"></a>Het nut van certificaten?
Soms moet B2B communicatie vertrouwelijk. Enterprise-integratie gebruikt certificaten deze communicatie op twee manieren beveiligen:

- Door te coderen van de inhoud van berichten
- Door berichten digitaal te ondertekenen  

## <a name="how-do-you-upload-certificates"></a>Hoe kan u certificaten uploaden?

### <a name="public-certificates"></a>Openbare certificaten
Als u een *openbare certificaat* in uw logica-apps gebruiken met B2B mogelijkheden, moet u eerst het certificaat uploaden naar uw account integratie. Als u wilt een *zelfondertekend certificaat*gebruikt, aan de andere kant, moet u eerst uploaden deze [Azure toets kluis]voor(../key-vault/key-vault-get-started.md "meer informatie over sleutel kluis").

Nadat u een certificaat uploaden, is deze beschikbaar waarmee u uw berichten B2B secure wanneer u hun eigenschappen definiëren in de [overeenkomsten](./app-service-logic-enterprise-integration-agreements.md) die u maakt.  

Hier volgen de gedetailleerde stappen voor het uploaden van uw openbare certificaten in uw account integratie nadat u zich aanmeldt bij de portal van Azure:

1. Selecteer **Bladeren**.  
    ![Selecteer Bladeren](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. **Integratie** invoeren in het zoekvak van het filter en selecteer vervolgens **Integratie Accounts** in de lijst met resultaten.     
    ![Selecteer Accounts van de integratie](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. Selecteer het integratie account waaraan u wilt toevoegen van het certificaat.  
    ![Selecteer de integratie account waaraan u wilt toevoegen van het certificaat](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  Selecteer de tegel **certificaten** .  
    ![Selecteer de tegel certificaten](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. Selecteer in het blad **certificaten** dat wordt geopend, de knop **toevoegen** .
    ![Klik op de knop toevoegen](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Voer een **naam** voor het certificaat en selecteer vervolgens het certificaattype. (In dit voorbeeld gebruikt we het openbare certificaattype.) Selecteer het pictogram van een map aan de rechterkant van het tekstvak van het **certificaat** .

7. Wanneer het kiezen van bestanden wordt geopend, zoek en selecteer het certificaatbestand dat u wilt uploaden naar uw account integratie.

8. Selecteer het certificaat en selecteer vervolgens **OK** in de bestandskiezer voor het. Dit is gevalideerd en uploadt het certificaat bij uw account integratie.

8. Tot slot weer op het blad **certificaat toevoegen** , selecteer de knop **OK** .  
    ![Selecteer de knop OK](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. In ongeveer een minuut ziet u een melding dat wordt aangegeven dat de certificaat-upload voltooid is.

10. Selecteer de tegel **certificaten** . Hier ziet u de zojuist toegevoegde certificaat.  
    ![Zie de nieuw certificaat](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Persoonlijke certificaten
U kunt persoonlijke certificaten uploaden naar uw account integratie door middel van de volgende stappen uit:  

1. [Uw persoonlijke sleutel toets kluis uploaden] (../key-vault/key-vault-get-started.md "Meer informatie over de belangrijkste kluis").  

    > [AZURE.TIP] U moet de functie logica Apps van Azure App-Service aan bewerkingen uitvoeren op de toets kluis machtigen. U kunt toegang verlenen tot de hoofdsom van de service logica Apps met behulp van de volgende PowerShell-opdracht:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. Een persoonlijk certificaat maken.  

3. Het privé certificaat uploaden naar uw account integratie.

Nadat u de vorige stappen hebt gemaakt, kunt u het privé certificaat overeenkomsten maken.

Hier volgen de gedetailleerde stappen voor het uploaden van uw persoonlijke certificaten in uw account integratie nadat u zich aanmeldt bij de portal van Azure:  

1. Selecteer **Bladeren**.  
    ![Uw persoonlijke certificaten uploaden naar uw account integratie](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. **Integratie** invoeren in het zoekvak van het filter en selecteer vervolgens **Integratie Accounts** in de lijst met resultaten.     
    ![Selecteer Accounts van de integratie](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. Selecteer het integratie account waaraan u wilt toevoegen van het certificaat.  
    ![Selecteer de integratie account waaraan u wilt toevoegen van het certificaat](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. Selecteer de tegel **certificaten** .  
    ![Selecteer de tegel certificaten](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. Selecteer in het blad **certificaten** dat wordt geopend, de knop **toevoegen** .
    ![Klik op de knop toevoegen](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Voer een **naam** voor het certificaat en selecteer vervolgens het certificaattype. (In dit voorbeeld gebruikt we het openbare certificaattype.) Selecteer het pictogram van een map aan de rechterkant van het tekstvak van het **certificaat** .

7. Wanneer het kiezen van bestanden wordt geopend, zoek en selecteer het certificaatbestand dat u wilt uploaden naar uw account integratie.

8. Nadat u het certificaat hebt geselecteerd, selecteert u **OK** in de bestandskiezer voor het. Deze actie is gevalideerd met het certificaat en geüpload naar uw account integratie.

9. Ten slotte weer op het blad **certificaat toevoegen** , selecteer de knop **OK** .  
    ![Certificaat toevoegen](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. In ongeveer een minuut ziet u een melding dat wordt aangegeven dat de certificaat-upload voltooid is.

11. Selecteer de tegel **certificaten** . Hier ziet u de zojuist toegevoegde certificaat.
    ![Zie de nieuw certificaat](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Nadat u een certificaat uploaden, is deze beschikbaar waarmee u uw berichten B2B secure wanneer u hun eigenschappen in [overeenkomsten](./app-service-logic-enterprise-integration-agreements.md)definiëren.  

## <a name="next-steps"></a>Volgende stappen
- [Een logica app maakt die B2B-functies worden gebruikt](./app-service-logic-enterprise-integration-b2b.md)  
- [Een overeenkomst B2B maken](./app-service-logic-enterprise-integration-agreements.md)  
- [Meer informatie over sleutel kluis] (../key-vault/key-vault-get-started.md "Meer informatie over de belangrijkste kluis")  
