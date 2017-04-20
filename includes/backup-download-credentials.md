## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Kluis referenties gebruiken om te verifiëren met de back-up van Azure-service

De on-premises implementatie-server (Windows-client of Windows Server of Data Protection Manager server) moet worden geverifieerd met een back-kluis voordat deze kan back-up van gegevens naar Azure. De verificatie is bereikt "kluis referenties" gebruiken. Het concept van het kluis referenties is vergelijkbaar met het concept van een bestand 'publicatie-instellingen', die wordt gebruikt in Azure PowerShell.

### <a name="what-is-the-vault-credential-file"></a>Wat is het kluis referentie-bestand?

Het bestand van de referenties kluis is een certificaat dat is gegenereerd door de portal voor elke back-kluis. De portal uploadt u de openbare sleutel vervolgens naar de Access Control Service (ACS). De persoonlijke sleutel van het certificaat is beschikbaar gemaakt voor de gebruiker als onderdeel van de werkstroom die is ingevoerd in de machine registratie-werkstroom. Hiermee wordt de machine voor het back-upgegevens verzenden naar een geïdentificeerd kluis in de back-Azure-service geverifieerd.

De referentie kluis wordt alleen gebruikt tijdens de registratie-werkstroom. Het is van de gebruiker verantwoordelijkheid om ervoor te zorgen dat het bestand dat kluis referenties niet wordt belemmerd. Als deze in de handen van elke rogue-gebruiker valt, kan het bestand van de referenties kluis worden gebruikt voor het registreren van andere computers ten opzichte van de dezelfde kluis. Terwijl de back-gegevens worden versleuteld met een wachtwoordzin die deel uitmaakt van de klant, kunnen bestaande back-ups echter kan niet worden geknoeid. Kluis referenties zijn om te beperken dit te voorkomen, ingesteld in 48hrs verloopt. Kunt u de kluis referenties van een back-kluis een willekeurig aantal malen – downloaden, maar alleen de meest recente kluis referentie-bestand is van toepassing tijdens de registratie-werkstroom.

### <a name="download-the-vault-credential-file"></a>Download het kluis referentie-bestand

Het kluis referentie-bestand is gedownload via een beveiligd kanaal van de Azure-portal. De back-up van Azure-service is niet op de hoogte van de persoonlijke sleutel van het certificaat en de persoonlijke sleutel is niet in de portal of de service. Gebruik de volgende stappen uit de kluis referentie-bestand te downloaden naar een lokale computer.

1.  Meld u aan bij de [beheerportal](https://manage.windowsazure.com/)
2.  Klik op **Services herstel** in het linkernavigatiedeelvenster en selecteert u de back-kluis die u hebt gemaakt. Klik op het cloudpictogram om te gaan naar de weergave snel aan de slag van de back-kluis.

    ![Snelle weergave](./media/backup-download-credentials/quickview.png)

3.  Klik op **downloaden kluis referenties**op de pagina snel starten. De portal genereert het kluis referentie-bestand, dat bestaat uit de downloaden.

    ![Downloaden](./media/backup-download-credentials/downloadvc.png)

4.  De portal genereert een kluis referentie met een combinatie van de naam van de kluis en de huidige datum. Klik op **Opslaan** om het downloaden van de referenties kluis naar de map downloads van de lokale account of selecteer OpslaanAls in het menu opslaan om op te geven van een locatie voor de referenties kluis.

### <a name="note"></a>Opmerking
- Zorg ervoor dat de referenties kluis is opgeslagen op een locatie die zijn toegankelijk vanaf uw computer. Als dit is opgeslagen in een bestand delen/SMB, controleert u de toegangsmachtigingen wilt.
- Het bestand van de referenties kluis wordt alleen gebruikt tijdens de registratie-werkstroom.
- Het bestand van de referenties kluis verloopt na 48hrs en kan worden gedownload van de portal.
- Raadpleeg de back-up van Azure- [Veelgestelde vragen](../articles/backup/backup-azure-backup-faq.md) voor vragen over de werkstroom.
