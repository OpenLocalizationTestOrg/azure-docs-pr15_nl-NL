<properties
    pageTitle="Tomcat op een virtuele machine | Microsoft Azure"
    description="Deze zelfstudie resources die zijn gemaakt met het implementatiemodel klassieke gebruikt, en ziet u hoe u een Windows virtuele machine maken en configureer deze Apache Tomcat toepassingsserver wordt uitgevoerd."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Java-toepassingsserver uitvoeren op een virtuele machine die zijn gemaakt met het implementatiemodel klassieke

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Met Azure, kunt u een virtuele machine gebruiken om server mogelijk te maken. Als voorbeeld, kan een virtuele machine Azure waarop worden geconfigureerd Java-toepassingsserver, zoals Apache Tomcat hosten. Nadat u deze handleiding hebt voltooid, hebt u inzicht in hoe u een virtuele machine waarop Azure maken en configureren voor het uitvoeren van een Java-toepassingsserver.

U leert:

* Het maken van een virtuele machine met een Java Development Kit (JDK) al is geïnstalleerd.
* Hoe u extern aanmelden met uw virtuele machine.
* Hoe u een Java-toepassingsserver installeert op uw VM.
* Het maken van een eindpunt voor uw virtuele machine.
* Hoe u een poort openen in de firewall voor uw toepassingsserver.

Voor de toepassing van deze zelfstudie, wordt een toepassingsserver Apache Tomcat op een virtuele machine zijn geïnstalleerd. De voltooide installatie resulteert in een installatie Tomcat zoals de volgende.

