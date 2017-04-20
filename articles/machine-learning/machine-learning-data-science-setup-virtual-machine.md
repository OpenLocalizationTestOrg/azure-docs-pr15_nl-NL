<properties
    pageTitle="Een virtuele machine als een server IPython notitieblok instellen | Microsoft Azure"
    description="Instellen van een Azure virtuele Machine voor gebruik in een omgeving met gegevens wetenschappelijk met IPython-Server voor geavanceerde analyses."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Een Azure virtuele machine instellen als een server IPython notitieblok voor geavanceerde analyses

Dit onderwerp wordt uitgelegd hoe u inrichten en een Azure virtuele machines configureren voor geavanceerde analyses die kan worden gebruikt als onderdeel van een wetenschappelijke-omgeving van gegevens. Het virtuele Windows-computer is geconfigureerd met ondersteunende hulpprogramma's zoals zoals IPython notitieblok, Azure opslag Explorer, AzCopy, evenals andere hulpprogramma's die handig voor geavanceerde analyses projecten zijn. Azure opslag Explorer en AzCopy, bijvoorbeeld handige manieren om gegevens te uploaden naar Azure-blobopslag van uw lokale computer of om deze te downloaden naar uw lokale computer uit blobopslag te bieden.

## <a name="create-vm"></a>Stap 1: Maak een algemene Azure virtuele machines

