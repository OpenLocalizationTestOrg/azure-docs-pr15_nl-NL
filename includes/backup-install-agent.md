## <a name="download-install-and-register-the-azure-backup-agent"></a>Downloaden, installeren en registreren van de back-up van Azure-agent

Nadat de kluis Azure back-up is gemaakt, moet een agent op elk van uw Windows-computers (Windows Server, Windows client, System Center Data Protection Manager server of Azure back-up-servercomputer) waarmee een back-up van gegevens en Azure-toepassingen zijn geïnstalleerd.

1. Meld u aan bij de [beheerportal](https://manage.windowsazure.com/)

2. Klik op **Herstel Services**en selecteer vervolgens de back-kluis die u wilt registreren bij een server. De pagina snel aan de slag voor die back-kluis verschijnt.

    ![Snel aan de slag](./media/backup-install-agent/quickstart.png)

3. Klik op de pagina snel starten op de optie **voor Windows Server of System Center Data Protection Manager of Windows-client** onder **Agent downloaden**. Klik op **Opslaan** naar de lokale computer te kopiëren.

    ![Agent opslaan](./media/backup-install-agent/agent.png)

4. Wanneer de-agent is geïnstalleerd, dubbelklikt u op MARSAgentInstaller.exe om de installatie van de back-up van Azure-agent starten. Kies de installatiemap en tijdelijke map die zijn vereist voor de agent. De locatie van de cache opgegeven moet beschikbare ruimte waarin ten minste 5% van de back-upgegevens hebben.

5.  Als u een proxyserver gebruikt verbinding maken met internet, klikt u in de **configuratie van Proxy** -scherm, geef de details van de server proxy. Als u een geverifieerde proxy bevindt gebruikt, voert u de gegevens van de gebruiker gebruikersnaam en wachtwoord in dit venster.

6.  De back-up van Azure-agent installeert .NET Framework 4.5 en Windows PowerShell (als deze nog niet beschikbaar) om de installatie te voltooien.

7.  Wanneer de-agent is geïnstalleerd, klikt u op de knop **Doorgaan naar de registratie** wilt gaan met de werkstroom.

    ![Register](./media/backup-install-agent/register.png)

8. Blader naar in het scherm kluis referenties en selecteer het bestand dat kluis referenties die eerder is gedownload.

    ![Kluis referenties](./media/backup-install-agent/vc.png)

    Het bestand van de referenties kluis geldt alleen voor 48 uur (na het downloaden van de portal). Als u een fout in dit scherm (bijvoorbeeld "kluis referenties bestand biedt is verlopen'), meld u aan bij de portal van Azure aantreft en het kluis referenties opnieuw downloaden.

    Zorg ervoor dat het bestand van de referenties kluis beschikbaar is op een locatie die toegankelijk is voor het installatieprogramma. Als u toegang tot gerelateerde fouten, de kluis referenties-bestand kopiëren naar een tijdelijke locatie op deze computer en probeer het opnieuw.

    Als u een ongeldige kluis referentie fout (bijvoorbeeld "Ongeldige kluis referenties opgegeven") het bestand is beschadigd of bevat niet hebt de meest recente referenties die is gekoppeld aan de herstel-service. Probeer opnieuw na een nieuw kluis referentie-bestand downloaden uit de portal. Deze fout wordt meestal weergegeven als de gebruiker de optie **downloaden kluis referentie** in de portal Azure snel achter elkaar op. In dit geval alleen het tweede kluis referentie bestand is geldig.

9. In het scherm **versleuteling instelling** kunt u een wachtwoordzin genereren of een coderingssleutel (minimaal van 16 tekens) opgeven. Moet de wachtwoordzin opslaan op een veilige locatie.

    ![Versleuteling](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Als de wachtwoordzin is kwijt of vergeten; De help van Microsoft kan niet in de back-upgegevens terugzetten. De eindgebruiker eigenaar is van de wachtwoordzin versleuteling en Microsoft geen inzicht in de wachtwoordzin die worden gebruikt door de gebruiker. Sla het bestand op een veilige locatie als dit nodig tijdens een herstelbewerking is.

10. Wanneer u op de knop **Voltooien** klikt, wordt de computer is geregistreerd om de en u bent nu klaar om een reservekopie op Microsoft Azure.

11. Wanneer u een back-up van Microsoft Azure zelfstandig gebruikt, kunt u de instellingen die tijdens de registratie-werkstroom is opgegeven door te klikken op de optie **Eigenschappen wijzigen** in de back-up van Azure mmc module kunt wijzigen.

    ![De eigenschappen van wijzigen](./media/backup-install-agent/change.png)

    Wanneer u een Data Protection Manager gebruikt, kunt u ook de instellingen die is opgegeven tijdens de registratie-werkstroom door te klikken op de optie **configureren** door in te schakelen **Online** onder het tabblad **rollenbeheer** wijzigen.

    ![Azure back-ups configureren](./media/backup-install-agent/configure.png)
