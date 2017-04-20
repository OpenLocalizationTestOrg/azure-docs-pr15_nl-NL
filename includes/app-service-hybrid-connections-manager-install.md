
1. Het blad **hybride verbindingen** de hybride verbinding die u zojuist hebt gemaakt, klik op **De instelling luisteraar ervan af**.
    
    ![Klik op luisteraar ervan af-instelling](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4. Hiermee opent u het blad **hybride verbindingseigenschappen** . Kies **downloaden en handmatig configureren**onder **On-premises hybride Connection Manager**, sla het gedownloade HybridConnectionManager.msi-pakket en kopieer de verbindingsreeks van de gateway.
    
    ![Klik hier om te installeren](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. Typ de volgende opdracht start het installatieprogramma van een opdrachtprompt beheerder:

        start HybridConnectionManager.msi
 
7. Wanneer het installatieprogramma is uitgevoerd, klikt u op **niet nu**, en blader naar de map %ProgramFiles%\Microsoft\HybridConnectionManager, HCMConfigWizard.exe uitvoeren en klik op **Ja** in het dialoogvenster **Gebruikersaccountbeheer** .
        
7. Plak de verbindingsreeks van hybride die u eerder hebt gekopieerd en klik op **OK**. 
    
    ![Installeren](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. Wanneer de installatie is voltooid, klikt u op **sluiten**.
    
    ![Klik op sluiten](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
    Klik op het blad **hybride verbindingen** ziet u de kolom **Status** nu **verbonden**. 
    
    ![Status verbonden](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)