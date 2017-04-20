<properties
    pageTitle="Het gebruik van de invoegtoepassing Azure slave met continue integratie Hudson | Microsoft Azure"
    description="Wordt beschreven hoe u de invoegtoepassing Azure slave met continue Hudson-integratie."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Het gebruik van de invoegtoepassing Azure slave met continue Hudson-integratie

De invoegtoepassing voor Hudson Azure slave kunt u voor het inrichten van slave knooppunten op Azure bij het uitvoeren van gedistribueerde genereert.

## <a name="install-the-azure-slave-plug-in"></a>De invoegtoepassing Azure-Slave installeren

1. Klik in het dashboard Hudson op **Hudson beheren**.

1. Klik op de pagina **Hudson beheren** op **Invoegtoepassingen beheren**.

1. Klik op het tabblad **beschikbaar** .

1. Klik op **Zoeken** en typ **Azure** om de lijst beperken tot relevante plug-ins.

    Als u ook voor kiezen om Blader door de lijst beschikbare invoegtoepassingen, vindt u de Azure slave invoegtoepassing onder de sectie **beheer van de Cluster en Distributed maken** in het tabblad **anderen** .

1. Schakel het selectievakje voor **Azure Slave Plug-in**.

1. Klik op **installeren**.

1. Start opnieuw Hudson.

Nu dat de invoegtoepassing is geïnstalleerd, worden de volgende stappen voor het configureren van de invoegtoepassing te installeren met uw profiel Azure-abonnement en een sjabloon te maken die wordt gebruikt bij het maken van de VM voor het knooppunt slave.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>De invoegtoepassing Azure-Slave configureren met het profiel van uw abonnement

Een profiel abonnement, ook bekend als publicatie-instellingen, is een XML-bestand met secure referenties en enkele aanvullende informatie die u moet samenwerken met Azure in uw ontwikkelomgeving. Als u wilt configureren de invoegtoepassing Azure slave, hebt u het volgende nodig:

* Uw abonnements-id
* Een certificaat management voor uw abonnement

Deze kunnen worden gevonden in uw [abonnement profiel]. Hieronder volgt een voorbeeld van een profiel abonnement.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Nadat u uw profiel abonnement hebt, volgt u deze stappen om de invoegtoepassing Azure slave configureren.

1. Klik in het dashboard Hudson op **Hudson beheren**.

1. Klik op **systeem configureren**.

1. Schuif omlaag op de pagina om te vinden van de sectie **Cloud** .

1. Klik op **nieuwe cloud toevoegen > Microsoft Azure**.

    ![nieuwe cloud toevoegen][add new cloud]

    Hiermee wordt de velden weergegeven waar u moet opgeven van de details van uw abonnement.

    ![profiel configureren][configure profile]

1. Het abonnement-id en beheer certificaat uit uw abonnement profiel Kopieer en plak deze in de betreffende velden.

    Wanneer u kopieert het abonnement-id en beheer certificaat, bevatten **niet** de aanhalingstekens rond de waarden.

1. Klik op **verifiëren configuratie**.

1. Wanneer de configuratie is is geverifieerd, klikt u op **Opslaan**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Een sjabloon virtuele machine instellen voor de Slave Azure invoegtoepassing

Een sjabloon VM definieert de parameters die de invoegtoepassing gebruikt om u te maken van een knooppunt slave op Azure. In de volgende stappen maakt we sjabloon voor een Ubuntu VM.

1. Klik in het dashboard Hudson op **Hudson beheren**.

1. Klik op **systeem configureren**.

1. Schuif omlaag op de pagina om te vinden van de sectie **Cloud** .

1. In de sectie **Cloud** **Toevoegen Azure virtuele machines sjabloon** zoeken en klik op de knop **toevoegen** .

    ![vm sjabloon toevoegen][add vm template]

1. Geef de naam van een cloud-service in het veld **naam** . Als de naam die u opgeeft naar een bestaande cloudservice verwijst, wordt de VM in die service deze is ingericht. Azure wordt anders maakt u een nieuwe record.

1. Voer in het veld **Beschrijving** beschrijving van de sjabloon die u maakt. Deze informatie geldt alleen voor documenten doeleinden en wordt niet gebruikt in een VM inrichten.

