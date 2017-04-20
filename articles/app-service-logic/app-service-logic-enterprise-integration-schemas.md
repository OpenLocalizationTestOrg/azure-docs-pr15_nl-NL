<properties
    pageTitle="Overzicht van schema's maken en de Enterprise-integratie Pack | Microsoft Azure"
    description="Informatie over het gebruik van schema's maken met de apps Enterprise Integration Pack en logica"
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
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Meer informatie over het schema's en het taalpakket Enterprise-integratie  

## <a name="why-use-a-schema"></a>Het nut van een schema?
Schema's gebruiken om te bevestigen dat XML-documenten die u ontvangt geldig, met de verwachte gegevens in een vooraf gedefinieerde notatie zijn. Schema's worden gebruikt voor het valideren van berichten die worden uitgewisseld in een scenario voor B2B.

## <a name="add-a-schema"></a>Een schema toevoegen
Vanuit de Azure-portal:  

1. Selecteer **meer services**.  
![Schermafbeelding van Azure-portal, waarbij 'Meer services' gemarkeerd](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. In het zoekvak filter **integratie**invoeren en **Integratie Accounts** selecteren uit de lijst met resultaten.     
![Schermafbeelding van het zoekvak filter](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Selecteer het **account van de integratie** waaraan u het schema toevoegen.    
![Schermafbeelding van de lijst met accounts van de integratie](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Selecteer de tegel **schema's** .  
![Schermafbeelding van IEP integratie-Account, met 'Schema's ' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Een schemabestand op die kleiner is dan 2 MB toevoegen  

1. Klik in het **schema's** blad dat wordt geopend (via de voorgaande stappen), selecteert u **toevoegen**.  
![Schermafbeelding van schema's blade, met de knop 'Toevoegen' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Voer een naam voor uw schema. Als u wilt uploaden het schemabestand, selecteer het mappictogram naast het tekstvak **Schema** . Nadat de upload is voltooid, selecteert u **OK**.    
![Schermafbeelding van "Schema toevoegen', 'Kleine bestand' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Voeg een schemabestand dat groter is dan 2 MB (maximaal van 8 MB)  

Het proces voor dit is afhankelijk van het toegangsniveau voor blob container: **openbaar** of **geen anonieme toegang**. Om te bepalen dit toegangsniveau, **Azure opslag Explorer**in **Blob Containers**, selecteert u de gewenste blob-container. Selecteer **beveiliging**en selecteer het tabblad **Toegangsniveau** .

1. Als het toegangsniveau voor blob beveiliging **openbaar**is, volgt u deze stappen.  
  ![Schermafbeelding van Azure Opslagverkenner, met "Blob Containers", "Beveiliging" en "openbare' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    een. Het schema uploaden naar opslag, en kopieer de URI.  
    ![Schermafbeelding van het account, opslag, met URI gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. In **Schema toevoegen**, selecteert u **grote bestanden**en geef de URI in het tekstvak **Inhoud URI** .  
    ![Schermafbeelding van schema's, met de knop 'Toevoegen' en 'Grote bestanden' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Als het toegangsniveau voor blob beveiliging **geen anonieme toegang is**, volgt u deze stappen.  
  ![Schermafbeelding van Azure Opslagverkenner, met "Blob Containers", "Beveiliging" en "geen anonieme toegang' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    een. Upload het schema naar opslag.  
    ![Schermafbeelding van opslag-account](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. Een handtekening gedeelde toegang voor het schema genereren.  
    ![Schermafbeelding van opslag sccount, met de gedeelde toegang handtekeningen tab gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c. In **Schema toevoegen**, selecteert u **grote bestanden**en geef de handtekening gedeelde toegang URI in het tekstvak **Inhoud URI** .  
    ![Schermafbeelding van schema's, met de knop 'Toevoegen' en 'Grote bestanden' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. Klik in het blad **schema's** van het Account van de integratie EIP, nu ziet u de zojuist toegevoegde schema.  
![Schermafbeelding van EIP integratie-Account, met 'Schema's ' en het nieuwe schema gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Schema's bewerken
1. Selecteer de tegel **schema's** .  
2. Selecteer het schema dat u wilt bewerken in het **schema's** blad dat wordt geopend.
3. Selecteer **bewerken**op het blad **schema's** .  
![Schermafbeelding van schema's blade](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Selecteer het schemabestand dat u bewerken wilt met behulp van het bestand kiezer dialoogvenster dat wordt geopend.
5. Selecteer **openen** in de bestandskiezer voor het.  
![Schermafbeelding van het kiezen van bestanden](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. U ontvangt een melding weergegeven dat de upload is voltooid.  

## <a name="delete-schemas"></a>Schema's verwijderen
1. Selecteer de tegel **schema's** .  
2. Selecteer het schema dat u wilt verwijderen uit het **schema's** blad dat wordt geopend.  
3. Klik op het blad **schema's maken** , selecteert u **verwijderen**.
![Schermafbeelding van schema's blade](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Om te bevestigen van uw keuze, selecteert u **Ja**.  
![Schermafbeelding van het bevestigingsbericht "Schema verwijderen"](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Tot slot ziet dat de lijst met schema's in het blad **schema's** worden vernieuwd en het schema dat u hebt verwijderd, wordt niet meer weergegeven.  
![Schermafbeelding van EIP integratie-Account, met 'Schema's ' gemarkeerd](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Algemene informatie over het taalpakket enterprise-integratie").  
