<!--author=SharS last changed: 01/15/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>Update 1.2 installeren vanaf de portal van Azure klassieke

1. Selecteer uw apparaat op de pagina StorSimple service. Navigeer naar **apparaten** > **onderhoud**.

2. Klik onder aan de pagina, op **Updates scannen**. Een taak wordt gemaakt als u wilt zoeken naar beschikbare updates. U krijgt wanneer de taak is voltooid.

3. Klik in de sectie **Software-Updates** op dezelfde pagina ziet u dat nieuwe software-updates beschikbaar zijn. Het is raadzaam dat u de releaseopmerkingen bekijken voordat u Update 1.2 op uw apparaat toepassen.

    ![Software-updates installeren](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)

4. Klik onder aan de pagina, op **Updates installeren**.

5. U wordt gevraagd om bevestiging. Klik op **OK**.

6. Een dialoogvenster **Updates installeren** er wordt weergegeven. Uw apparaat moet voldoen aan de controles die in dit dialoogvenster wordt vermeld. Deze stappen zijn voltooid voordat u de update. Selecteer **ik meer informatie over de bovenstaande vereiste en ben van mijn apparaat bijwerken**. Klik op het pictogram van het selectievakje.

    ![Bevestigingsbericht](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)

7. Een reeks automatische oude controles wordt nu starten. Hierbij:

    - **Controller-status wordt gecontroleerd** om te bevestigen dat de apparaatcontrollers orde en online zijn.
    
    - **Hardware onderdeel systeemstatus Hiermee wordt gecontroleerd** om te bevestigen dat de hardware-onderdelen op uw apparaat StorSimple in orde zijn.
    
    - **Gegevens 0 Hiermee wordt gecontroleerd** om te bevestigen dat de gegevens 0 op uw apparaat is ingeschakeld. Als deze interface niet is ingeschakeld, moet u naar deze inschakelen en probeer het opnieuw.
    
    - **Gegevens 2 en 3 van de gegevens Hiermee wordt gecontroleerd** om te bevestigen dat de gegevens 2 en 3 van de gegevens netwerk-interfaces niet zijn ingeschakeld. Als deze interfaces zijn ingeschakeld, wordt moet u ze uitschakelen en probeer het bijwerken van uw apparaat. Deze controle wordt uitgevoerd alleen als u wilt bijwerken vanaf een apparaat met GA software. Apparaten met versies 0,1, 0,2 of 0,3 hoeft dit selectievakje niet.
    
    - De **gateway controleren** op elk apparaat met een versie vóór Update 1. Deze controle wordt uitgevoerd op alle het apparaat uitgevoerd oude update 1-software, maar niet op de apparaten met een gateway is geconfigureerd voor de netwerkinterface van een dan gegevens 0.
 
    Update 1.2 wordt alleen toegepast als de bovenstaande oude update controles zijn voltooid. U wordt geïnformeerd dat oude update controles bezig zijn.
  
    ![Melding vooraf controleren](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)

    Hier volgt een voorbeeld waarin de voorafgaand aan de upgrade controleren is mislukt. U moet controleren of de apparaatcontrollers orde en online. U moet ook de status van de hardwareonderdelen controleren. In dit voorbeeld Controller 0 en 1 Controller onderdelen aandacht meer nodig heeft. Mogelijk moet u contact op met Microsoft Support als u deze problemen door zelf kan niet beantwoorden.

     ![Oude controle is mislukt](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

    > [AZURE.NOTE] Nadat u Update 1.2 hebt toegepast op uw apparaat StorSimple, controles die gegevens 2 en 3 van de gegevens en de gateway controleren wordt niet meer nodig zijn voor toekomstige updates. Andere oude controles worden nog steeds moeten uitgevoerd.


8. Nadat de voorafgaand aan de upgrade controles zijn voltooid, kunt u een bijwerktaak wordt gemaakt. U krijgt wanneer de bijwerktaak is gemaakt.
 
    ![Het maken van taken bijwerken](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)

    De update worden vervolgens worden toegepast op uw apparaat.
 
9. Klik op **Taak weergeven**om de voortgang van de bijwerktaak. Klik op de pagina **taken** ziet u de voortgang bijwerken. 

    ![Voortgang van taak bijwerken](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)

10. De update worden enkele uren duren. U kunt de details van de taak op elk gewenst moment weergeven.

    ![Details van update](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)

11. Wanneer de taak voltooid is, Ga naar **de onderhoudspagina** en schuif omlaag naar **Software-Updates**.

12. Controleer of uw apparaat **StorSimple 8000 reeks Update 1.2 (6.3.9600.17584)**wordt uitgevoerd. **Laatst bijgewerkt datum** moet ook worden gewijzigd.

    ![Onderhoudspagina](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)

13. U ziet nu dat onderhoud modus updates beschikbaar zijn. Deze updates zijn storend updates die resulteren in apparaat downtime en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat. Volg de instructies in het [onderhoud modus updates installeren](storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple) voor deze updates via de Windows PowerShell installeren voor StorSimple.

> [AZURE.NOTE] Het bericht waarin wordt aangegeven dat onderhoud modus updates beschikbaar zijn mogelijk tot 24 uur nadat de updates van de modus onderhoud zijn toegepast op het apparaat worden weergegeven in bepaalde gevallen wordt.  


