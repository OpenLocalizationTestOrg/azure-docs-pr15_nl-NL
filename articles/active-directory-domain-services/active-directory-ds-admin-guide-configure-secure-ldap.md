<properties
    pageTitle="Veilige LDAP (leesbaar TEKSTFORMAAT) configureren in Azure AD-domeinservices | Microsoft Azure"
    description="Secure LDAP (LDAPS) voor een beheerde domein van Azure AD Domain Services configureren"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Secure LDAP (LDAPS) voor een beheerde domein van Azure AD Domain Services configureren
In dit artikel leest hoe u Secure Lightweight Directory Access Protocol (leesbaar TEKSTFORMAAT) kunt inschakelen voor uw beheerde domeinservices van Azure AD-domein. Veilige LDAP wordt ook wel ' Lightweight Directory Access Protocol (LDAP) via Secure Sockets Layer (SSL) / TLS Transport Layer Security ()'.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken in dit artikel, hebt u het volgende nodig:

1. Een geldig **abonnement op Azure**.

2. Een **Azure AD directory** - hetzij gesynchroniseerd met een on-premises adreslijst of een map alleen de cloud.

3. **Azure AD-domeinservices** moet zijn ingeschakeld voor de Azure AD-map. Als u dit nog niet hebt gedaan, voert u alle taken die worden beschreven in de [handleiding aan de slag](./active-directory-ds-getting-started.md).

