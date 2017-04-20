## <a name="defining-a-backup-policy"></a>Een back-beleid definiëren

Een back-beleid bepaalt een matrix van wanneer de gegevens zijn momentopnamen en hoe lang deze momentopnamen blijven behouden. Wanneer u een beleid voor het back-ups van een VM definieert, kunt u een back-uptaak *één keer per dag*kunt activeren. Wanneer u een nieuw beleid maakt, wordt deze toegepast op de kluis. De interface van de back-beleid ziet er zo uit:

![Back-beleid](./media/backup-create-policy-for-vms/backup-policy.png)

Een beleid maken:

1. Voer een naam voor de **Beleidsnaam**.

2. Momentopnamen van uw gegevens kunnen worden gemaakt op dagelijks of wekelijks momenten. Gebruik het vervolgkeuzemenu **Back-up frequentie** om te kiezen of gegevens momentopnamen worden dagelijks of wekelijks.

    - Als u een dagelijks interval kiest, gebruik u het gemarkeerde besturingselement te selecteren van de tijd van de dag van de momentopname. Als u wilt wijzigen van het uur, schakelt u het uur en selecteer het nieuwe uur.

    ![Dagelijkse back-beleid](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Als u een wekelijks interval kiest, moet u de gemarkeerde besturingselementen gebruiken om de dag van de week, en het tijdstip aan de momentopname te selecteren. Selecteer een of meer dagen in het menu dag. Selecteer in het menu uur één uur. Als u wilt wijzigen van het uur, schakelt u het geselecteerde uur en selecteer het nieuwe uur.

    ![Wekelijkse back-beleid](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Standaard worden alle **Bewaarbeleid bereik** opties geselecteerd. Schakel een bewaarbeleid bereik beperking dat u niet wilt gebruiken. Geef vervolgens de interval(s) gebruiken.

    Maandelijkse en jaarlijkse bewaarbeleid bereiken, kunt u de momentopnamen op basis van een wekelijks of dagelijks verhoging opgeven.

    >[AZURE.NOTE] Wanneer een VM beveiligt, wordt een back-uptaak uitgevoerd één keer per dag. De tijd waarop de back-up wordt uitgevoerd is dezelfde voor elk bereik bewaarbeleid.

4. Klik op **Opslaan**nadat alle opties voor het beleid, aan de bovenkant van het blad.

    Het nieuwe beleid wordt onmiddellijk toegepast op de kluis.
