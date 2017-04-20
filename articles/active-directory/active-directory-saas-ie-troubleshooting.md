<properties
    pageTitle="De Access-extensie voor Configuratiescherm voor probleemoplossing voor Internet Explorer | Microsoft Azure"
    description="Klik hier voor meer informatie over het groepsbeleid gebruiken om te implementeren van de invoegtoepassing Internet Explorer voor de portal mijn Apps."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>De Access-extensie voor Configuratiescherm voor probleemoplossing voor Internet Explorer

In dit artikel kunt u de volgende problemen:

- U bent geen toegang krijgen tot uw apps via de portal mijn Apps bij het gebruik van Internet Explorer.
- U ziet het bericht 'Software installeren' Hoewel u de software al hebt geïnstalleerd.

Als u een beheerder bent, Zie ook: [het implementeren van de extensie voor Configuratiescherm van Access voor Internet Explorer met Groepsbeleid](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>Het diagnostische hulpprogramma uitvoeren

U kunt een diagnose stellen bij installatieproblemen met de Access-extensie voor Configuratiescherm door te downloaden en het deelvenster toegang diagnostische hulpprogramma mailflow:

1. [Klik hier om de diagnostische hulpprogramma te downloaden.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Open het bestand en klik op **alle uitpakken** .

    ![Druk op alle uitpakken](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Druk vervolgens op de knop **extraheren** om door te gaan.

    ![Druk op uitpakken](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Naar het hulpprogramma uitvoeren, met de rechtermuisknop op het bestand met de naam **AccessPanelExtensionDiagnosticTool**en selecteer **openen met > Microsoft Windows op basis van Script Host**.

    ![Openen met > Microsoft Windows op basis van scripthost](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Vervolgens wordt het volgende diagnostische venster waarin wordt beschreven wat mogelijk zijn er mis met de installatie.

    ![Een voorbeeld van het venster diagnostische gegevens](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Klik op '**Ja**' als u wilt dat het programma de problemen opgelost die zijn gevonden.

7. Deze wijzigingen op te slaan sluit alle vensters van Internet Explorer en open Internet Explorer opnieuw.<br />Als u nog steeds geen toegang uw apps tot, kunt u de onderstaande stappen.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Controleren of de Access-extensie voor Configuratiescherm is ingeschakeld

Controleren of de Access-extensie voor Configuratiescherm is ingeschakeld in Internet Explorer:

1. Klik op het **tandwielpictogram** in de rechterbovenhoek van het venster in Internet Explorer. Selecteer vervolgens **Internetopties**.<br />(In oudere versies van Internet Explorer vindt u onder **Extra > Internetopties**.

    ![Ga naar Extra > Internetopties](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Klik op het tabblad **programma's** en klik op op de knop **Invoegtoepassingen beheren** .

    ![Klik op Invoegtoepassingen beheren](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. In dit dialoogvenster, selecteert u **Access-extensie voor Configuratiescherm** en klik op de knop **inschakelen** .

    ![Klik op inschakelen](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. Deze wijzigingen op te slaan sluit alle vensters van Internet Explorer en open Internet Explorer opnieuw.

##<a name="enable-extensions-for-inprivate-browsing"></a>Uitbreidingen voor InPrivate-navigatie inschakelen

Als u de modus InPrivate-navigatie gebruikt:

1. Klik op het **tandwielpictogram** in de rechterbovenhoek van het venster in Internet Explorer. Selecteer vervolgens **Internetopties**.<br />(In oudere versies van Internet Explorer vindt u onder **Extra > Internetopties**.

    ![Een voorbeeld van het venster diagnostische gegevens](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Ga naar het tabblad **Privacy** , **Schakel** het selectievakje **uitschakelen werkbalken en uitbreidingen wanneer InPrivate-navigatie wordt gestart**</p>

    ![Schakel uitschakelen werkbalken en uitbreidingen wanneer InPrivate-navigatie wordt gestart](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. Deze wijzigingen op te slaan sluit alle vensters van Internet Explorer en open Internet Explorer opnieuw.

##<a name="uninstall-the-access-panel-extension"></a>Verwijderen van de Access-extensie voor Configuratiescherm

De uitbreiding van Access Configuratiescherm vanaf uw computer verwijderen:

1. Druk op de **Windows-toets** om het menu Start openen op het toetsenbord. Wanneer het menu geopend is, kunt u typen als u wilt zoeken. Typ "Configuratiescherm" en open vervolgens het **Configuratiescherm** wanneer dit wordt weergegeven in de lijst met zoekresultaten.

    ![Zoeken naar het Configuratiescherm](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. In de rechterbovenhoek van het Configuratiescherm, wijzigt u de optie **weergeven op** **grote**pictogrammen. Zoek en klik op de knop **programma's en onderdelen** .

    ![Chang de weergave grote pictogrammen weer te geven](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. Selecteer in de lijst, **Access-extensie voor Configuratiescherm**en klik vervolgens op op de knop **verwijderen** .

    ![Klik op verwijderen](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Vervolgens kunt u proberen te installeren van de extensie opnieuw als u wilt zien of het probleem is opgelost.

Als u problemen hebt met het verwijderen van de extensie, kunt u deze met het hulpprogramma [Microsoft oplossen deze](https://go.microsoft.com/?linkid=9779673) ook verwijderen.

## <a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Het implementeren van de Access-extensie voor Configuratiescherm voor Internet Explorer met Groepsbeleid](active-directory-saas-ie-group-policy.md)
