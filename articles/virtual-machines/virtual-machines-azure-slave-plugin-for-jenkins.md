<properties
    pageTitle="Het gebruik van de invoegtoepassing Azure slave met continue integratie Jenkins | Microsoft Azure"
    description="Wordt beschreven hoe u de invoegtoepassing Azure slave met continue Jenkins-integratie."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>Het gebruik van de invoegtoepassing Azure slave met continue Jenkins-integratie

U kunt de invoegtoepassing Azure slave voor Jenkins inrichten slave knooppunten op Azure bij het uitvoeren van gedistribueerde genereert.

## <a name="install-the-azure-slave-plug-in"></a>De invoegtoepassing Azure slave installeren

1. Klik in het dashboard Jenkins op **Jenkins beheren**.

1. Klik op de pagina **Jenkins beheren** op **Invoegtoepassingen beheren**.

1. Klik op het tabblad **beschikbaar** .

1. Typ in het filterveld boven aan de lijst met beschikbare invoegtoepassingen **Azure** om de lijst beperken tot relevante plug-ins.

    Als u ook voor kiezen om Blader door de lijst beschikbare invoegtoepassingen, vindt u de Azure slave invoegtoepassing onder de sectie **beheer van de Cluster en Distributed maken** .

1. Schakel het selectievakje **Azure Slave-invoegtoepassing** .

1. Klik op **installeren zonder opnieuw starten** of **nu downloaden en installeren na opnieuw starten**.

Nu dat de invoegtoepassing is geïnstalleerd, zijn de volgende stappen voor het configureren van de invoegtoepassing te installeren met uw profiel Azure-abonnement en maken van een sjabloon die wordt gebruikt in de virtuele machine voor het knooppunt slave maken.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>De invoegtoepassing Azure slave configureren met het profiel van uw abonnement

Een profiel abonnement, ook bekend als publicatie-instellingen, is een XML-bestand met secure referenties en enkele aanvullende informatie die u moet samenwerken met Azure in uw ontwikkelomgeving. Als u wilt configureren de invoegtoepassing Azure slave, hebt u het volgende nodig:

* Uw abonnements-id
* Een certificaat management voor uw abonnement

Deze kunnen worden gevonden in uw [abonnement profiel]. Hieronder volgt een voorbeeld van een profiel abonnement.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Nadat u het profiel van uw abonnement hebt, volgt u deze stappen om de invoegtoepassing Azure slave configureren:

1. Klik in het dashboard Jenkins op **Jenkins beheren**.

1. Klik op **systeem configureren**.

1. Schuif omlaag op de pagina om te vinden van de sectie **Cloud** .

1. Klik op **nieuwe cloud toevoegen > Microsoft Azure**.

    ![sectie van de cloud][cloud section]

    Hiermee wordt de velden weergegeven waar u moet opgeven van de details van uw abonnement.

    ![configuratie van abonnement][subscription configuration]

1. Kopieer de abonnements-id en management certificaat waarden uit uw abonnement profiel en plak deze in de betreffende velden.

    Wanneer u kopieert het abonnement-id en beheer certificaat, neem niet de aanhalingstekens rond de waarden.

1. Klik op **configuratie controleren**.

1. Wanneer de configuratie correct is geverifieerd, klikt u op **Opslaan**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Een sjabloon virtuele machine instellen voor de Azure slave invoegtoepassing

Een sjabloon VM definieert de parameters dat de invoegtoepassing gebruiken wilt voor het maken van een knooppunt slave op Azure. In de volgende stappen wordt we een sjabloon voor een Ubuntu virtuele machine maken.

1. Klik in het dashboard Jenkins op **Jenkins beheren**.

1. Klik op **systeem configureren**.

1. Schuif omlaag op de pagina om te vinden van de sectie **Cloud** .

1. Klik in de sectie **Cloud** **Toevoegen Azure virtuele machines sjabloon**zoeken en klik vervolgens op **toevoegen**.

    ![vm sjabloon toevoegen][add vm template]

    Hier ziet de velden waarin u meer informatie over de sjabloon die u maakt invoeren.

    ![lege algemene configuratie][blank general configuration]

1. Voer in het vak **naam** de naam van een Azure cloud-service. Als de naam die u hebt ingevoerd naar een bestaande cloudservice verwijst, wordt de virtuele machine in die service deze is ingericht. Azure wordt anders maakt u een nieuwe record.

1. Voer in het vak **Beschrijving** beschrijving van de sjabloon die u maakt. Dit geldt alleen voor uw records en wordt niet gebruikt in de inrichting van een virtuele machine.

1. Het vak **Labels** wordt gebruikt om aan te geven van de sjabloon die u maakt en vervolgens wordt gebruikt om te verwijzen naar de sjabloon bij het maken van een taak Jenkins. Voer voor ons doel, **linux** in dit vak.