1. Voer in het veld **etiketten** **linux**. Dit label wordt gebruikt om aan te geven van de sjabloon die u maakt en vervolgens wordt gebruikt om te verwijzen naar de sjabloon bij het maken van een taak Hudson.

1. Selecteer een gebied waar de VM worden gemaakt.

1. Selecteer de juiste VM grootte.

1. Geef een opslag-account waar de VM worden gemaakt. Zorg ervoor dat dit is in hetzelfde gebied, als de cloudservice die u wilt gebruiken. Als u wilt dat nieuwe opslag moet worden gemaakt, laat u dit veld leeg.

1. Bewaarbeleid tijd Hiermee geeft u het aantal minuten voordat Hudson Hiermee verwijdert u een niet-actieve slave. Laten staan de standaardwaarde van 60.

1. Selecteer in **Gebruik**de juiste voorwaarde wanneer dit knooppunt slave wordt gebruikt. Selecteer nu **Utilize dit knooppunt zo veel mogelijk**.

    Nu er uw formulier enigszins ongeveer als volgt:

    ![sjabloon zoekconfiguratie][template config]

1. In de **afbeelding familie of -Id** die u moet opgeven welke afbeelding systeem wordt geïnstalleerd op uw VM. U kunt selecteren uit een lijst met afbeelding gezinnen of een aangepaste afbeelding opgeven.

    Als u selecteren uit een lijst met afbeelding gezinnen wilt, voert u het eerste teken (hoofdlettergevoelig) van de achternaam van de afbeelding. Bijvoorbeeld, krijgt typt **U** een lijst met Ubuntu Server gezinnen. Wanneer u in de lijst hebt geselecteerd, wordt Jenkins de nieuwste versie van die afbeelding systeem van die familie gebruiken bij de inrichting van uw VM.

    ![Lijst met OS uw gezin][OS family list]

    Als u een aangepaste afbeelding die u wilt gebruiken in plaats daarvan hebt, voert u de naam van die aangepaste afbeelding. Afbeelding van de aangepaste namen worden niet weergegeven in een lijst, zodat u hoeft om ervoor te zorgen dat de naam correct is ingevoerd.    

    Voor deze zelfstudie, typt **U** een lijst met Ubuntu afbeeldingen en klik op **Ubuntu Server 14.04 LTS**.

1. Selecteer **SSH**voor het **starten van de methode**.

1. De onderstaande script kopiëren en plakken in het veld **initialisatie script** .

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    Het **script initialisatie** wordt uitgevoerd nadat de VM is gemaakt. In dit voorbeeld is het script Java, cijfer en ant geïnstalleerd.

1. Voer in de velden **gebruikersnaam** en **wachtwoord** voor uw voorkeur waarden voor het beheerdersaccount die op uw VM worden gemaakt.

1. Klik op de **Sjabloon controleren** om te controleren of de parameters die u hebt opgegeven geldig zijn.

1. Klik op **Opslaan**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Een taak Hudson die wordt uitgevoerd op een knooppunt slave op Azure maken

In dit gedeelte maakt u een taak Hudson die kan worden uitgevoerd op een knooppunt slave op Azure.

1. Klik in het dashboard Hudson op **Nieuwe taak**.

1. Voer een naam voor de taak die u maakt.

1. Selecteer **een software vrij te geven stijl van een taak maken**voor het taaktype.

1. Klik op **OK**.

1. Selecteer in de pagina taak **beperken waar dit project kan worden uitgevoerd**.

1. Selecteer **knooppunt en label menu** en selecteer **linux** (we opgegeven dit label bij het maken van de sjabloon VM in de vorige sectie).

1. In de sectie **maken** , klikt u op **toevoegen stap maken** en selecteer **Execute shell**.

1. Het volgende script, **{github naam van uw account}**, **{de naam van uw project}**en **{het telefoonboek van uw project}** vervangen met de juiste waarden, bewerken en plak de bewerkte script in het tekstgebied dat wordt weergegeven.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Klik op **Opslaan**.

1. Zoek de taak die u zojuist hebt gemaakt en klik op het pictogram **planning een opbouwen** in het dashboard Hudson.

Hudson vervolgens maakt u een slave knooppunt met de sjabloon in de vorige sectie hebt gemaakt en het script die u hebt opgegeven in de stap opbouwen voor deze taak uitvoeren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[abonnement profiel]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