4. Een **certificaat moet worden gebruikt om in te schakelen veilige LDAP**.
    - **Aanbevolen** - een certificaat aanvragen van uw onderneming CA of openbare certificeringsinstantie. Deze configuratieoptie is veiliger.
    - U kunt ook kunt u ook [een zelfondertekend certificaat maken](#task-1---obtain-a-certificate-for-secure-ldap) zoals verderop in dit artikel.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Vereisten voor het beveiligde LDAP-certificaat
In het bezit van een geldig certificaat per de volgende richtlijnen, voordat u veilige LDAP inschakelen. Er optreden fouten als u probeert te veilige LDAP voor uw domein beheerde met een ongeldig/onjuiste certificaat inschakelen.

1. **Vertrouwde uitgever** - het certificaat moet worden uitgegeven door een vertrouwd door computers die u moeten verbinding maken met het domein gebruiken, veilige LDAP. Deze instantie mogelijk van uw organisatie enterprise certificeringsinstantie of een openbare certificeringsinstantie vertrouwd door deze computers.

2. **Levensduur** - certificaat moet ten minste de komende 3 tot en met 6 maanden geldig zijn. Beveiligde LDAP-toegang tot uw beheerde domein wordt onderbroken wanneer het certificaat is verlopen.

3. **Onderwerpnaam** - de onderwerpnaam op het certificaat moet een jokerteken voor uw domein beheerde. Bijvoorbeeld als 'contoso100.com' naam van uw domein, de naam van een onderwerp van het certificaat moet ' *. contoso100.com'. Stel de DNS-naam (onderwerpnaam alternatieve) op de jokertekennaam van deze.

3. **Gebruik van de sleutel** - het certificaat moet worden geconfigureerd voor de volgende doeleinden - digitale handtekeningen en uitwisselen.

4. **Certificaatdoel** - certificaat moet zijn geldig voor het SSL-serververificatie.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Taak 1: een certificaat aanvragen voor secure LDAP
De eerste taak heeft betrekking op het verkrijgen van een certificaat dat is gebruikt voor secure LDAP-toegang tot het beheerde domein. U hebt twee opties:

- Een certificaat aanvragen bij een certificeringsinstantie. De instantie mogelijk van uw organisatie Certificeringsinstantie, of een openbare certificeringsinstantie.

- Een zelfondertekend certificaat maken.


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Optie een (aanbevolen): een secure LDAP-certificaat aanvragen bij een certificeringsinstantie
Als uw organisatie implementeert een enterprise public key infrastructure (PKI), moet u een certificaat aanvragen bij de enterprise-certificeringsinstantie (CA) voor uw organisatie. Als uw organisatie wordt opgehaald van de certificaten van een openbare certificeringsinstantie, moet u de beveiligde LDAP-certificaat verkrijgen van openbare certificeringsinstantie.

Wanneer u een certificaat aanvraagt, moet u ervoor zorgen dat u de vereisten die worden beschreven in de [vereiste voor het beveiligde LDAP-certificaat](#requirements-for-the-secure-ldap-certificate)opvolgt.

> [AZURE.NOTE] Clientcomputers die u moeten verbinding maken met de beheerde domein gebruiken, veilige LDAP moeten de uitgever van het certificaat leesbaar TEKSTFORMAAT vertrouwen.


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Optie B - een zelfondertekend certificaat maken voor secure LDAP
U kunt een zelfondertekend certificaat maken voor veilige LDAP als:

- certificaten in uw organisatie worden niet uitgegeven door een certificeringsinstantie enterprise of
- u verwacht niet gebruik een certificaat van een openbare certificeringsinstantie.

**Een zelfondertekend certificaat via PowerShell maken**

Open een nieuw venster PowerShell als **Administrator** op uw Windows-computer, en typ de volgende opdrachten, een nieuwe zelfondertekend certificaat maken.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

Vervang in het voorgaande voorbeeld, 'contoso100.com' met de DNS-naam van uw beheerde domeinservices van Azure AD-domein.

![Selecteer Azure AD-map](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Het nieuwe zelfondertekend certificaat bevindt zich in de lokale computer certificaat store.


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Taak 2: exporteren de veilige LDAP-certificaat naar een. PFX-bestand
Voordat u deze taak, zorg ervoor dat u het beveiligde LDAP-certificaat hebt ontvangen van uw ondernemingscertificeringsinstantie of een openbare certificeringsinstantie of een zelfondertekend certificaat hebt gemaakt.

Voer de volgende stappen uit, als u wilt exporteren van het certificaat leesbaar TEKSTFORMAAT om te een. PFX-bestand.

1. Druk op de knop **Start** en typ **R**. Klik in het dialoogvenster **uitvoeren** , typ **mmc** en klik op **OK**.

    ![De MMC-console starten](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. Klik op de prompt **Gebruikersaccountbeheer** op **Ja** om te starten MMC (Microsoft Management Console) als beheerder.

3. Klik in het menu **bestand** op **toevoegen/verwijderen uitlijnen-in...**.

    ![Module toevoegen aan MMC-console](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. Selecteer in het dialoogvenster **toevoegen of verwijderen van modules** , de module **certificaten** en klik op de **toevoegen >** knop.

    ![Certificaatmodule toevoegen aan MMC-console](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. Klik in de wizard **certificaatmodule** Selecteer **computeraccount** en klik op **volgende**.

    ![Certificaten-module voor computeraccount toevoegen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. Selecteer op de pagina **Computer selecteren** **lokale computer: (de computer waarop deze console wordt uitgevoerd)** en klik op **Voltooien**.

    ![Module Certificaten - select computer toevoegen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. Klik op **OK** om de module Certificaten toevoegen aan MMC in het dialoogvenster **toevoegen of verwijderen van modules** .

    ![Certificaatmodule toevoegen aan MMC - klaar](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. Klik in het venster MMC op **De hoofdmap van Console**. U ziet de module Certificaten geladen. Klik op **certificaten (lokale Computer)** om uit te vouwen. Klik op het **persoonlijke** knooppunt, gevolgd door het knooppunt **certificaten** .

    ![Openen met persoonlijke certificaten](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. Hier ziet u de zelfondertekend certificaat die u hebt gemaakt. U kunt de eigenschappen van het certificaat om ervoor te zorgen dat op de windows PowerShell gemeld bij het maken van het certificaat komt overeen met de vingerafdruk onderzoeken.

10. Selecteer de zelfondertekend certificaat en **Klik met de rechtermuisknop**. In het snelmenu, selecteert u **Alle taken** en selecteer **exporteren...**.

    ![Certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. Klik in de **Wizard Certificaat exporteren**, klikt u op **volgende**.

    ![Certificaatwizard exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. Selecteer **Ja, de persoonlijke sleutel exporteren**op de pagina **Persoonlijke sleutel exporteren** en klik op **volgende**.

    ![Persoonlijke certificaatsleutel exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] U moet de persoonlijke sleutel samen met het certificaat exporteren. Als u een PFX die de persoonlijke sleutel voor het certificaat niet bevat opgeeft, mislukt veilige LDAP voor uw domein beheerde inschakelen.

13. Selecteer op de pagina **Bestandsindeling voor Export** **Personal Information Exchange - PKCS #12 (. PFX)** als de bestandsindeling voor het geëxporteerde certificaat.

    ![De bestandsindeling certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] Alleen de. PFX-bestandsindeling wordt ondersteund. Het certificaat niet exporteren de. CER-bestandsindeling.

14. Klik op de pagina **beveiliging** Selecteer de optie **wachtwoord** en typ een wachtwoord beveiligen het. PFX-bestand. In dit wachtwoord onthouden omdat deze nodig in de volgende taak zijn. Klik op **volgende** om door te gaan.

    ![Wachtwoord voor een certificaat exporteren ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] Noteer dit wachtwoord. U deze nodig hebt tijdens het activeren van secure LDAP voor dit domein beheerde in [taak 3: veilige LDAP voor het domein dat beheerde inschakelen](#task-3---enable-secure-ldap-for-the-managed-domain)

15. Geef de bestandsnaam en de locatie waar u naartoe wilt exporteren van het certificaat op de pagina **te exporteren bestand** .

    ![Pad voor certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. Klik op de volgende pagina, klikt u op **Voltooien** om het certificaat exporteren naar een PFX-bestand. U ziet bevestigingsvenster wanneer het certificaat is geëxporteerd.

    ![Klaar certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Taak 3: veilige LDAP inschakelen voor het domein dat beheerde
Als u wilt inschakelen veilige LDAP, voert u de volgende configuratiestappen:

1. Ga naar de **[klassieke Azure-portal](https://manage.windowsazure.com)**.

2. Selecteer het knooppunt **Active Directory** in het linkerdeelvenster.

3. Selecteer de Azure AD-map (ook wel 'tenant'), waarvoor u Azure AD-domeinservices hebt ingeschakeld.

    ![Selecteer Azure AD-map](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klik op het tabblad **configureren** .

    ![Tabblad van map configureren](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Schuif omlaag naar de sectie **domain services**. Hier ziet u een optie getiteld **Secure LDAP (LDAPS)** , zoals wordt weergegeven in de volgende schermafbeelding:

    ![Sectie Domain Services-configuratie](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. Klik op de knop **configureren certificaat …** om het dialoogvenster **Certificaat configureren voor LDAP Secure** weer te geven.

    ![Certificaat voor secure LDAP configureren](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. Klik op het mappictogram onder **PFX bestand met certificaat** het PFX-bestand, waarin het certificaat dat u wilt gebruiken voor secure LDAP-toegang tot het beheerde domein opgeven. Ook Voer het wachtwoord die u hebt opgegeven toen het certificaat naar het PFX-bestand exporteert. Klik vervolgens op de knop Gereed onderaan.

    ![Secure LDAP PFX-bestand en het wachtwoord opgeven](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. De sectie **domeinservices** van het tabblad **configureren** moeten krijgen grijs en staat in de **in behandeling...** provinciale voor een paar minuten. Het certificaat leesbaar TEKSTFORMAAT is geverifieerd voor de nauwkeurigheid en veilige LDAP is geconfigureerd voor uw domein beheerde tijdens deze periode.

    ![LDAP - Secure wachtende staat](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] Het duurt ongeveer 10 tot 15 minuten veilige LDAP voor uw domein beheerde inschakelen. Als de meegeleverde secure LDAP-certificaat niet overeenkomen met de vereiste criteria, veilige LDAP is niet ingeschakeld voor uw adreslijst en er een fout. Bijvoorbeeld de naam van het domein klopt, het certificaat is verlopen of verloopt binnenkort enz.

9. Wanneer veilige LDAP is ingeschakeld voor uw beheerde domein, de **in behandeling...** bericht moet verdwijnen. Hier ziet u de vingerafdruk van het certificaat dat wordt weergegeven.

    ![Veilige LDAP ingeschakeld](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Taak 4 - secure LDAP-toegang inschakelen via internet
**Optionele taak** - als u niet wilt toegang tot het beheerde domein met leesbaar TEKSTFORMAAT via internet, gaat u verder deze configuratietaak.

Voordat u deze taak, controleer dan of dat u de stappen in de [taak 3](#task-3---enable-secure-ldap-for-the-managed-domain)hebt voltooid.

1. Hier ziet u een optie **Inschakelen SECURE LDAP toegang via INTERNET** in de sectie **domeinservices** van de pagina **configureren** . Deze optie is standaard ingesteld op **geen** aangezien internettoegang tot het beheerde domein via secure LDAP is standaard uitgeschakeld.

    ![Veilige LDAP ingeschakeld](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. Wisselknop **veilige LDAP toegang via de INTERNET inschakelen** op **Ja**. Klik op de knop **Opslaan** op de onderkant van de verpakking.
    ![Veilige LDAP ingeschakeld](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. De sectie **domeinservices** van het tabblad **configureren** moeten krijgen grijs en staat in de **in behandeling...** provinciale voor een paar minuten. Na enige tijd is om uw beheerde domein via secure LDAP toegang tot internet ingeschakeld.

    ![LDAP - Secure wachtende staat](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] Het duurt ongeveer 10 minuten internettoegang via secure LDAP voor uw domein beheerde inschakelen.

4. Wanneer beveiligde LDAP-toegang tot uw beheerde domein via internet is ingeschakeld, de **in behandeling...** bericht moet verdwijnen. Hier ziet u de externe IP-adres dat kan worden gebruikt voor toegang tot het telefoonboek van uw via leesbaar TEKSTFORMAAT in het veld **Externe IP-adres voor leesbaar TEKSTFORMAAT toegang**.

    ![Veilige LDAP ingeschakeld](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Taak 5: DNS voor toegang tot het beheerde domein van internet configureren
**Optionele taak** - als u niet wilt toegang tot het beheerde domein met leesbaar TEKSTFORMAAT via internet, gaat u verder deze configuratietaak.

Voordat u deze taak, controleer dan of dat u de stappen in de [taak 4](#task-4---enable-secure-ldap-access-over-the-internet)hebt voltooid.

Zodra u hebt beveiligde LDAP toegang via internet ingeschakeld voor uw domein beheerde, moet u DNS bijwerken zodat clientcomputers dit beheerde domein kunnen vinden. Aan het einde van de taak 4, wordt een extern IP-adres op het tabblad **configureren** in **Externe IP-adres voor leesbaar TEKSTFORMAAT toegang**weergegeven.

Uw externe DNS-provider zodanig configureren dat de DNS-naam van het beheerde domein (bijvoorbeeld ' contoso100.com') deze externe IP-adres waarnaar wordt verwezen. In ons voorbeeld moeten we de volgende DNS-vermelding maken:

    contoso100.com  -> 52.165.38.113

Dat is het: u bent nu klaar verbinding maken met de beheerde domein gebruiken, veilige LDAP via internet.

> [AZURE.WARNING] Vergeet niet dat de uitgever van het certificaat leesbaar TEKSTFORMAAT om te kunnen succes verbinden met de beheerde domein met leesbaar TEKSTFORMAAT moeten worden vertrouwd door clientcomputers. Als u een ondernemingscertificeringsinstantie of een openbaar vertrouwde certificeringsinstantie gebruikt, hoeft u niet te doen omdat deze certificate authority clientcomputers worden vertrouwd. Als u een zelfondertekend certificaat gebruikt, moet u het openbare deel van het zelfondertekend certificaat in het archief vertrouwd certificaat op de clientcomputer installeren.

<br>

## <a name="related-content"></a>Verwante onderwerpen

- [Een beheerde domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