Als u al een Azure virtuele machines en alleen wilt instellen op een server IPython notitieblok op is geïnstalleerd, kunt u deze stap overslaan en ga verder naar [stap 2: een eindpunt voor IPython notitieblokken toevoegen aan een bestaande VM](#add-endpoint).

Voordat u begint het proces van het maken van een virtuele machine op Azure, moet u het formaat van de computer die nodig is om de gegevens voor hun project te verwerken. Kleinere machines hebben minder geheugen en minder CPU cores dan grotere machines, maar deze zijn ook minder dure. Zie de pagina <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtuele Machines prijzen</a> voor een lijst van machine typen en prijzen

1. Meld u aan bij <a href="https://manage.windowsazure.com" target="_blank">Klassieke Azure-Portal</a>en klikt u op **Nieuw** in de linkerbenedenhoek. Een venster wordt weergegeven. Selecteer **berekenen** -> **VM** -> **FROM-GALERIE**.

    ![Werkruimte maken][24]

2. Kies een van de volgende afbeeldingen:

    * Windows Server 2012 R2 Datacenter
    * Windows Server Essentials ervaring (Windows Server 2012 R2)

    Klik vervolgens op de pijl naar rechts in de rechterbenedenhoek om te gaan van de volgende configuratiepagina.

    ![Werkruimte maken][25]

3. Voer een naam voor de virtuele machine die u wilt maken, selecteert u de grootte van de computer (standaard: A3) op basis van de grootte van de gegevens die de machine te proces dient en hoe krachtige u wilt dat de computer (de grootte van geheugen en het aantal berekeningscluster kernen), voert u een gebruikersnaam en wachtwoord voor de computer. Klik vervolgens op de pijl naar rechts om te gaan naar de volgende configuratiepagina.

    ![Werkruimte maken][26]

4. Selecteer het **Land/AFFINITEIT groep/virtuele netwerk** waarin het **Opslag-ACCOUNT** dat u wilt gebruiken voor deze virtuele machine en selecteer dat opslag-account. Een eindpunt onderaan in het veld **EINDPUNTEN** toevoegen door op te geven van de naam van het eindpunt ("IPython" hier). Als de **naam** van het eindpunt en een geheel getal tussen 0 en 65536 die is **beschikbaar** als de **Openbare poort**kunt u een willekeurige tekenreeks. De **Privé poort** heeft moeten **9999**. Gebruikers moeten **voorkomen** met behulp van openbare poorten die al zijn toegewezen voor internetservices. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Poorten voor Internet-Services</a> vindt u een lijst met poorten die zijn toegewezen en te voorkomen.

    ![Werkruimte maken][27]

5. Klik op het vinkje om te beginnen de virtuele machine inrichting proces.

    ![Werkruimte maken][28]


Duurt 15-25 minuten de virtuele machine inrichting van proces. Nadat de virtuele machine is gemaakt, moet de status van deze computer worden **uitgevoerd**weergegeven.

![Werkruimte maken][29]

## <a name="add-endpoint"></a>Stap 2: Een eindpunt voor IPython notitieblokken toevoegen aan een bestaande VM

Als u de virtuele machine volgens de instructies in stap 1 hebt gemaakt, klikt u vervolgens het eindpunt voor IPython notitieblok al is toegevoegd en wordt deze stap worden overgeslagen.

Als de virtuele machine al bestaat, moet u een eindpunt voor IPython notitieblok dat u in stap 3 onder eerste log in de klassieke Portal Azure installeren wilt toevoegen, selecteer de virtuele machine en het eindpunt voor IPython notitieblok server toevoegen. De volgende afbeelding bevat een schermafbeelding van de portal nadat het eindpunt voor IPython notitieblok is toegevoegd aan een virtuele Windows-computer.

![Werkruimte maken][17]

## <a name="run-commands"></a>Stap 3: IPython notitieblok en andere ondersteunende's installeren

Nadat de virtuele machine is gemaakt, gebruikt u Remote Desktop Protocol (RDP) aanmelden bij de virtuele Windows-computer. Zie voor instructies [hoe u aanmelden bij een virtuele Machine uitvoeren Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). Open de **opdrachtprompt** (**niet de Powershell-opdrachtvenster**) als een **beheerder** en voer de volgende opdracht.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Wanneer de installatie is voltooid, de server IPython notitieblok in automatisch wordt gestart de *C:\\gebruikers\\\<gebruikersnaam in te voeren\>\\documenten\\IPython notitieblokken* directory.

Wanneer u wordt gevraagd, voert u een wachtwoord voor het notitieblok IPython en het wachtwoord van de beheerder van de computer. Hiermee worden de IPython notitieblok dat u wilt uitvoeren als een service die u op de computer.

## <a name="access"></a>Stap 4: Access IPython notitieblokken vanuit een webbrowser
De server IPython notitieblok, opent u een web browser en invoer *computernaam voor DNS-https://&#60;virtual >: & #60; openbare poortnummer >* in het tekstvak URL. Hier de *& #60; openbare poortnummer >* moet het poortnummer die u hebt opgegeven waarop het eindpunt IPython notitieblok is toegevoegd.

De *& #60; VM DNS-naam >* vindt u in de klassieke Portal van Azure. Nadat de logboekregistratie in bij de klassieke-Portal op **virtuele MACHINES**, selecteert u de computer die u hebt gemaakt en selecteer vervolgens **DASHBOARD**, de naam van de DNS-wordt als volgt weergegeven:

![Werkruimte maken][19]

U ontvangt een waarschuwing waarin wordt gemeld dat _er een probleem is met het beveiligingscertificaat van deze website_ (Internet Explorer) of _de verbinding niet privé is_ (Chrome), zoals wordt weergegeven in de volgende cijfers. Klik op **Doorgaan om deze website (niet aanbevolen)** (Internet Explorer) of **Geavanceerd** en kiest u vervolgens * *Doorgaan naar & #60;* De naam van de DNS-*> (onveilig) ** (Chrome) om door te gaan. Het wachtwoord dat u eerder hebt opgegeven voor toegang tot de notitieblokken IPython vervolgens worden ingevoerd.

**Internet Explorer:**
![-werkruimte maken][20]

**Chrome:**
![-werkruimte maken][21]

Wanneer u zich bij het notitieblok IPython aanmeldt, een map *DataScienceSamples* wordt weergegeven in de browser. Deze map bevat steekproef IPython notitieblokken die worden gedeeld door Microsoft om wetenschappelijke Gegevenstaken leiden gebruikers te helpen. Deze steekproef IPython notitieblokken zijn uitgecheckt uit [**Github opslagplaats**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) tot de virtuele machines tijdens de IPython notitieblok server proces instellen. Microsoft onderhoudt en deze opslagplaats regelmatig worden bijgewerkt. Gebruikers kunnen gaat u naar de bibliotheek Github om de meest recente steekproef IPython notitieblokken.
![Werkruimte maken][18]

## <a name="upload"></a>Stap 5: Een bestaand notitieblok van IPython van een lokale computer naar de server IPython notitieblok uploaden

IPython notitieblokken bieden een eenvoudige manier om gebruikers een bestaand notitieblok IPython op hun lokale computers uploaden naar de server IPython notitieblok op de virtuele machines. Nadat gebruikers zich bij het notitieblok IPython in een webbrowser aanmelden, klikt u op in de **map** die het notitieblok IPython om te worden geüpload. Selecteer vervolgens een IPython .ipynb notitieblokbestand voor het uploaden van de lokale computer in de **Verkenner**en slepen en neer te zetten naar de map IPython notitieblok op de webbrowser. Klik op de knop **uploaden** om het .ipynb-bestand uploaden naar de server IPython notitieblok. Andere gebruikers kunnen start deze in vanuit hun webbrowser gebruiken.

![Werkruimte maken][22]

![Werkruimte maken][23]


##<a name="shutdown"></a>Afsluiten en maak toewijzen VM wanneer u niet in gebruik

Azure virtuele Machines prijs als **betalen alleen voor wat u gebruikt**. Om ervoor te zorgen dat u bent niet die worden gefactureerd wanneer u uw VM niet gebruikt, hebt u er moet in de stand **gestopt (Deallocated)** wanneer u niet in gebruik.

> [AZURE.NOTE] Als u de virtuele machine uit binnen de VM afsluit (via power-opties voor Windows), de VM is gestopt maar blijft toegewezen. Om ervoor te zorgen u niet moet worden gefactureerd door, moet u altijd virtuele machines in de [Klassieke Azure-Portal](http://manage.windowsazure.com/)stoppen. U kunt ook de VM via Powershell stoppen door te bellen **ShutdownRoleOperation** met 'PostShutdownAction' gelijk is aan 'StoppedDeallocated'.

Afsluiten en de virtuele machine opheffen:

1. Meld u aan bij de [Klassieke Azure-Portal](http://manage.windowsazure.com/) met uw account.  

2. Selecteer **virtuele MACHINES** in de linkernavigatiebalk.

3. In de lijst met virtuele machines, klik op de naam van uw virtuele computer en Ga naar **de dashboardpagina** .

4. Klik onderaan op de pagina, op **Afsluiten**.

![VM afsluiten][15]

De virtuele machine wordt opgeheven maar deze niet verwijderd. U kunt uw VM opnieuw opstarten op elk gewenst moment in de klassieke Azure-Portal.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Uw VM Azure is klaar voor gebruik: Wat is het volgende?

Uw VM is nu klaar voor gebruik in uw gegevens wetenschappelijke oefeningen. De virtuele machine is ook klaar voor gebruik als een server IPython notitieblok voor het verkennen en de verwerking van gegevens en andere taken in combinatie met Azure Machine Learning en het Team gegevens wetenschap proces.

De volgende stappen in het Team gegevens wetenschap proces in de [Leerpad](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zijn toegewezen, en eventueel stappen die gegevens verplaatsen naar HDInsight, verwerken en probeer het voorbereiding op leren van de gegevens met Azure Machine Learning.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