![VM Apache Tomcat uitgevoerd][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Een virtuele machine maken

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2. Klik op **Nieuw**op **berekenen**, klikt u op **VM**, en klik vervolgens op **Uit de galerie**.
3. Selecteer in het dialoogvenster **VM afbeelding selecteren** **JDK 7 Windows Server 2012**.
Houd er rekening mee dat **JDK 6 Windows Server 2012** beschikbaar is als u oudere toepassingen die niet kunnen worden uitgevoerd in JDK 7 hebt.
4. Klik op **volgende**.
5. Klik in het dialoogvenster **configuratie VM** :
    1. Geef een naam voor de virtuele machine.
    2. De grootte wilt gebruiken voor de virtuele machine opgeven.
    3. Voer een naam voor de beheerder in het veld **Gebruikersnaam in te voeren** . Onthoud dat deze naam en het wachtwoord dat u het volgende wilt invoeren, u kunt ze worden gebruikt wanneer u extern aanmelden bij de virtuele machine.
    4. Voer in het veld **Nieuw wachtwoord** een wachtwoord en voer deze opnieuw in het veld **bevestigen** . Dit is het accountwachtwoord beheerder.
    5. Klik op **volgende**.
6. Klik in het volgende dialoogvenster voor **de configuratie VM** :
    1. Voor **Cloudservice binnenkomt**, gebruikt u de standaard **maken een nieuwe cloudservice**.
    2. De waarde voor **de naam van de DNS-service Cloud** moet over cloudapp.net uniek zijn. Indien nodig, wordt deze waarde wijzigen zodat deze Azure wordt aangegeven dat deze uniek is.
    2. Geef een regio, affiniteit groep of virtuele netwerk. Voor de toepassing van deze zelfstudie, Geef een gebied zoals **West VS**.
    2. Selecteer **een account automatisch gegenereerde opslag gebruiken**voor **Opslag-Account**.
    3. Voor **Beschikbaarheid instellen**, selecteert u **(geen)**.
    4. Klik op **volgende**.
7. In het uiteindelijke in het dialoogvenster **configuratie VM** :
    1. Accepteer de standaard-eindpunt-vermeldingen.
    2. Klik op **Voltooien**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Extern aanmelden bij uw VM

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2. Klik op **virtuele machines**.
3. Klik op de naam van de virtuele machine die u aanmelden wilt bij.
4. Nadat de virtuele machine is gestart, kan een pop-upmenu onder aan de pagina verbindingen.
5. Klik op **verbinding maken**.
6. Reageer op de aanwijzingen naar wens verbinding maken met de virtuele machine. Dit moet voorzien in het RDP-bestand met de verbindingsgegevens kunt openen of opslaan. Mogelijk moet u de url: poort als het laatste onderdeel van de eerste regel van het RDP-bestand kopiëren en plak deze in een externe toepassing aanmelden.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Java-toepassingsserver installeren op uw VM

U kunt een Java-toepassingsserver kopiëren naar uw virtuele computer of u kunt een Java-toepassingsserver via een installatieprogramma installeren.

Voor de toepassing van deze zelfstudie, wordt Tomcat geïnstalleerd.

1. Wanneer u bent aangemeld bij uw VM, een browsersessie naar [Apache Tomcat](http://tomcat.apache.org/download-70.cgi)openen.
2. Dubbelklik op de koppeling voor **32-bits/64-bits Windows Service Installer**. Met behulp van deze techniek wordt Tomcat geïnstalleerd als een Windows-service.
3. Wanneer u wordt gevraagd, kiest u naar het installatieprogramma uitvoert.
4. Volg de aanwijzingen voor het installeren van Tomcat binnen de installatiewizard voor **Apache Tomcat** . Voor de toepassing van deze zelfstudie is het geen probleem accepteer telkens de standaardinstellingen. Wanneer u het dialoogvenster **de wizard Apache Tomcat Setup** hebt bereikt, kunt u desgewenst **Apache Tomcat uitvoeren** als u wilt dat Tomcat nu starten controleren. Klik op **Voltooien** om het installatieproces Tomcat.

## <a name="to-start-tomcat"></a>Tomcat starten
Als u geen Tomcat uitvoeren in het dialoogvenster **de wizard Apache Tomcat Setup** hebt gekozen, kunt u deze door een opdrachtprompt op uw virtuele computer openen en uit te voeren **net starten Tomcat7**starten.

U ziet nu Tomcat uitgevoerd als u de VM browser uitvoeren en <http://localhost:8080>opent.

Als u wilt zien Tomcat uitgevoerd vanaf externe computers, moet u een eindpunt maken en een poort openen.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Een eindpunt voor uw virtuele machine maken
1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2. Klik op **virtuele machines**.
3. Klik op de naam van de virtuele machine die uw Java-toepassingsserver wordt uitgevoerd.
4. Klik op de **eindpunten**.
5. Klik op **toevoegen**.
6. Zorg ervoor **toevoegen zelfstandige eindpunt** is geselecteerd en klik vervolgens op **volgende**in het dialoogvenster **toevoegen eindpunt** .
7. Klik in het dialoogvenster **eindpuntdetails van nieuwe** :
    1. Geef een naam voor het eindpunt; bijvoorbeeld: **HttpIn**.
    2. **TCP** opgeven voor het protocol.
    3. Geef **80** voor de openbare poort.
    4. Geef **8080** voor de poort privé.
    5. Klik op de knop **voltooid** om het dialoogvenster te sluiten. Uw eindpunt wordt gemaakt.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Een poort openen in de firewall voor uw VM
1. Meld u aan bij uw VM.
2. Klik op **Start van Windows**.
3. Klik op **Configuratiescherm**.
4. Klik op **systeem en beveiliging**op **Windows Firewall**en klik vervolgens op **Geavanceerde instellingen**.
5. Klik op **Regels voor binnenkomende verbindingen**en klik vervolgens op **Nieuwe regel**.
 ![Nieuwe binnenkomende regel][NewIBRule]
6. Selecteer **poort**voor het **Type regel**, en klik vervolgens op **volgende**.
 ![Nieuwe regel voor inkomende poort][NewRulePort]
7. Op het scherm **protocollen en poorten** , selecteert u **TCP**, **8080** opgeven als de **specifieke lokale poort**en klik vervolgens op **volgende**.
 ![Nieuwe binnenkomende regel][NewRuleProtocol]
8. Klik op het scherm **actie** selecteert u **toestaan de verbinding**en klik vervolgens op **volgende**.
 ![Nieuwe binnenkomende regelactie][NewRuleAction]
9. Zorg ervoor dat **domein**, **persoonlijke**en **openbare** zijn geselecteerd en klik vervolgens op **volgende**op het scherm **profiel** .
 ![Nieuwe binnenkomende regel-profiel][NewRuleProfile]
10. Klik op het scherm **naam** een naam voor de regel, zoals **HttpIn** (naam van de regel is niet vereist echter zodat deze overeenkomt met de naam van de eindpunt), en klik op **Voltooien**.  
 ![Nieuwe binnenkomende regelnaam][NewRuleName]

Nu uw website Tomcat weergegeven op een externe browser met behulp van een URL van het formulier moet worden * *http://*uw\_DNS\_naam*. cloudapp.net**waarbij ** *uw\_DNS\_naam*** de DNS-naam die u hebt opgegeven tijdens het maken van de virtuele machine.

## <a name="application-lifecycle-considerations"></a>Aandachtspunten voor de levenscyclus van toepassing
* U kunt uw eigen web application archief (WAR) maken en toe te voegen aan de map **webapps** . Bijvoorbeeld een eenvoudige Java Service pagina (JSP) dynamische webproject maken en deze te exporteren als een WAR-bestand, de WAR kopiëren naar de map Apache Tomcat **webapps** op de virtuele machine, voert u deze in een browser.
* Standaard wanneer de Tomcat-service is geïnstalleerd, is dit ingesteld op de werkstroom handmatig starten. U kunt deze vervolgens automatisch wordt gestart met behulp van de Services-module schakelen. Start de Services-module door te klikken op **Start van Windows**, **Beheerprogramma's**en vervolgens **Services**. Dubbelklik op de service **Apache Tomcat** en **Opstarttype** instellen op **automatisch**.

    ![Instellen van een service automatisch wordt gestart][service_automatic_startup]

    Het voordeel van Tomcat start automatisch te laten is dat deze wordt gestart als de virtuele machine opnieuw is opgestart (bijvoorbeeld nadat software-updates die opgevolgd opnieuw moeten worden geïnstalleerd).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over andere services (zoals Azure opslag, service bus en SQL-Database) die u opnemen met uw Java-toepassingen wilt door te bekijken van de beschikbare gegevens in het [Java Developer Center](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png