1. Klik op het gebied waar de virtuele machine worden gemaakt in de lijst **regio** .

1. Klik op de juiste grootte in de lijst **VM grootte** .

1. Geef in het vak **Opslagaccountnaam** een opslag-account waar de virtuele machine worden gemaakt. Zorg ervoor dat dit is in hetzelfde gebied, als de cloudservice die u wilt gebruiken. Desgewenst kunt u nieuwe opslag moet worden gemaakt, kunt u dit vak leeg laat.

1. Bewaarbeleid tijd Hiermee geeft u het aantal minuten voordat Jenkins Hiermee verwijdert u een niet-actieve slave. Laten staan de standaardwaarde van 60. U kunt er ook voor kiezen de slave in plaats van deze te verwijderen als deze niet actief is afgesloten. Schakel het selectievakje **Alleen-afsluiten (niet verwijderd) na het bewaarbeleid tijd** om dat te doen.

1. Klik in de lijst **Gebruik** , klikt u op de juiste voorwaarde wanneer dit knooppunt slave wordt gebruikt. Klik op **Utilize dit knooppunt zo veel mogelijk**op dit moment.

    Het formulier ziet er nu enigszins ongeveer als volgt:

    ![controlepunt algemene sjabloon zoekconfiguratie][checkpoint general template config]

    De volgende stap is vindt u informatie over het besturingssysteem-afbeelding die u wilt dat uw slave worden gemaakt in.

1. In het vak **afbeelding familie of -Id** die u moet opgeven welke afbeelding systeem wordt geïnstalleerd op uw virtuele machine. U kunt selecteren uit een lijst met afbeelding gezinnen of een aangepaste afbeelding opgeven.

    Als u selecteren uit een lijst met afbeelding gezinnen wilt, voert u het eerste teken (hoofdlettergevoelig) van de achternaam van de afbeelding. Bijvoorbeeld, krijgt typt **U** een lijst met Ubuntu Server gezinnen. Nadat u hebt geselecteerd in de lijst, gebruik Jenkins de nieuwste versie van die afbeelding systeem van die familie bij de inrichting van uw virtuele computer.

    ![Afbeelding van de OS lijst steekproef][OS Image list sample]

    Als u een aangepaste afbeelding die u wilt gebruiken in plaats daarvan hebt, voert u de naam van die aangepaste afbeelding. Afbeelding van de aangepaste namen worden niet weergegeven in een lijst, zodat u hoeft om ervoor te zorgen dat de naam correct is ingevoerd.

    Voor deze zelfstudie, typ **U** om een lijst met Ubuntu afbeeldingen weer te geven en klik vervolgens op **Ubuntu Server 14.04 LTS**.

1. Klik in de lijst **Starten methode** op **SSH**.

1. De onderstaande script kopiëren en plak deze in het vak **Initialisatie Script** .

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

    Het script initialisatie wordt uitgevoerd nadat de virtuele machine is gemaakt. In dit voorbeeld is het script Java, cijfer en ant geïnstalleerd.

1. Voer in de vakken **gebruikersnaam** en **wachtwoord** voor uw voorkeur waarden in voor het beheerdersaccount die op uw computer virtuele worden gemaakt.

1. Klik op **Verifiëren sjabloon** om te controleren of de parameters die u hebt opgegeven geldig zijn.

1. Klik op **Opslaan**.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Een taak Jenkins die wordt uitgevoerd op een knooppunt slave op Azure maken

In dit gedeelte maakt u een taak Jenkins die kan worden uitgevoerd op een knooppunt slave op Azure. U moet uw eigen project omhoog op GitHub erbij hebt.

1. Klik op **Nieuw Item**in het dashboard Jenkins.

1. Voer een naam voor de taak die u maakt.

1. Klik op **Freestyle project**voor het projecttype.

1. Klik op **Ok**.

1. Selecteer in de taakpagina **beperken waar dit project kan worden uitgevoerd**.

1. Voer in het vak **Label expressie** **linux**. We hebben gemaakt een slave sjabloon dat we **linux heet**, wat geven we hier in de vorige sectie.

1. In de sectie **maken** , klikt u op **toevoegen stap maken** en selecteer **Execute shell**.

1. Het volgende script, vervangen **(GitHub naam van uw account)**, **(uw projectnaam)**en **(het telefoonboek van uw project)** met de juiste waarden, bewerken en plak de bewerkte script in het tekstgebied dat wordt weergegeven.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Klik op **Opslaan**.

1. In het dashboard Jenkins, plaats de muisaanwijzer op de taak die u zojuist hebt gemaakt en klik op de pijl-omlaag om weer te geven Taakopties.

1. Klik op **nu samenstellen**.

Jenkins vervolgens slave knooppunt maken met behulp van de sjabloon in de vorige sectie hebt gemaakt en het script die u hebt opgegeven in de stap opbouwen voor deze taak uitvoeren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[abonnement profiel]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png